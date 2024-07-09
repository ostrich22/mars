# HTK 4.3.0.2 Patch Note

## 版本差異
* 4.2.0.13 -> 4.3.0.2

## 重點注意事項

### Android
* target api需設為34
* minSdk改至24
* 使用Demo_Android\app\src\main\sharedres\values\mars_settings.xml覆蓋遊戲中的

#### 新增doMsgProcessAccountHesitationDeletionPeriod事件
```java
// 帳號處於後悔期
public void doMsgProcessAccountHesitationDeletionPeriod(String[] args) {
    // 顯示帳號目前狀態
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
```

#### 調整依賴函式庫
```diff
+implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
-implementation 'androidx.constraintlayout:constraintlayout:2.0.0'

+def lifeCycleVersion = "2.3.1"
-def lifeCycleVersion = "2.1.0"

+implementation "androidx.lifecycle:lifecycle-common:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-livedata:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-viewmodel:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-viewmodel-savedstate:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-process:$lifeCycleVersion"
-implementation "androidx.lifecycle:lifecycle-runtime:$lifeCycleVersion"
-implementation "androidx.lifecycle:lifecycle-extensions:$lifeCycleVersion"

+implementation "com.google.android.gms:play-services-games-v2:19.0.0"
-implementation "com.google.android.gms:play-services-games-v2:+"

+def billing_version = "6.2.1"
-def billing_version = "6.0.1"

+implementation 'com.facebook.android:facebook-android-sdk:17.0.0'
-implementation 'com.facebook.android:facebook-android-sdk:12.3.0'

+implementation 'com.google.android.gms:play-services-auth-api-phone:17.5.1'
```

### iOS
* 將Demo_iOS\Resources\mars_settings.xml覆蓋遊戲中的

#### Apple的上架政策：App更新需符合隱私聲明規範(包含第三方SDK)
* [隱私清單計畫正式公告](https://developer.apple.com/news/?id=3d8a9yyh)
* [News and Updates](https://developer.apple.com/news/upcoming-requirements/)
* [About Privacy manifest files](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api?language=objc)
* [Upcoming third-party SDK requirements](https://developer.apple.com/support/third-party-SDK-requirements/)  
APPLE列出來架上常用的SDK清單, 既然被列出清單內的SDK就必須要有隱私清單與SDK簽名
* 遊戲內所有使用的SDK都必須符合規範

#### 更換Framework&Bundle
* 請刪除以下的檔案再倒入4.3.0.2版demo中的Framework&Bundle
```
UJMobileSDKResources.bundle
AppAuth.framework
FBAEMKit.framework
FBSDKCoreKit.framework
FBSDKCoreKit_Basics.framework
FBSDKLoginKit.framework
FBSDKShareKit.framework
GoogleSignIn.framework
GTMAppAuth.framework
GTMSessionFetcher.framework
UJMobileSDK.framework
```
* 以下Framework加入專案後需改為Embed & Sign
```
AppAuth.framework
FBLPromises.framework
GoogleSignIn.framework
GoogleUtilities.framework
GTMAppAuth.framework
GTMSessionFetcher.framework
```

#### 刪除/復原帳號事件
* 相關事件請參考Demo_iOS中AppDelegate.m 292~319
* 以下三個事件必需呼叫[[ViewMgr Instance] ShowRestoreAccountDialog]
```objc
// ShowRestoreAccountDialog會提示玩家目前帳號的狀況，若帳號還在後悔期，該介面可以復原帳號
- (void)doMsgProcessAccountDeleted:(NSArray*) args {
    [[ViewMgr Instance] ShowRestoreAccountDialog];
}
- (void)doMsgProcessRequestDeleteAccountSuccess:(NSArray *)args {
    [[ViewMgr Instance] ShowRestoreAccountDialog];
}
- (void)doMsgProcessAccountHesitationDeletionPeriod:(NSArray *)args {
    [[ViewMgr Instance] ShowRestoreAccountDialog];
}
```

#### Google登入
* Info.plist需加入以下設定
```
	<key>CFBundleURLTypes</key>
	<array>
		<dict>
			<key>CFBundleTypeRole</key>
			<string>Editor</string>
			<key>CFBundleURLSchemes</key>
			<array>
				<string>com.googleusercontent.apps.94565543865-vugljcl59a42sjt3k7p8cale8lsvbut7</string>
			</array>
		</dict>
	</array>
	<key>GIDClientID</key>
	<string>94565543865-vugljcl59a42sjt3k7p8cale8lsvbut7.apps.googleusercontent.com</string>
	<key>GIDServerClientID</key>
	<string>94565543865-ihfdkc27rssu3s4aonb9127hiu2qe2nt.apps.googleusercontent.com</string>
```
