# Zipファイルのエントリ変更

* Zip 仕様  
https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT

* したかったこと  
Android の署名は META-INF/ 以下のファイルを削除することで無効化できる。  
入力された APK から META-INF を削除した APK を出力する処理を Java で実装。  
加えて Android 4.0.3 以前の armeabi-v7a 端末は APK 中で lib/armeabi ディレクトリが  
lib/armeabi-v7a ディレクトリよりも後ろにあると lib/armeabi ディレクトリの .so が  
読み込まれるらしい。なのでエントリの順番を変える。  
→https://developer.android.com/ndk/guides/abis.html?hl=ja  
  
* 方法  
ZipInputStream で各エントリを周り、ZipOutputStream で必要な順でエントリを追加していく。 
  
* 実装  
```java:sample.java
class ZipOptimizer {
	private Path apkFile;
	
	public ZipOptimizer(Path apkFile) {
		this.apkFile = apkFile;
	}
	
	public void optimize() throws IOException {
		Pattern certificatePattern = Pattern.compile("^META-INF/.*");
		Pattern armeabiV7aPattern = Pattern.compile("^lib/armeabi-v7a/.*");
		Pattern armeabiPattern = Pattern.compile("^lib/armeabi/.*");
		
		// 一時出力用ファイルの作成.
		Path temporaryPath = getTemporaryApkFile();
		
		try (OutputStream outputStream = Files.newOutputStream(temporaryPath);
			ZipOutputStream zipOutputStream = new ZipOutputStream(outputStream)) {
			
			zipOutputStream.setLevel(BEST_COMPRESSION);
			// lib/armeabi-v7a/.* と lib/armeabi/.* と META-INF/.* 以外のエントリを追加する
			this.putZipEntiesWithoutDesignationEntries(this.apkFile, zipOutputStream, Arrays.asList(certificatePattern, armeabiPattern, armeabiV7aPattern));
			// lib/armeabi/.* 以下のエントリを追加する
			this.putZipEntriesWithDesignationEntries(this.apkFile, zipOutputStream, armeabiPattern);
			// lib/armeabi-v7a/.* 以下のエントリを追加する
			this.putZipEntriesWithDesignationEntries(this.apkFile, zipOutputStream, armeabiV7aPattern);
		}
		// エントリを置き換えた apk とオリジナルを置き換える。
		Files.move(temporaryPath, this.apkFile, StandardCopyOption.REPLACE_EXISTING);
	}
	
	private void putZipEntiesWithoutDesignationEntries(Path src, ZipOutputStream zipOutputStream, List<Pattern> patterns) throws IOException {
		try (FileInputStream fileInputStream = new FileInputStream(src.toString());
				ZipInputStream zipInputStream = new ZipInputStream(fileInputStream)) {
			
			ZipEntry inputZipEntry = null;
			while ((inputZipEntry = zipInputStream.getNextEntry()) != null){
				// 指定エントリ以外を書き出す
				if(!this.hasMatchWithEntryName(inputZipEntry.getName(), patterns)) {
					ZipEntry zipEntry = new ZipEntry(inputZipEntry);
					// 万が一、同じ圧縮レベルで作成された apk でない場合は
					// 圧縮サイズに違いがでるため、元のエントリのまま使うとエラーになるため空をセット.
					zipEntry.setCompressedSize(-1);
					this.writeZipEntry(zipEntry, zipInputStream, zipOutputStream);
				}
				zipInputStream.closeEntry();
			}
		}
	}

	private void putZipEntriesWithDesignationEntries(Path src, ZipOutputStream zipOutputStream, Pattern pattern) throws IOException {
		try (FileInputStream fileInputStream = new FileInputStream(src.toString());
				ZipInputStream zipInputStream = new ZipInputStream(fileInputStream)) {
			
			ZipEntry inputZipEntry = null;
			while ((inputZipEntry = zipInputStream.getNextEntry()) != null){
				// 指定エントリを書き出す
				if(this.hasMatchWithEntryName(inputZipEntry.getName(), Arrays.asList(pattern))) {
					ZipEntry zipEntry = new ZipEntry(inputZipEntry);
					// 万が一、同じ圧縮レベルで作成された apk でない場合は
					// 圧縮サイズに違いがでるため、元のエントリのまま使うとエラーになるため空をセット.
					zipEntry.setCompressedSize(-1);
					this.writeZipEntry(zipEntry, zipInputStream, zipOutputStream);
				}
				zipInputStream.closeEntry();
			}
		}
	}

	private boolean hasMatchWithEntryName(String entryName, List<Pattern> patterns) {
		Iterator<Pattern> iterator = patterns.iterator();
		while(iterator.hasNext()) {
			Pattern pattern = iterator.next();
			Matcher matcher =  pattern.matcher(entryName);
			if(matcher.find()) {
				return true;
			}
		}
		return false;
	}
	
	private void writeZipEntry(ZipEntry zipEntry, ZipInputStream zipInputStream, ZipOutputStream zipOutputStream) throws IOException {
		zipOutputStream.putNextEntry(zipEntry);
		byte[] readByte = new byte[1024];
		int length = 0;
		while ((length = zipInputStream.read(readByte, 0, 1024)) != -1) {
			zipOutputStream.write(readByte, 0, length);
		}
		zipOutputStream.closeEntry();
	}

	private Path getTemporaryApkFile () throws IOException {
		Path tempDir = Files.createTempDirectory(Paths.get(this.apkFile.getParent().toString()), ".");
		tempDir.toFile().deleteOnExit();
		final Path tempDir2 = tempDir;
		Runtime.getRuntime().addShutdownHook(new Thread() {
			@Override
			public void run() {
				// 強制終了時に一時フォルダを掃除.
				Dirs.deleteQuietly(tempDir2);
			}
		});
	
		// 対象の apk を処理用にいったんコピー.
		Path temp = Paths.get(tempDir.toString(), PathString.lastPathString(this.apkFile.toString()));
		Files.copy(this.apkFile, temp);
		
		return temp;
	}
}
```
 
* 注意点  
	* ZipEntry.getCompressedSize()  
元の Zip と作成する Zip の圧縮レベルが違えば当然上記の値も変わる。  
元 Zip から取り出した ZipEntry には上記のプロパティは記載されている。  
取り出したままで Entry を書き出そうとすると実際の書き出しバイト数と  
エントリに記載されている値が違うことで例外 ZipException がスローされる。  
対策として ZipEntry.getCompressedSize() は取り出せなければ -1 が変えるので  
これをセットしてから書き込むようにすれば万が一圧縮レベルにズレがあっても  
例外が発生することはない。  
  
* 結果  
エントリの順番も変わって、未署名状態の APK が作成できた。  
ただ、エントリの順番を変えても Android 4.0.3 がインストールされた armeabi-v7a 端末は  
lib/armeabi/ 以下の .so をロードしている。  
→proc/[PID]/~.so を dump して確認。  
対策としては以下しかないのだろうか。  
→https://groups.google.com/forum/#!msg/android-ndk/N8FLjvM81pg/2rYeClQZcckJ  
`I have a different approach. I put both arm and armv7 binaries into
the armeabi directory, and I then use the cpu-features library in
order to decide whether I load the optimized library or not, using
dlopen(). It works just great.` 
