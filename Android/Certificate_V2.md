# V2 署名について

Android 7 以降は V2 署名が使える。  
https://source.android.com/security/apksigning/v2    

jarsigner ではなく apksigner を使うことになる。  

* apksigner  
Pathは Android SDK の ./sdk/build-tools/25.0.2/apksigner  
コマンド例は以下  
`apksigner  sign --ks ~/.android/debug.keystore --v1-signing-enabled true --v2-signing-enabled true Android.apk`
