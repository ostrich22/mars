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
* 新增Apple帳號登入
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
