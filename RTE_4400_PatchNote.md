# MSDK v4.4.0 Release Note 
 
## ※ 4.4.0 - 新版本釋出 - 2024-09-06
### ※ [更新內容]
### [Android] 廣告 ID 相關重大變更：
- Google不再支援`com.google.android.gms:play-services-ads:20.5.0`, 使用此版本的APP需於2024/11月前進行升級或移除才可上傳APP
- 考慮到並非所有專案都需廣告id功能, SDK調整如下:
  - 往後主SDK`UJMobileSDK.aar`不再支援獲取廣告ID功能並移除gradle依賴: `com.google.android.gms:play-services-ads:20.5.0`
  - 若專案需透過UJMobileSDK取得廣告ID, 請跟SDK組拿擴充插件`UJMobileSDK-Extensions-AdMob.aar`, 還需去**Google-Admob後台申請一組廣告AdMob app ID, 並新增到AndroidManifest.xml內才可使用(沒有設定會導致APP閃退)**


### Android 重大變更
- 更新 Billing 庫至 com.android.billingclient:billing 7.0.0。
- 升級 Google Sign-In 庫以使用 credentials-play-services，這意味著無法通過 Sign-In 客戶端檢索 Google UID。
```diff
// mainTemplate.gradle
dependencies {
   // Google Billing Library
-    def billing_version = "6.2.1"
+    def billing_version = "7.0.0"
    implementation "com.android.billingclient:billing:$billing_version"
}
```
```diff
// gradle dependencies
dependencies {
-   implementation 'com.google.android.gms:play-services-auth:20.4.1' //Android 版 Google 登入

+   implementation 'com.google.android.gms:play-services-auth:21.2.0' //Android 版 Google 登入
+   //noinspection CredentialDependency
+   implementation "androidx.credentials:credentials:1.2.2"
+   implementation "androidx.credentials:credentials-play-services-auth:1.2.2"
+   implementation "com.google.android.libraries.identity.googleid:googleid:1.1.1"

}
```

### Android
- 變更: Gradle  移除了 com.google.android.gms:play-services-ads 的依賴。
- 新功能： [Android] 新增 SDK 擴展插件的回調：doMsgProcessExtensionNotAvailable，當擴展插件不存在或未啟用時將觸發此回調。
- 新功能：實作 Billing 客戶端 API getBillingConfigAsync，並新增相應的回調和 API。此 API 可獲得儲值當下用戶所在區域。
- 修正：修復部分在使用 UJMobileSDK v4.2.0 或更低版本時，Google 的待處理訂單無法完成的問題。
- 修正：修復當開發者使用自適應圖標時，UserCenter介面中 App 圖標顯示不正確的問題。

### iOS
- 新功能：StoreKit1 和 StoreKit2 將共存在Framework中，開發者可以選擇使用的 StoreKit 版本，而不需要更改Framework。
- 新功能：在伺服器端添加新的驗證階段，以幫助 SDK 處理 StoreKit1 訂單的狀態。
- 新功能：當用戶使用 Google 登錄時，將用戶的電子郵件資訊發送到服務器（僅適用於 4.4 版本客戶端）。
- 調整：iOS 手機驗證介面優化，改善區號搜索功能。
- 修正：修復在某些情況下，同一筆訂單可能被多次處理的問題。

### Server 
- 調整：改進賬號刪除流程，解決 ALPHA 環境中賬號未按正確時間刪除的問題。
- 新功能：新增版本控制功能，以通過版本號和BundleId初始化獨立平台的 SDK。
- 新功能：新增伺服器端設置版本控制，以管理不同BundleId的不同設置。
- 新功能：新增取消待刪賬號倒計時的 Web API。
- 修正：調整 Web API CheckPlayerResetStatus 響應狀態。
- 修正：修復退款訂單未正確扣除返利的問題。
- 調整: Email驗證信件內文統一式樣

### Mars後台
- 新功能：新增禮包碼類型 (4.MULTI_RACE)，每組禮包碼可給多位玩家使用，且每位玩家可以使用多個不同的禮包碼。
- 修正：(Mars後台)修復禮包碼系統中的某些輸入參數問題。
- 新功能：新增PM專屬API: 使用 Google 購買憑證(purchase token)和 UJ 訂單 ID 幫玩家進行補單。


