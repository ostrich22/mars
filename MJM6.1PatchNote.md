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

### iOS
* 儲值流程更新至MSDK 3.5.x
* 儲值相關接口調整
* FB SDK 更新至8.2.0
* 移除UIWebView

## Function變更
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

UserjoyPlatform.doUJEventRequestGold = doUJEventRequestGold;


```
