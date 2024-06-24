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
* 以下三個事件必須呼叫BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
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
