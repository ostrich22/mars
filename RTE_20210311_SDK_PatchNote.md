# 更新說明

## 更新項目

### Android
* 1. 內購改為Google Billing Client 3.0.2。
* 2. 內購相關接口調整。
* 3. UJMobileSDK 改依賴AndroidX libraries。
* 4. FB SDK更新至8.2.0。

### iOS
* 1. 對應Android調整內購接口。
* 2. FB SDK更新至8.2.0。
* 3. 問題回報頁面關閉按鈕調整。

## 檔案說明

### AndroidDemo
* 相關依賴的libraries可以參考app\build.gradle的dependencies。
* AndroidX libraries的設定可以參考gradle.properties。

### iOS_Demo
FB SDK 8.2.0的配置比較多，下面會詳述。

### UJMobileSDK
雙平台要更新的檔案。

## 內購接口更改說明
* 1. 角色資訊改至初始化時帶入，需帶入服務器ID、角色ID與角色名稱。
* 2. 購買時，會使用帶入的角色資訊，進行角色註冊與更名的動作。  
(也就是玩家建立角色後，不需要調用web api來進行角色註冊)
* 3. 若玩家進行切換角色，需再次調用內購初始化來帶入新的角色資訊。
* 4. 認領GPP獎勵的功能，也會使用初始化帶入的角色資訊。
* 5. 新增doEventQueryInventory Event(傳回目前所有商品的資訊)。

## 接口範例

### Android
```javascript
  // 初始化
  String serverID = "1001";
  String characterID = "123456789";
  String characterName = "牛肉好吃";
  String RSAKey = "...";
  GooglePlatform.InitGoogleIAB(RSAKey, serverID, characterID, characterName);
  
  // 購買
  String designID = "1006";
  String extraInfo = ""; // 尚未開放
  GooglePlatform.DoPurchase(designID, extraInfo);
  
  // 詢問尚未領取的項目
  GooglePlatform.RequestUJOrderList();
```

### iOS
```ObjC
  // 初始化
  NSString* serverID = @"1001";
  NSString* characterID = @"123456789";
  NSString* characterName = @"牛肉好吃";
  [[IOSPlatform Instance] StoreKitPluginInit:serverID :characterID :characterName];
  
  // 購買
  NSString* designID = @"1006";
  NSString* extraInfo = @""; // 尚未開放
  [[IOSPlatform Instance] DoPurchase:designID :extraInfo];
  
  // 詢問尚未領取的項目
  [[IOSPlatform Instance] RequestUJOrderList];
```

## Android 環境配置

### AndroidX Libraries
在gradle.properties加入
```
android.useAndroidX=true
android.enableJetifier=true
```

### 依賴調整
在app\build.gradle
```diff
    implementation 'com.google.code.gson:gson:2.7'
-    implementation 'com.android.support:multidex:1.0.1'
-    implementation 'com.facebook.android:facebook-android-sdk:5.13.0'
-    implementation 'com.android.support:appcompat-v7:27.0.2'
+    implementation 'androidx.multidex:multidex:2.0.1'
+    implementation 'com.facebook.android:facebook-android-sdk:8.2.0'
+    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'com.google.android.gms:play-services-games:17.0.0'
    implementation 'com.google.android.gms:play-services-auth:16.0.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
-    implementation 'com.android.support:design:27.0.2'
-    implementation 'com.android.support:recyclerview-v7:27.0.2'
-    implementation 'android.arch.lifecycle:extensions:1.1.1'
-    implementation 'android.arch.lifecycle:runtime:1.1.1'
+    implementation 'androidx.arch.core:core-runtime:2.1.0'
+    implementation 'com.google.android.material:material:1.0.0'
+    implementation 'com.android.billingclient:billing:3.0.2'

+    implementation "androidx.lifecycle:lifecycle-runtime:2.1.0"
+    implementation "androidx.lifecycle:lifecycle-extensions:2.1.0"
+    implementation 'androidx.preference:preference:1.1.0'
```

### 更換UJMobileSDK
更換UJMobileSDK-1.0.aar。

## iOS 環境配置

### 更換Framework
建議從Xcode做刪除後，再加入新版的
```
FBSDKCoreKit.framework
FBSDKLoginKit.framework
FBSDKShareKit.framework
UJMobileSDK.framework
```

### 加入Accelerate.framework
在專案設定->Build Phases->Link Binary With Libraries加入Accelerate.framework。

### 建立Swift bridge
專案新增一支swift檔(空白就好)，Xcode會詢問是否要建立Swift bridge，點同意後就完成。

### 加入-lc++
在專案設定->Build Settings->Other Linker Flags中加入-lc++。
