# 版本變更
- 4.4.0.2 -> 4.5.0.4

# 重點內容
## Android
1. windows 的 layoutInDisplayCutoutMode 
    - 原本大於 API 28 以上會使用 LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES
    - 增加 API 35 以上會更換成 LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.VANILLA_ICE_CREAM) {
    getWindow().getAttributes().layoutInDisplayCutoutMode = WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_ALWAYS;
}
else if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
    getWindow().getAttributes().layoutInDisplayCutoutMode = WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES;
}
```
2. PGS(Play Game Service) 初始化有改方法並且初始化有變位置
   - 刪除有實作 `GooglePlatform.InitPGSSdk();` 的地方
   - 要確認在 MarsPlatform.Callback 的 doMsgProcessInitMSDKCompleted 是否有 `PlayGameServicePlatform.SyncPGSData();`
   - 不建議在其他地方使用 `PlayGameServicePlatform.SyncPGSData();`
```java
@Override
public void doMsgProcessInitMSDKCompleted(String[] args) {
    Log.i(UJMarsViewBase.VIEW_TAG,"[AndroidPlugin MarsPlatform] doMsgProcessInitMSDKCompleted");
    PlayGameServicePlatform.SyncPGSData();
    DemoView.SetShow(true);
}
```
3. 變更了一些 Callback
   - 新增了 UserjoyPlatform.Callback 的 doUJEventInitUJWebBilling
   - 刪除了 TelephoneVerifyPlatform.Callback 的 doEventAccountHasTelephoneVerified
   - 刪除了 TelephoneVerifyPlatform.Callback 的 doEventVerifyFailed
   - 刪除了 TelephoneVerifyPlatform.Callback 的 doEventVerifyFailReachLimited
   - 新增了 SelectPhotoPlatform.Instance().eventCallback 的 doEventNotCameraPermission
4. 大小寫變更
   - `PlayerCharactorBridge.getInstance().IsSet()` 改成 `PlayerCharactorBridge.getInstance().isSet()` 
5. 取得地區號碼回傳類型有改
   - 原本的 `TelephoneVerifyPlatform.Instance().GetAreaCodeList();` 是回傳 `String[]` 改成 `Map<String, String>` 。下方那個可以將 `Map<String, String>` 轉換成 `String[]`
    ```java
    Map<String, String> rawAreaCodeList = TelephoneVerifyPlatform.Instance().GetAreaCodeList();
        String[] areaCodeList = new String[rawAreaCodeList.size()];
        int index = 0;
        for (Map.Entry<String, String> entry : rawAreaCodeList.entrySet()) {
            areaCodeList[index] = entry.getKey() + "(" + entry.getValue() + ")";
            index++;
        }
    ```
6. 是否使用 Mars UI 的設定改為伺服器回傳 
   - 刪除 `LoginMgr.Instance().SetIsUJLoginUI(bool)`。bool可能是 true 或 false
## iOS
1. Minimun Deployments需為15.0 (配合StoreKitV2)

# MSDK 4.5.x Release Note  

<a id="ver-4504"></a>
## ※ 4.5.0.4 - 2025-07-08
### ※ [更新內容]

## iOS
- 調整: WKWebView針對requestWithURL針對網址格式調整 SafelyConvertToNSURL的處理方式
- 新增: 鎖定MSDK UI方向的API
- 修正: 推播title含”/“字元時，圖片無法存檔的問題
- 修正: 訂閱訂單短時間通知兩次,會重複送驗證請求的問題

## Android
- 新增: 增加啟動相機前確認權限的功能
- 調整:UJ平台網頁的showtype改由server加
- 調整: 調整SDK內部PGS SDK 初始化時機
- 修正:調整橫版手機驗證介面，軟體鍵盤的樣式(避免蓋住全畫面)
- 修正:因缺少theme讓UI文字變小的問題

## Unity
- 調整: 調整3DWebview的demo
- 新增: 新增鎖定MSDK UI方向的API
- 調整: 同步手機認證取得AreaCodeList規格
- 新增: SelectPhotoPlatform 的 Callback: 沒有相機權限的事件

## Unreal
- 新增: 鎖定MSDK UI方向的API
- 調整: 同步手機認證取得AreaCodeList規格
- 新增: SelectPhotoPlatform 的 Callback: 沒有相機權限的事件
- 調整: 使用Slate的方式實作MSDK UI


## Server
- 新增: 新增規格: 禮包碼輸入錯誤次數過多會鎖定禮包碼輸入
> 錯誤三次, 五分鐘內無法進行輸入
- 新增: 開始支援StoreKit 2訂閱訂單
- 新增: (Beta功能) 後台新增: 新雙平台補單API; GP訂單補單後會自動銷單, IOS訂單可根據client收據單號配合商品id來補單
- 新增: webbilling 儲值返利額度 強制覆寫API
- 調整: 調整優化透過後台設定Redis的版控功能
   - 1. 設定送審版號, 如果bundleName獨立設定不在, 不會在額外加上去
   - 2. 目前僅有透過 updateUIStatus 設定才會產生bundleName, 且會從Mars:Client:複製所有clientUI上來

<a id="ver-4503"></a>
## ※ 4.5.0.3 - 2025-05-09
### ※ [更新內容]
## iOS
- 修正: GameCenter 在綁定/解綁介面時, 關閉MSDKUI沒有送事件而遊戲UI無法透過事件解鎖的問題
- 新增: 新增Apple退款請求相關服務支援

## Unreal
- 新增: 修改 IOS_UPL.xml，以避免覆蓋專案設定。
- 修正: 修正直接開啟伺服器網頁時，URL 後面的資料會消失的問題。
- 修正: 有機率會在關閉瀏覽器會閃退的問題
- 修正: 如果開啟統一編譯(Unity Build)的功能名稱會被 Windows API 巨集修改。
- 修正: 綁定信箱後，沒有收集信箱地址的問題
- 修正: 儲值後詢問可領取訂單時會發生閃退的問題
- 調整: UJWebBillingGetTokenReply stsus回傳形態已經移除強制轉型string, 改回int處理
- 

## Android
- 新增: 讓環境支援TargetAPI 35

## Server
- 新增: 遊戲端註冊通知Callback擴充
    * WebBilling: 用於通知網頁儲值到帳可領取
    * ForceLogout: 用於通知帳號需要踢下線, 比如被封鎖
    * Refund: 用於通知訂單被退款, 目前專案針對退款的品項沒有動作, 新增針對資產調整的可能性
    * ConsumptionRequest: 用於通知玩家退款請求, 向遊戲端取得該玩家資訊, 提供apple判斷是否允許退款
- 新增:  儲值反利重複啟用功能

<a id="ver-4502"></a>
## ※ 4.5.0.2 - 2025-02-20
### ※ [更新內容]
## iOS
- 修正：修正手機認證介面的filter功能有機會因為時間差而NULL的狀況
- 修正： [UserjoyPlatform] API: StartUJWeb 角色id沒有正確送出問題
- 修正： [UserjoyPlatform] API: InitUJWebBilling後收不到callback問題


## Unity
- 修正：刪除帳號成功後還是收到fail事件
- 修正：[WebUI] 修復 PC 版介面的刪除頁面卡死問題
- 調整: [WebUI] 移除非必要檔案: `string.rar`

## Unreal
- 新增: PC端開始支援DSS相關功能, ex: 個人頭像,離線語音
- 修正: 當專案加入MSDK後, 關閉UE Editor時會有閃退問題(觸發crash reporter)
- 新增: [UserjoyPlatformEntry] 新增 API: `InitUJWebBilling` & `IsInit`
- 調整: 以下API實作邏輯將與Untiy一致: SteamPlatform & UserjoyPlatfrom API: RequestUJOrderList 

<a id="ver-4501"></a>
## ※ 4.5.0.1 - 2025-02-07
### ※ [更新內容]
## iOS
- 修復：修正了iOS UI啟動方法，將其指定為主執行緒（MainThread），因為在Unreal中未指定主執行緒會導致「非主執行緒」異常。

## Android
- 修復：修正手機認證介面的filter功能有機會因為時間差而NULL的狀況, 順手優化語系切換後, 原本已選過的語系應該還是要重新選擇

## Unity
- 修正：修正了啟用腳本定義 _DISABLE_UJ_MSDK 時，未能正確禁用所有MSDK功能的問題。
- 新增：[WebUI] 調整了中文、韓文和日文區域語言的會員中心佈局樣式。
- 新增：[WebUI] 優化了WebUI中的字串顯示方法，減少了語言切換時的閃爍問題。
- 新增：[WebUI] 調整了WebUI相關API的名稱，使功能名稱更加清晰明瞭。
- 新增：移除了Demo對IngameDebugConsole插件的依賴。
- 修正：MSDK不支援.NET Standard 2.1的問題
- 新增：新增設定可開啟或關閉語音/圖片上傳與下載的等待進度，預設為開啟。
        MarsPlatform.EnableImageWaitProgress(bool enableMessage, bool enablePersonal)
        MarsPlatform.EnableVoiceWaitProgress(bool enableUpload, bool enableDownload)

## Unreal
- 新增：[WebUI] 調整了中文、韓文和日文區域語言的會員中心佈局樣式。
- 新增：[WebUI] 優化了WebUI中的字串顯示方法，減少了語言切換時的閃爍問題。
- 新增：[WebUI] 修改了Unreal內嵌瀏覽器，防止螢幕閃爍。

<a id="ver-4500"></a>
## ※ 4.5.0.0 新版本釋出 - 2024-12-26
### ※ [更新內容]
## iOS
- 新增: 新增UserJoy官網儲值初始化API: `[[UserjoyPlatformBridge Instance] InitUJWebBilling:serverID :serverID :characterName]`
- 新增: 移除以下手機綁定callback:
    - doEventAccountHasTelephoneVerified
    - doEventVerifyFailed
    - doEventVerifyFailReachLimited
- 新增: 補完原生推播callback:
    - OnNotificationPermissionAllow (權限取得成功)
    - OnNotificationPermissionCancel(
    - OnNotificationPermissionDenied
    - OnNotificationPermissionDontAllow
    - OnIOSNotificationSettingsStatus
- 新增: MoJoy信箱驗證新增API: `GetVerifyCodeResendTime`


## Android
- 新增: 優化PGS同步帳號流程: 帳號同步時可跳出詢問視窗, 使用者可決定是否將PGS紀錄的帳號同步到此裝置上.
- 新增: 新增UserJoy官網儲值初始化. API: `UserjoyPlatform.InitUJWebBilling`. callback: `doUJEventInitUJWebBilling`
- 新增: PlayerCharactor API checkCharacterData 更名為 checkCharacterDataFormat
- 新增: 移除以下手機綁定callback:
    - doEventAccountHasTelephoneVerified
    - doEventVerifyFailed
    - doEventVerifyFailReachLimited
- 修正: 修正原生在推播權限被拒絕時，會觸發兩次詢問dialog的問題


## Unity
- 新增: 新增UserJoy官網儲值初始化. API: `UserjoyPlatform.Instance().InitUJWebBilling(_serverId, _characterId, _characterName)`. callback: `public static doProccessWithStatus doInitBilling;`
- 新增: [GooglePlatform/IOSPlatform] 新增API `public UJBillingProduct GetProduct(string productId)`, 可用於取得GP/IOS後台儲值品項資訊(需等初始化成功後才可使用)
- 新增: SDK廣告ID功能已改為擴充插件. AndroidManifest.xml 移除設定檔 `com.google.android.gms.permission.AD_ID`
- 修正: [PC] 修正 API: `InitUnityPluginWithOS` 在切換package name的狀況下, WebUI URL可能會有不正確的URL問題.

### Unity Beta Feature:
- [Unity] SDK Dss相關功能可在PC端上使用. ex: 個人頭像,罐頭語音.


## Unreal (此版本重點更新內容)
### 破壞性更新, MSDK使用的Config不需更改專案端的Config內容, **請參考以下說明搬移專案端的設定值**. 原先放置於`<Project>/Config/DefaultGame.ini`內的SDK設定已不使用.
#### SDK Config內容變更
#### 1. 以下放置於`[專案資料夾]/Config/DefaultGame.ini`的SDK設定移至`[專案資料夾]/Config/DefaultUJMobileSDK.ini`

```ini
[/Script/UJMobileSDK.MarsDeveloperSettings]
BP_WebBrowser=/UJMobileSDK/Blueprints/Widget/Mars/WBP_MarsDemoMars_WebBrowser.WBP_MarsDemoMars_WebBrowser_C
WebBrowserHeightCurve=/UJMobileSDK/Blueprints/Widget/Mars/Mars_WebCurve_Hight.Mars_WebCurve_Hight
WebBrowserWidthCurve=/UJMobileSDK/Blueprints/Widget/Mars/Mars_WebCurve_Width.Mars_WebCurve_Width
WebCurveType=Width
```

#### 2. UPL設定檔將額外生成`DefaultUJMobileSDKForUPL.ini`, 與Rumtime設定做區分. 且`DefaultUJMobileSDKForUPL.ini`只存在於本機端不會隨著包檔而釋出.
- 移除以下放置於`[專案資料夾]/Config/DefaultGame.ini`的SDK設定. 
- UPL設定移至`<Project>/Restricted/NoRedist/Config/DefaultUJMobileSDKForUPL.ini`

```ini
[Staging]
+AllowedConfigFiles=clientDevByUnreal/Config/DefaultUJMobileSDK.ini
```

#### MSDK WebUI實作方式調整
- 原先放於 `Project Setting`->`USERJOY MobileSDK Setting`的WebUI設定放至`Runtime`內. 並支援以下設定值(等同於[SDK Config內容變更](#sdk-config內容變更) ):
> - Web Brower BP(等同原先的WebUI BP設定路徑): 含有"MarsWebBrowser"元件的 Widget Blueprint。
> - Web Brower Curve Type: 指定 Web Brower 視窗縮放基準(預設為Width)
> - Web Brower Height Curve: 高度依據縮放曲線. 如果"Web Brower Curve Type"是 Height，就會按照此比例去縮放。
> - Web Brower Width Curve: 寬度依據縮放曲線. 如果"Web Brower Curve Type"是 Width，就會按照此比例去縮放。

- 使用UE內建瀏覽器實作WebUI的邏輯移至C++並移除原有BP邏輯. 根據以下範例實作, 即可串接MSDK WebUI所需的`Title` `ButtonClose` `ButtonPreviousPage` 事件

```cpp
// Copyright (c) UserJoy Technology All Rights Reserved.

#include "Widgets/DefaultWebBrowser.h"

void UDefaultWebBrowser::NativeConstruct() {
    Super::NativeConstruct();
    MarsWebBrowser->SetTitleObject(TextBlockTitle);
    MarsWebBrowser->SetButtonClose(ButtonClose);
    MarsWebBrowser->SetButtonPreviousPage(ButtonPreviousPage);
}

```

- 使用UE內建瀏覽器實作WebUI的參考內容移至`<Project>\Plugins\UJMobileSDK\Content\Blueprints\Widget\Mars\Mars_DefaultWebBrowser.uasset`


### 其餘更新項目:

- 新增: 完善Demo示範內容.
- 新增: SDK Callback Delegate 改成 Multicast Delegate
- 新增: 移除以下手機綁定callback:
    - doEventAccountHasTelephoneVerified
    - doEventVerifyFailed
    - doEventVerifyFailReachLimited
- 新增: 各功能處理邏輯與Unity端同步
- 修正: 修正SDK錯誤邏輯
