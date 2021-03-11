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
