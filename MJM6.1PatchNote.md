# MJM6.1更新項目
## 更新項目
### Android
* 儲值改為Billing Client 4.0.0
* 儲值相關接口調整
* FB SDK 更新至8.2.0
* 新增第三方儲值用的RequestUJOrderList與對應事件
* 改使用Androidx Libraries
* 使用mainTemplate.gradle管理插件
* 移除帳號引繼功能
* 移除Party Track

### iOS
* 儲值流程更新至MSDK 3.5.x
* 儲值相關接口調整
* FB SDK 更新至8.2.0
* 移除UIWebView
* 移除Party Track

## 函式變更
### 清除登入資訊
```diff
- MarsFunction.ClearInfoForLogin();
+ MarsFunction.ClearMarsInfoForLogin();
```

### 儲值功能初始化
* Android
```diff
- GooglePlatform.Instance().InitGoogleIAB(RSAKey);
+ GooglePlatform.Instance().InitGoogleIAB(RSAKey, "", "", "");
```

* iOS
```diff
- IOSPlatform.Instance().StoreKitPluginInit();
+ IOSPlatform.Instance().StoreKitPluginInit("", "", "");
```

### 確認待領取品項(mycard與PC版專用)
```diff
+ UserjoyPlatform.Instance().RequestUJOrderList("", "", "");
```

* callback事件
```csharp
public void SetCallbacks()
{
    UserjoyPlatform.doUJEventRequestGold = doUJEventRequestGold;
}

public static void doUJEventRequestGold()
{
    // 通知game server透過web api領取訂單
}
```

## 驗證項目
### 登入相關
* 新創馬上玩帳號
* FB登入
* Apple帳號登入
* 帳密登入
* 取消帳號引繼(app刪除重裝後，無法取得之前登入過的帳號)

### 儲值相關
* Google與Apple儲值
* 遊戲內的第三方儲值
* 官網儲值

#### 建議實作
* 進入遊戲後
```csharp
#if UNITY_ANDROID && !UNITY_EDITOR
    GooglePlatform.Instance().RequestUJOrderList();
#elif UNITY_IOS && !UNITY_EDITOR
    IOSPlatform.Instance().RequestUJOrderList();
#else
    UserjoyPlatform.Instance().RequestUJOrderList("", "", "");
#endif
```

* 在商城介面上(或其它遊戲介面上)，加入一按鈕(冷卻3分鐘)，呼叫下面的函式
```csharp
#if UNITY_ANDROID && !UNITY_EDITOR
    GooglePlatform.Instance().RequestUJOrderList();
#elif UNITY_IOS && !UNITY_EDITOR
    IOSPlatform.Instance().RequestUJOrderList();
#else
    UserjoyPlatform.Instance().RequestUJOrderList("", "", "");
#endif
```

* **注意**：在Android與iOS上，要先進行初始化才可以呼叫RequestUJOrderList。

### 手機綁定相關
* 手機綁定功能
* 測試機不會發簡訊，驗證碼固定為331079
* 正式機會發簡訊

## Unity 刪除檔案列表
```
Assets/Editor/XUPorter-master/XCPlist.cs
Assets/Editor/XUPorter-master/XCPlist.cs.meta
Assets/Editor/XUPorter-master/XCPlistEditor.cs
Assets/Editor/XUPorter-master/XCPlistEditor.cs.meta
Assets/Editor/XUPorter-master/XCProject.cs
Assets/Editor/XUPorter-master/XCProject.cs.meta
Assets/Editor/XUPorter-master/XClass.cs
Assets/Editor/XUPorter-master/XClass.cs.meta
Assets/Editor/XUPorter-master/XCodePostProcess.cs
Assets/Editor/XUPorter-master/XCodePostProcess.cs.meta
Assets/Plugins/Android/android-support-v4_27.0.2
Assets/Plugins/Android/appcompat-v7-27.0.2
Assets/Plugins/Android/cardview-v7
Assets/Plugins/Android/facebook-android-sdk-v4.38.1
Assets/Plugins/Android/facebook-applinks-v4.38.1
Assets/Plugins/Android/facebook-common-v4.38.1
Assets/Plugins/Android/facebook-core-v4.38.1
Assets/Editor/XUPorter-master/Plist.meta
Assets/Plugins/Android/facebook-login-v4.38.1
Assets/Plugins/Android/facebook-share-v4.38.1
Assets/Plugins/Android/libs/bolts-android-1.4.0.jar
Assets/Plugins/Android/libs/bolts-applinks-1.4.0.jar
Assets/Plugins/Android/libs/bolts-tasks-1.4.0.jar
Assets/Plugins/Android/libs/gson-2.7.jar
Assets/Plugins/Android/libs/gson-2.7.jar.meta
Assets/Plugins/Android/libs/multidex-1.0.1.jar
Assets/Plugins/Android/libs/multidex-1.0.1.jar.meta
Assets/Plugins/Android/libs/partytrack.jar.meta
Assets/Plugins/Android/libs/play-services-base-8.4.0.aar
Assets/Plugins/Android/libs/play-services-basement-8.4.0.aar
Assets/Plugins/Android/libs/play-services-games-8.4.0.aar
Assets/Plugins/Android/libs/support-compat-27.0.2.aar
Assets/Plugins/Android/libs/support-core-ui-27.0.2.aar
Assets/Plugins/Android/libs/support-core-utils-27.0.2.aar
Assets/Plugins/Android/libs/support-fragment-27.0.2.aar
Assets/Addons/MarsSDK/platform/PartyTrackPlatform.cs
Assets/Addons/MarsSDK/platform/PartyTrackPlatform.cs.meta
Assets/Plugins/Android/libs/support-media-compat-27.0.2.aar
Assets/Plugins/Android/libs/support-v4-27.0.2.aar
Assets/Plugins/Android/mars-sdk-library/libs.meta
Assets/Plugins/Android/runtime-1.0.3
Assets/Plugins/UJPlugin/LitJson
Assets/Plugins/UJPlugin/LitJson.meta
Assets/Plugins/Android/libs/partytrack.jar
Assets/Editor/XUPorter-master/.gitignore
Assets/Editor/XUPorter-master/LICENSE
Assets/Editor/XUPorter-master/LICENSE.meta
Assets/Editor/XUPorter-master/MiniJSON
Assets/Editor/XUPorter-master/MiniJSON.meta
Assets/Editor/XUPorter-master/Mods/GameKit.projmods
Assets/Editor/XUPorter-master/Mods/GameKit.projmods.meta
Assets/Editor/XUPorter-master/Mods/KKKeychain.projmods
Assets/Editor/XUPorter-master/Mods/KKKeychain.projmods.meta
Assets/Editor/XUPorter-master/Mods/MarsResource.projmods
Assets/Editor/XUPorter-master/Mods/MarsResource.projmods.meta
Assets/Editor/XUPorter-master/Mods/MarsSetting.projmods
Assets/Editor/XUPorter-master/Mods/MarsSetting.projmods.meta
Assets/Editor/XUPorter-master/Mods/StoreKit.projmods
Assets/Editor/XUPorter-master/Mods/StoreKit.projmods.meta
Assets/Editor/XUPorter-master/Mods/StoryboardImage.projmods
Assets/Editor/XUPorter-master/Mods/StoryboardImage.projmods.meta
Assets/Editor/XUPorter-master/Mods/StoryboardResource.projmods
Assets/Editor/XUPorter-master/Mods/StoryboardResource.projmods.meta
Assets/Editor/XUPorter-master/Mods/iOS/MarsResources.meta
Assets/Resources/MarsSDK/mars_settings.xml
Assets/Resources/MarsSDK/mars_settings.xml.meta
Assets/Editor/XUPorter-master/PBX Editor
Assets/Editor/XUPorter-master/PBX Editor.meta
Assets/Editor/XUPorter-master/Plist
Assets/Plugins/Android/mars-sdk-library/libs
Assets/Editor/XUPorter-master/PostProcessBuildTrigger.cs
Assets/Editor/XUPorter-master/PostProcessBuildTrigger.cs.meta
Assets/Editor/XUPorter-master/Readme.mdown
Assets/Editor/XUPorter-master/Readme.mdown.meta
Assets/Editor/XUPorter-master/XCBuildConfiguration.cs
Assets/Editor/XUPorter-master/XCBuildConfiguration.cs.meta
Assets/Editor/XUPorter-master/XCConfigurationList.cs
Assets/Editor/XUPorter-master/XCConfigurationList.cs.meta
Assets/Editor/XUPorter-master/XCMod.cs
Assets/Editor/XUPorter-master/XCMod.cs.meta
```
