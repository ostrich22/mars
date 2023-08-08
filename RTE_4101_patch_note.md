# 更新內容
## 1. 刪除帳號
* (1) 按鈕位置調整
* (2) 規格調整 - 改為三天內可取消, 確認刪除需輸入當下的PlayerId, 時間到將由排程自動刪除
* (3) 新增本地端網頁版用戶中心(但須先登入帳號), 細項功能則需連線

## 2. 禮包碼:
### webapi
#### 修正 
* reply中文內容是亂碼的問題
* GiftCode webapi reply參數型態不正確問題
* 補上doEventRedeemCodeResult 缺少的giftToken參數

#### 調整
* consumeGift 新增回傳參數: recordId
* requestGiftList 新增回傳參數: recordId
### iOS
#### 修正
* 修正禮包碼輸入視窗只能輸入數字問題
* 修正禮包碼部分callback無法正確通知給Unity的問題
        
## 3. 儲值相關:
     
### Android

#### Gradle
```gradle
  def billing_version = "6.0.1"
  implementation "com.android.billingclient:billing:$billing_version"
```
#### 更新
BillingClient升級為 6.0.1 (為了兼容android 14), 並做一些對應的調整
* v6.0後,待付款訂單(Pending Purchase)不會給GP單號. 改成有收到GP單號才記錄到etb_BillingOrderMap, 其餘流程照舊.
#### 修正
* 待處理訂單不能正常記錄的問題
* UJBillingProduct.GetPrice取不到值的問題
### iOS
* 新增API: IOSPlatform.Instance().QueryProducts(), 可跟Apple重新撈取商品資料
        
## 4. mars_setting.xml 機制調整
### 縮減設定
移除 "Mars_Service", "Mars_DSS_UploadServer", "Mars_DSS_DownloadServer", "SIWAWebURL", "Twitter_Callback_URL" 等設定.  
"Mars_Service" 改由遊戲端InitSDK時代入, 等於切換環境需重新InitSDK,  
"Mars_DSS_UploadServer", "Mars_DSS_DownloadServer", "SIWAWebURL", "Twitter_Callback_URL" 改由InitSDK後 MSDK Server打下來

## 5. GameCenter(For Unity):
Xcode 14後需要新的Capbility, 目前Unity不支援, 目前使用其他xcode api強制加上
## 6. 罐頭語音:
### iOS
* 修正玩家拒絕麥克風權限後會開啟遊戲BGM的問題 
## 7. 問題回報:
### Android/iOS
* SDK用戶中心內問題回報在登入角色後改為帶入玩家登入的角色資料、遊戲內暱稱、伺服器Id問題, 登入前因為不知道角色資料則照舊


# 更新方式
## 1. 移除替換舊版SDK( frameworks bundle / aar)

## 2. gradle調整(參考附加檔案):
```diff
- implementation 'com.android.billingclient:billing:5.1.0'
+ def billing_version = "6.0.1"
+ implementation "com.android.billingclient:billing:$billing_version"
```
   
## 3. 調整InitSDK的方式
### Android
```diff
- _androidPlugin = new AndroidPlugin(this);
+ String serviceURL = "https://xxx"; // 設定請參考下方
+ _androidPlugin = new AndroidPlugin(this, serviceURL);
```
### iOS
```diff
- iOSPlugin *plugin = [[iOSPlugin alloc] initWithiOSPlugin];
+ NSString *serviceURL = "https://xxx"; // 設定請參考下方
+ iOSPlugin *plugin = [[iOSPlugin alloc] initWithiOSPlugin:serviceURL];
```
### Service URL
* test  
http://test-2-msdk.uj.com.tw/RTE/game/service.php
* prod  
https://msdk.uj.com.tw/RTE/game/service.php

### 新增對應事件
* Android
```java
MarsPlatform.Callback = new MarsPlatform.MessageProcess() {
  @Override
  public void doMsgProcessInitMSDKCompleted(String[] args){}

  @Override
  public void doMsgProcessInitMSDKFailed(String[] args){}  
};
```

* iOS
```objc
-(void) doMsgProcessInitMSDKCompleted:(NSArray*) args {}
-(void) doMsgProcessInitMSDKFailed(NSArray*) args {}
```

* 範例  
請見demo。

## 4. 切換帳號/切換角色
請先重新設定一次IAB對象才能進入遊戲。
* Android
```java
GooglePlatform.Instance().InitGoogleIAB(RSAKey, serverId, characterId, characterName);
```

* iOS
```objc
[[StoreKitPlugin Instance] Init:serverId :characterId :characterName];
```

## 5. 對應刪除帳號流程的調整
### 修改
* Android
```diff
public void doMsgProcessAccountDeleted(String[] args) {
-  Log.i(UJMarsViewBase.VIEW_TAG,"[AndroidPlugin MarsPlatform] doMsgProcessAccountDeleted, error code  = " + args[0] + ", playerIds = " + args[1]);
+  BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
```
* iOS
```diff
 -(void) doMsgProcessAccountDeleted:(NSArray*) args {
-  [[UjLog Instance] LogInfo:@"doMsgProcessAccountDeleted, status = %@, playerId = %@", args[0], args[1]];
+  [[ViewMgr Instance] ShowRestoreAccountDialog];
}
```

### 新增
* Android
```java
@Override
public void doMsgProcessRequestDeleteAccountSuccess(String[] args) {
    // 顯示對應的提示介面
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}

@Override
public void doMsgProcessRequestDeleteAccountFailure(String[] args) {}

@Override
public void doMsgProcessRequestRestoreAccountSuccess(String[] args) {}

@Override
public void doMsgProcessRequestRestoreAccountFailure(String[] args) {}
```
* iOS
```objc
- (void)doMsgProcessRequestDeleteAccountSuccess:(NSArray *)args {
    // 顯示對應的提示介面
    [[ViewMgr Instance] ShowRestoreAccountDialog];
}

- (void)doMsgProcessRequestDeleteAccountFailure:(NSArray *)args {}

- (void)doMsgProcessRequestRestoreAccountSuccess:(NSArray *)args {}

- (void)doMsgProcessRequestRestoreAccountFailure:(NSArray *)args {}
```

## 6. 移除SetServiceURL, SetDSSDownloadURL, SetDSSUploadURL, SetImagePersonalUploadURL, SetImageMessageUploadURL等相關舊API

## 7. Android Target API 33:
```diff
+    <!-- target api 33 拿廣告ID需要新增此權限-->
+    <uses-permission android:name="com.google.android.gms.permission.AD_ID" />
```
