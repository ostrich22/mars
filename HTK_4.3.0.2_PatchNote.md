# HTK 4.3.0.2 Patch Note

## 版本差異
* 4.2.0.13 -> 4.3.0.2

## 重點注意事項

### Android

#### 新增doMsgProcessAccountHesitationDeletionPeriod事件
```java
// 帳號處於後悔期
public void doMsgProcessAccountHesitationDeletionPeriod(String[] args) {
    // 顯示帳號目前狀態
    BridgeController.DeleteAccountInstance().ShowRestoreAccountDialog();
}
```

### iOS
