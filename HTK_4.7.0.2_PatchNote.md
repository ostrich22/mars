# 版本變更
4.5.0.4 -> 4.7.0.2

# 更新歷程
[連結](https://doc-msdk.uj.com.tw/#/ReleaseNote/v4.0)

# 更新方式
* 請先刪除舊版SDK資料後，再倒入新版SDK

# 重點提醒

## Android
* 請務必比對Demo中`app\build.gradle`所依賴的插件

### 刪除/復原帳號功能
* 流程中的提示對話窗改為自動彈出
* 調用`BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog()`的地方必須刪除

### PGS
* 調整function SyncPGSData
```diff
- PlayGameServicePlatform.SyncPGSData();
+ PlayGameServicePlatform.Instance().SyncPGSData();
```

* 新增PGS相關callback
  * 詳細請見MainActivity.java中function RegisterGooglePGSCallbacks

### 第三方充值功能重構
* 為解決角色資料註冊時機過晚問題, 需先執行`InitUJWebBilling`後**進行儲值初始化與註冊角色**後才能使用相關功能. **初始化結果callback: `doUJEventInitUJWebBilling`**
```java
//init billing
UserjoyPlatform.Instance().InitUJWebBilling(serverID, characterID, characterName);

//callback
@Override
public void doUJEventInitUJWebBilling(String[] args) {
}
```

* 請改用以下API進行UJ平台儲值相關操作:
  * `OpenNewsPages`: (開啟/取得) UJ平台 - 最新公告頁面URL
  * `OpenCustomerServicePages`: (開啟/取得) UJ平台 - 問題回報頁面URL
  * `OpenFAQPages`: (開啟/取得) UJ平台 - FAQ頁面URL
  * `OpenUJWebBillingPages`, `OpenUJWebBillingPagesWithCountryCode`: (開啟/取得) UJ平台 - 官網儲值頁面URL (需先初始化)
  * `DoPurchase`, `DoPurchaseWithCountryCode`: (開啟/取得) 特定品項的官網儲值頁面URL  (需先初始化)

* API變更列表
```diff
    //---- API調整範例
    //(開啟/取得) UJ平台 - 最新公告頁面URL
-   UserjoyPlatform.Instance().RequestNewsURL("GG", "1", "9527");
+   UserjoyPlatform.Instance().OpenNewsPages(UserjoyPlatform.UJWebOption.AutoOpenURL);

    //(開啟/取得) UJ平台 - 問題回報頁面URL
-   UserjoyPlatform.Instance().RequestCustomerServiceURL("GG", "1", "9527", "ja-JP");
+   UserjoyPlatform.Instance().OpenCustomerServicePages(UserjoyPlatform.UJWebOption.AutoOpenURL, "ja-JP");

    //(開啟/取得) UJ平台 - FAQ頁面URL
-   UserjoyPlatform.Instance().RequestFaqURL("GG", "1", "9527");
+   UserjoyPlatform.Instance().OpenFAQPages(UserjoyPlatform.UJWebOption.AutoOpenURL);

    //官網儲值頁面
-   UserjoyPlatform.Instance().StartUJWebWithCountryCode(_characterName, _serverId, "3", "", "", _characterId, "TW");
+   UserjoyPlatform.Instance().OpenUJWebBillingPagesWithCountryCode(UserjoyPlatform.UJWebOption.AutoOpenURL, "TW");

    //詢問是否有尚未領取訂單
-   UserjoyPlatform.Instance().RequestUJOrderList(_serverId, _characterId, _characterName);
+   UserjoyPlatform.Instance().RequestUJOrderList();

    //官網儲值頁面-特定品項
-   UserjoyPlatform.Instance().DoPurchaseWithCountryCode(_serverId, _characterId, _characterName, webbilling_DesignId_textField, webbilling_ItemType_textField, "TW");
+   UserjoyPlatform.Instance().DoPurchaseWithCountryCode(webbilling_DesignId_textField, webbilling_ItemType_textField, "TW");
```

* 修正`UserjoyPlatform`獲取平台網址發生錯誤時, 不會觸發callback問題. **重構對應callback的回傳參數**:
```java
UserjoyPlatform.Callback = new UserjoyPlatform.MessageProcess() {
    @Override
    public void doUJEventGetUJWebURL(int status, String[] args) {
        if (status == 0) {
            String url = args[0];
            Log.i(UJMarsViewBase.VIEW_TAG,"Get UJWebURL status : " + status + ", url : " + url);
        } else {
            Log.i(UJMarsViewBase.VIEW_TAG,"Get UJWebURL Failed status : " + status);
        }
    }
}
```

### 信箱驗證
* 新增事件
```java
    @Override
    public void doEventRequestPassSucceeded() {
    }

    @Override
    public void doEventRequestPassFailed(int status) {
    }
```
