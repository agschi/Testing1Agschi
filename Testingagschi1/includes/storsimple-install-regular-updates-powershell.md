<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>透過 Windows PowerShell for StorSimple 安裝一般更新
1. 開啟裝置序列主控台，然後選取選項 1 [使用完整存取權登入] 。 輸入密碼。 預設密碼為 *Password1*。 
2. 在命令提示字元中，輸入：
   
     `Get-HcsUpdateAvailability`
   
    系統將通知您是否有可用的更新，以及更新是否為破壞性或非破壞性更新。
3. 在命令提示字元中，輸入：
   
     `Start-HcsUpdate`
   
    更新程序將隨即開始。

> [!IMPORTANT]
> * 這個命令只適用於一般更新。 您只需在某一個控制站上執行此命令，就會更新這兩個控制站。 
> * 您可能在更新程序期間注意到控制站容錯移轉。不過，容錯移轉並不會影響系統的可用性或運作。
> 
> 

