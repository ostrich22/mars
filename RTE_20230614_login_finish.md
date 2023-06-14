# 驗證玩家

## 取得Login Access Token
``` Java
MarsPlatform.Callback = new MarsPlatform.MessageProcess(){
    @Override
    public void doMsgProcessDelegateSuccess(String[] args)
    {
        String loginAccessToken = args[0];
        // TODO:透過遊戲server調用web api驗證玩家
    }
};
```
``` ObjC
- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions{
    // ......
    [[MarsPlatform Instance] setCallback:self];
}

-(void) doMsgProcessDelegateSuccess: (NSArray*) args{
    NSString* loginAccessToken = args[0];
    // TODO:透過遊戲server調用web api驗證玩家
}
```

## 登入驗證WebAPI

- 使用時機：doMsgProcessDelegateSuccess收到登入成功事件時\
  **_\[註\] AccessToken為消耗性資料, 下次登入驗證時必須從Client取得新的 AccessToken_**
- 測試位址：請透過Userjoy MSDK小組取得
- 正式位址：請透過Userjoy MSDK小組取得

### Step 1. 資料欄位:

#### 資料欄位：

| 欄位 | 對應資料 |
|----|------|
| class | SharedWebApi\\GameSession |
| method | login |
| cmd | 48 |
| session | 客戶端登入MSDK伺服器回傳之AccessToken(時效20分鐘, 單次消耗型, 驗證過後需Client重新取得) |
| token_owner | 欲做驗證的帳號PlayerId |
| operator | 專案名稱，例如：RTE、SGC... |

#### 資料組成範例(PHP)

```php
$apiData = array(
    "class"=>"SharedWebApi\\GameSession",
    "method"=>"login",
    "cmd"=>48,
    "session"  => "BTANPwBqCS1XYwAyDTNUZw",
    "token_owner"  => "1140999999",
    "operator" => "MSDK"
);  
```

### Step 2. 進行請求資料處理與溝通

將組成的資料帶入與MSDK伺服器溝通的專用管道makeSignedRequest($apiData)，進行API資料加密處理並透過curlPost發送API請求。\
(makeSignedRequest實作與curlPost的範例請參閱文件 [0_Prepare For WebAPI](doc-msdk/Server/webapi/0_Prepare_For_WebAPI) )

### Step 3. 處理回傳資料

* SDK伺服器以JSON格式回傳以下資料：

| 欄位 | 說明 |
|----|----|
| reply | 回傳用protocol代碼 |
| code | 回傳代碼(0為要求成功) |
| playerid | 玩家帳號ID |
| os | 0 = 未知<br>1 = ANDROID<br>2 = IOS<br>3 = WINDOWS<br>4 = MAC<br>5 = STEAM |
| time | 驗証成功時間 |

##### 成功：

驗證成功，登入平台為Android。\
由驗證資料取得playerid為1140999999。

```xml
{"SVRCB":{"0":{ "reply":49,"code":0,"playerid":"1140999999",”os”:1,"time":1447916893}}}
```

##### 失敗：

AccessToken驗證失敗：

```xml
{"SVRCB":{"0":{ "reply":56,"0":錯誤碼 } }}
```

#### 錯誤碼對照表：

| 錯誤碼 | 對應狀況 |
|-----|------|
| 12 | AccessToken 過期或有誤 (過期係指超過20或已經被驗證過) |
| 33 | 驗證的專案錯誤 |
| 34 | 驗證的環境錯誤 (ex: 正式機token 卻送到了測試機驗證) |

* 錯誤碼與對應的處理建議：\
  1\.AccessToken過期 - token 時效為**20分鐘**，Client重登後重新驗證Token就可以解決。\
  2\.驗證的專案錯誤 - 請回報MSDK小組協助處理，會與專案串接人員確認設定是否正確。\
  3\.驗證的環境錯誤 - 請串接人員先行確認，登入與驗證的設定是否有對照上。

##### 回傳資料解析範例(PHP)：

```php
    $result = json_decode($resp, true);     // web api response 做json decode
    if (isset($result["SVRCB"]["0"]) && isset($result["SVRCB"]["0"]))
    {
        $obj0 = $result["SVRCB"]["0"];
        $reply = $obj0["reply"];
        if ($reply == 49)
        {
            $playerID = $obj0["playerid"];  // Mars ID
            $os = $obj0["os"];
            // TODO：完成登入流程
        }
        else
        {
            $errorCode = $obj0["0"];
            // TODO：針對錯誤做訊息提示
        }
    }
```
