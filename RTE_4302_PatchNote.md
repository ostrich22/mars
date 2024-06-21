# RTE 4.3.0.2 更新項目

## 版本差異
* 4.1.0.1 -> 4.3.0.2

## 重點注意事項

### Android
* minSdk 24
* 調整依賴函式庫(請參考Demo_Android中build.gradle的dependencies)
* 調整刪除帳號規則
  * 直接刪除 -> 執行後進入三天後悔期
  * 後悔期期間可以復原帳號
  * 三天後，由排程執行刪除帳號動作
  * 新增刪除/復原帳號相關事件(請參考Demo_Android中MainActivity.java 767~798)
  * 以下三個事件必須呼叫BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
```java
// ShowRestoreAccountDialog會提示玩家目前帳號的狀況，若帳號還在後悔期，該介面可以復原帳號
@Override
public void doMsgProcessAccountDeleted(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
@Override
public void doMsgProcessRequestDeleteAccountSuccess(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
@Override
public void doMsgProcessAccountHesitationDeletionPeriod(String[] args) {
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
```
  * 
