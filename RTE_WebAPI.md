# Web API新增與調整
## 調整requestUJOrderListMultiChar回傳資料
* 允許回傳empty array
* 新增訂閱資訊

### 允許回傳empty array
當沒有可領取項目時，現在改回傳empty array
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\GameBilling::requestUJOrderListMultiChar","result":1,"IABList":{},"thirdList":{},"subsList":{}}}}}}
```

### 新增訂閱資訊
#### 回傳json string
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\GameBilling::requestUJOrderListMultiChar","result":1,"IABList":{},"thirdList":{},"subsList":{"115":{"expiry":1550711288,"orderList":{"0":"APP121573G1995376856","1":"APP121752472294573R8","2":"APP121987713708N1980","3":"APP12144374466412T98","4":"APP121R5519755708075","5":"APP121272357414104J5","6":"APP12141411X49726027","7":"APP12169U70191665674"}}}}}}
```
#### 取出訂閱資訊(php範例)
``` php
$reply = CurlKid::curl_post($url, $post, $curlOptions, $warningMsgTime);
$result = JsonKit::decode($reply);
if (!isset($result["SVRCB"]) || !isset($result["SVRCB"][0]))
{
    return;
}

if (!isset($result["SVRCB"][0]["result"]) || $result["SVRCB"][0]["result"] != 1)
{
    return;
}

$subsList = $result["SVRCB"][0]["subsList"];
foreach ($subsList as $designID => $subsData)
{
    $expiry = $subsData["expiry"];      // Unix Time(單位：秒)
    // TODO：更新到期時間
    $orderList = $subsData["orderList"];
    foreach ($orderList as $orderID)
    {
        // TODO：記錄 order id
    }
}
```
* 每筆order id代表開始訂閱或續訂

## 新增requestSubscriptionsStatus(取得訂閱到期時間)
### API Data
| 欄位          | 對應資料 |
| :------------ | :------- |
| class         | SharedWebApi\GameBilling |
| method        | requestSubscriptionsStatus |
| playerID      | Mars Player ID<BR>SDK登入所取得的PlayerId |
| serverID      | 伺服器編號 |
| characterID   | 角色編號 |
| operator      | 專案名稱，例如：RTE... |
### WebAPI 回傳格式
#### 成功：  
```
{"SVRCB":{"0":{"reply":"SharedWebApi\\GameBilling::requestSubscriptionsStatus","result":1,"subsList":{"115":1550711288}}}}
```
#### 失敗：
```
{"SVRCB":{"0":{"reply":0,"code":67021,"message":"playerId=206026619, get dataMgr fail","type":"SharedWebApi\\GameBillingException"}}}
```
### 取出訂閱到期時間(php範例)
``` php
$reply = CurlKid::curl_post($url, $post, $curlOptions, $warningMsgTime);
$result = JsonKit::decode($reply);
if (!isset($result["SVRCB"]) || !isset($result["SVRCB"][0]))
{
    return;
}

if (!isset($result["SVRCB"][0]["result"]) || $result["SVRCB"][0]["result"] != 1)
{
    return;
}

$subsList = $result["SVRCB"][0]["subsList"];
foreach ($subsList as $designID => $expiry)
{
    // TODO：更新到期時間
}
```

