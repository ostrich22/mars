# 玩家角色控管WebAPI  

## 說明
* 註冊角色與角色更名可透過"購買"與"詢問是否有可領取的訂單"來完成，不一定要呼叫web api。  
* 若是遊戲有角色轉服的需求，則需要呼叫web api刪除舊服角色，避免玩家用網頁儲值時儲錯角色。
<BR><BR>  

- 使用時機：儲值方式為針對角色且可新增／移除之可能時串接  

#### <font color="red">注意：角色名字必須使用UTF-8編碼</font>  

* [註冊角色](#註冊角色)：  
  - 新增一個角色。
* [刪除角色](#刪除角色)：  
  - 刪除一個存在的角色。
* [角色更名](#刪除角色)：  
  - 更改一個存在的角色名稱。

## 註冊角色：  
* 新增一個角色。   

### Step 1. 資料欄位：  

| 欄位          | 對應資料 |
| :------------ | :------- |
| class         | SharedWebApi\MultiCharacterWebApi |
| method        | registerCharacter |
| playerID      | 玩家帳號ID<BR>(MSDK客戶端登入所取得的帳號ID) |
| serverID      | 伺服器編號 |
| characterID   | 角色編號 |
| characterName | 角色名稱 |
| operator      | 專案名稱，例如：MJM、FAZ... |  
  
### 資料組成範例：  

```php
$apiData = array(
    "class"=>"SharedWebApi\\MultiCharacterWebApi",
    "method"=>"registerCharacter",
    "playerID"=>"207881331",
    "serverID"  => "1005",
    "characterID"  => "10052078813311",
    "characterName"  => "一一",
    "operator" => "MJM"
);
```  

### Step 2. 進行請求資料處理與溝通
將構成的資料提供給與MSDK伺服器溝通的專用管道makeSignedRequest($apiData)，進行API資料加密處理並透過curlPost發送API請求。  

### Step 3. 處理回傳資料  
#### 成功範例：  
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\MultiCharacterWebApi::registerCharacter","result":1}}
```
#### 失敗範例：
```
{"SVRCB":{"0":{"reply":0,"code":3,"message":"playerId=207870128, register character fail, character is existed","type":"SharedWebApi\\MultiCharacterWebApiException"}}}
```
## 刪除角色：  
* 刪除一個存在的角色。  
  
### Step 1. 資料欄位：  

| 欄位          | 對應資料 |
| :------------ | :------- |
| class         | SharedWebApi\MultiCharacterWebApi |
| method        | deleteCharacter |
| playerID      | 玩家帳號ID<BR>(MSDK客戶端登入所取得的帳號ID) |
| serverID      | 伺服器編號 |
| characterID   | 角色編號 |
| operator      | 專案名稱，例如：MJM、FAZ... |  
  
### 資料組成範例：  

```php
$apiData = array(
    "class"=>"SharedWebApi\\MultiCharacterWebApi",
    "method"=>"deleteCharacter",
    "playerID"=>"207881331",
    "serverID"  => "1005",
    "characterID"  => "10052078813311",
    "operator" => "MJM"
);
```    
### Step 2. 進行請求資料處理與溝通
將構成的資料提供給與MSDK伺服器溝通的專用管道makeSignedRequest($apiData)，進行API資料加密處理並透過curlPost發送API請求。  

### Step 3. 處理回傳資料  

#### 成功範例：  
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\MultiCharacterWebApi::deleteCharacter","result":1}}}
```
#### 失敗範例：
```
{"SVRCB":{"0":{"reply":0,"code":4,"message":"playerId=207870128, delete character fail, character is not existed","type":"SharedWebApi\\MultiCharacterWebApiException"}}}
```
## 角色更名：  
* 更改一個存在的角色名稱。  
  
### Step 1. 資料欄位：  

| 欄位          | 對應資料 |
| :------------ | :------- |
| class         | SharedWebApi\MultiCharacterWebApi |
| method        | renameCharacter |
| playerID      | 玩家帳號ID<BR>MSDK客戶端登入所取得的帳號ID |
| serverID      | 伺服器編號 |
| characterID   | 角色編號 |
| characterName | 角色名稱 |
| operator      | 專案名稱，例如：MJM、FAZ... |  
  
### 資料組成範例：  

```php
$apiData = array(
    "class"=>"SharedWebApi\\MultiCharacterWebApi",
    "method"=>"renameCharacter",
    "playerID"=>"207881331",
    "serverID"  => "1005",
    "characterID"  => "10052078813311",
    "characterName"  => "一一",
    "operator" => "MJM"
);
```  
### Step 2. 進行請求資料處理與溝通
將構成的資料提供給與MSDK伺服器溝通的專用管道makeSignedRequest($apiData)，進行API資料加密處理並透過curlPost發送API請求。  

### Step 3. 處理回傳資料    

#### 成功範例：  
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\MultiCharacterWebApi::renameCharacter","result":1}}
```
#### 失敗範例：
```
{"SVRCB":{"0":{"reply":0,"code":5,"message":"playerId=207870128, rename character fail, character is not existed","type":"SharedWebApi\\MultiCharacterWebApiException"}}}
```  
  
  
## 回傳碼說明
| Code | 代表含意 |
| :--- | :------- |
| 0   | 操作成功|
| 1   | 帳號資料取得失敗 |
| 2   | 角色資料取得失敗 |
| 3   | 角色註冊失敗 |
| 4   | 角色刪除失敗 |
| 5 | 角色更名失敗 |  
