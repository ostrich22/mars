# MSDK更新說明

## 更新項目
* FB SDK更新(舊版FB SDK有使用UIWebView)
* 移除UIWebView，改用WKWebView
* 移除Party Track
* 移除未使用的twitter相關資源
* 新增Apple防刷退功能

## 更新framework
建議將以下的framework刪除後，再將新包中的framework匯入專案。
* Bolts.Framework
* FBSDKCoreKit.framework
* FBSDKLoginKit.framework
* FBSDKShareKit.framework
* Partytrack.framework
* UJMoblieSDK.framework

## FB SDK
新版的FB SDK改為dynamic，匯入後需多做設定。  
專案設定->General->Frameworks, Libraries, and Embedded Content
將FBSDKCoreKit、FBSDKLoginKit與FBSDKShareKit這三個framework的Embed改為Embed&Sign。  
![示意圖](/Image/FBSDKSetting.png)
