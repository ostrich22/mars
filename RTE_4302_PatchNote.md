# RTE 4.3.0.2 更新項目

## 版本差異
* 4.1.0.1 -> 4.3.0.2

## 重點注意事項

### Android
* minSdk改至24

#### 調整刪除帳號規則
* 直接刪除 -> 執行後進入三天後悔期
* 後悔期期間可以復原帳號
* 三天後，由排程執行刪除帳號動作
* 新增刪除/復原帳號相關事件(請參考Demo_Android中MainActivity.java 767~798)
* 以下三個事件必須呼叫BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog()
```java
// ShowRestoreAccountDialog會提示玩家目前帳號的狀況，若帳號還在後悔期，該介面可以復原帳號
@Override
public void doMsgProcessAccountDeleted(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
@Override
public void doMsgProcessRequestDeleteAccountSuccess(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
@Override
public void doMsgProcessAccountHesitationDeletionPeriod(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
```

#### 調整依賴函式庫
```diff
+implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
-implementation 'androidx.constraintlayout:constraintlayout:2.0.0'

+implementation "androidx.lifecycle:lifecycle-common:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-livedata:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-viewmodel:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-viewmodel-savedstate:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-process:$lifeCycleVersion"
+implementation "androidx.lifecycle:lifecycle-common-java8:$lifeCycleVersion"
-implementation "androidx.lifecycle:lifecycle-runtime:2.1.0"
-implementation "androidx.lifecycle:lifecycle-extensions:2.1.0"

+def billing_version = "6.2.1"
+implementation "com.android.billingclient:billing:$billing_version"
-implementation 'com.android.billingclient:billing:5.1.0'

+implementation 'com.facebook.android:facebook-android-sdk:17.0.0'
-implementation 'com.facebook.android:facebook-android-sdk:12.3.0'

+implementation 'com.google.android.gms:play-services-auth-api-phone:17.5.1'
```

#### AndroidManifest.xml
* Facebook登入需要新增的設定
```diff
+<meta-data android:name="com.facebook.sdk.ClientToken" android:value="@string/facebook_client_token_gameapp"/>
+<queries>
+    <package android:name="com.facebook.katana" />
+</queries>
```

### iOS
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
#### AppDelegate.m
* 新版Facebook登入
```diff
 - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
+    return [[FBSDKApplicationDelegate sharedInstance] application:app openURL:url options:options];
-    return [GIDSignIn.sharedInstance handleURL:url];
}
```

#### 刪除/復原帳號功能
* 功能變更如同Android
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

#### 儲值功能API變更
* 初始化
```diff
NSString* serverID = self.Field_serverID.text;       // 伺服器編號
NSString* characterID = self.Field_characterID.text;    // 角色編號
NSString* characterName = self.Field_characterName.text;  // 角色名稱
+[[StoreKitPluginBridge Instance] InitBilling:0      // 國家代碼:0(全開放)
+                                    serverId:serverID
+                                 characterId:characterID
+                               characterName:characterName ];
-[[StoreKitPlugin Instance] Init:serverID :characterID :characterName ];
```
* 購買商品
```diff
NSString* designID = @"1";
NSString* info = @"defined by sdk user";
+[[StoreKitPluginBridge Instance] DoPurchase:designID extraInformation:info delay:true];
-[[StoreKitPlugin Instance] RequestUJOrder:designID:info:true];
```

* 詢問是否有可領取的訂單
```diff
+[[StoreKitPluginBridge Instance] RequestUJOrderList];
-[[StoreKitPlugin Instance] RequestUJOrderList];
```

#### Facebook登入
* Info.plist
```diff
+	<key>FacebookClientToken</key>
+	<string>0699a7447367e0b2ec986cdf73d08e96</string>
```
