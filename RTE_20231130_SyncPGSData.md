# 啟用PGS同步帳號機制

## 說明
Google商店版app需先開啟PGS同步帳號機制，Android 電腦版才可以取得  
已在Android平台上遊玩的帳號。

## 更新檔案
* 新增app\libs\PGS\UJMobileSDK-PGS-121.4.1.2.1.aar
* 更新app\src\main\sharedres\values\mars_settings.xml

## 執行同步

### Demo
* MainActivity.java
```java
// 初始化MSDK
_androidPlugin = new AndroidPlugin(this, "https://msdk.uj.com.tw/RTE/game/service.php");

// 註冊callback
RegisterCallbacks();

// 同步PGS資料
PlayGameServicePlatform.SyncPGSData();
```

## 測試步驟
* 1.使用啟動PGS同步的client進行登入或創新帳號
* 2.刪除App再重新安裝
* 3.進入遊戲開啟會員中心，觀察是否為第一步的帳號
