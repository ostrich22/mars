# Android更新項目
* sdk 封裝從jar改為aar。  
(大部分資源已包入aar內，除了mars_settings.xml)
* 更新儲值流程。
* 更新FB SDK至5.13。
* 新增修改密碼功能。
* 新增GPP獎勵認領功能。
(此功能只能在測試服測試，驗証無誤後才會推到正式)

# 移除舊版sdk
將ujmobilesdk這個資料夾整個刪除。

# 導入新版sdk
## 加入aar
將UJMobileSDK-1.0.aar放到app\libs。
## 加入mars settings
將mars_settings.xml放到app\src\main\res\values。
## 建立dependency
* gradle 範例
```
dependencies {
    ...
    implementation fileTree(include: ['*.aar'], dir: 'libs')
    ...
}
```
## 加入其它dependencies
* gradle 範例
```
dependencies {
    ...
    implementation 'com.google.code.gson:gson:2.7'
    implementation 'com.android.support:multidex:1.0.1'
    implementation 'com.facebook.android:facebook-android-sdk:5.13.0'
    implementation 'com.android.support:appcompat-v7:27.0.2'
    implementation 'com.google.android.gms:play-services-games:17.0.0'
    implementation 'com.google.android.gms:play-services-auth:16.0.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation 'com.android.support:design:27.0.2'
    implementation 'com.android.support:recyclerview-v7:27.0.2'
    implementation 'android.arch.lifecycle:extensions:1.1.1'
    implementation 'android.arch.lifecycle:runtime:1.1.1'
    ...
}
```
