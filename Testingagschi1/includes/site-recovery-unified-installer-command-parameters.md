|參數名稱| 類型 | 說明| 可能的值|
|-|-|-|-|
| /ServerMode|強制|指定應該同時安裝組態和處理序伺服器，還是只安裝處理序伺服器|CS<br>PS|
|/InstallLocation|強制|安裝元件的資料夾| 電腦上的任何資料夾|
|/MySQLCredsFilePath|強制|儲存 MySQL 伺服器認證的檔案路徑|此檔案應該具備如下所指定的格式|
|/VaultCredsFilePath|強制|保存庫認證檔的路徑|有效的檔案路徑|
|/EnvType|強制|您想要保護的環境類型 |VMware<br>NonVMware|
|/PSIP|強制|要用於複寫資料傳輸的 NIC IP 位址| 任何有效的 IP 位址|
|/CSIP|強制|接聽組態伺服器的 NIC IP 位址| 任何有效的 IP 位址|
|/PassphraseFilePath|強制|複雜密碼檔案的位置完整路徑|有效的檔案路徑|
|/BypassProxy|選用|指定組態伺服器不使用 Proxy 連接至 Azure|若要這樣做，請從 Venu 取得此值|
|/ProxySettingsFilePath|選用|Proxy 設定 (預設的 Proxy 需要驗證或自訂的 Proxy)|此檔案應該具備如下所指定的格式|
|DataTransferSecurePort|選用|要用於複寫資料的 PSIP 上的連接埠號碼| 有效的連接埠號碼 (預設值是 9433)|
|/SkipSpaceCheck|選用|略過快取磁碟的空間檢查| |
|/AcceptThirdpartyEULA|強制|旗標表示接受協力廠商使用者授權合約| |
|/ShowThirdpartyEULA|選用|顯示協力廠商使用者授權合約。 如果提供作為輸入，則會忽略所有其他參數| |
