# Google Play Points Reward串接
* 在Google Play商店購買Google Play Points Reward需要進行讓玩  
家認領商品的動作。  

![流程圖](/mars/Image/GPPReward.png)

## 實作『傳送需認領訂單資訊』的callback
```java
@Override
public void doEventGotClaimOrderInfo(String[] args){
    UjLog.LogInfo("doEventGotClaimOrderInfo json string = " + args[0]);
    try
    {
        JSONObject jsonObject = new JSONObject(args[0]);
        Iterator keyList = jsonObject.keys();
        while (keyList.hasNext())
        {
            String orderID = (String) keyList.next();
            JSONObject orderInfo = jsonObject.getJSONObject(orderID);
            String productID = orderInfo.getString("productID");
            String designID = orderInfo.getString("designID");
        }
    }
    catch (JSONException e)
    {
        e.printStackTrace();
    }
}
```
* args[0]：(json string)需認領的訂單資訊
* orderID：Google回傳的訂單編號
* productID：Google後台設定的商品ID
* designID：遊戲商城設定表的ID

## 進行認領
```java
    String[] orderIDList = new String[]{"GPA.x-x-x-00001", "GPA.x-x-x-00002", "GPA.x-x-x-00003"};
    String serverID = "7105";
    String characterID = "1070700818181";
    GooglePlatform.ClaimOrder(serverID, characterID, orderIDList);

```
* 此功能可以讓玩家自選要領的項目

## 全部認領
```java
    String serverID = "7105";
    String characterID = "1070700818181";
    GooglePlatform.ClaimAllOrder(serverID, characterID);
```
