# Android Credential Manager

## 登入選項
### PublicKey
允許使用者自行建立一組金鑰，金鑰的產生方式可以是透過生物特徵辨識感應器 (例如指紋或臉部辨識)、PIN 碼或解鎖圖案。
對MSDK來說，相當於一種新的平台登入方式。

### Password
將使用者的帳號與密碼存至Google的密碼管理工具中。
對MSDK而言，只要將現行的Player ID與密碼存入，並在適當的時機讓玩家選擇登入的Player ID，可做為相當方便的小號管
理器。

### Custom
* Google
  提供使用者使用Google帳號登入，這段流程與Web版相似，完成後會拿到IdToken，需再經過驗證才可以取得Google ID。
