<!--author=SharS last changed: 02/22/2016-->

### <a name="to-configure-and-register-the-device"></a>設定和註冊裝置
1. 存取 StorSimple 裝置序列主控台上的 Windows PowerShell 介面。 如需相關指示，請參閱 [使用 PuTTY 來連接至裝置序列主控台](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) 。 **請務必確實依照此程序，否則將無法存取主控台。**
2. 在開啟的工作階段中，按 Enter 鍵一次以取得命令提示字元。
3. 系統將提示您選擇想要為裝置設定的語言。 指定語言，然後按 Enter 鍵。
   
    ![StorSimple 設定和註冊裝置 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. 在顯示的序列主控台功能表中，選擇選項 1 以使用完整存取權進行登入。
   
    ![StorSimple 註冊裝置 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. 執行下列步驟，為裝置設定最小的必要網路設定。
   
   > [!IMPORTANT]
   > 這些設定步驟必須在裝置的主動控制器上執行。 序列主控台功能表會在橫幅訊息中指出控制站狀態。 如果您未連接到主動控制器，請中斷連線，然後連接到主動控制器。
   > 
   > 
   
   1. 在命令提示字元中，輸入您的密碼。 預設裝置密碼是 **Password1**。
   2. 輸入以下命令：
      
        `Invoke-HcsSetupWizard`
   3. 安裝精靈將協助您設定裝置的網路設定。 請提供下列資訊：
      
      * 適用於 DATA 0 網路介面的 IP 位址
      * 子網路遮罩
      * 閘道器
      * 適用於主要 DNS 伺服器的 IP 位址
      * 適用於主要 NTP 伺服器的 IP 位址
      
      > [!NOTE]
      > 您可能需要等候幾分鐘，以套用子網路遮罩和 DNS 設定。
      > 
      > 
   4. 選擇性設定 Web Proxy 伺服器。
      
      > [!IMPORTANT]
      > 雖然 Web Proxy 設定是選用的，但請注意，如果您使用 Web Proxy，就只能在此處設定它。 如需詳細資訊，請參閱 [設定裝置的 Web Proxy](../articles/storsimple/storsimple-configure-web-proxy.md)。
      > 
      > 
6. 按 Ctrl + C 來結束安裝精靈。
7. 安裝更新，如下所示：
   
   1. 使用下列 Cmdlet 在兩個控制器上設定 IP：
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. 在命令提示字元下，執行 `Get-HcsUpdateAvailability`。 您應該會在更新可用時收到通知。
   3. 執行 `Start-HcsUpdate`。 您可以在任何模式中執行此命令。 更新會套用至第一個控制器，控制器會容錯移轉，然後更新會套用至其他控制器。
      
      您可以執行 `Get-HcsUpdateStatus` 以監視更新進度。    
      
      下列範例輸出顯示更新進行中。
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      下列範例輸出指出更新已完成。
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      可能需要多達 11 個小時來套用所有更新，包括 Windows Updates。
8. 執行下列 Cmdlet，將裝置指向 Microsoft Azure Government 入口網站 (因為預設指向公用 Azure 傳統入口網站)。 這樣會重新啟動兩個控制器。 建議您使用兩個 PuTTY 工作階段以同時連線至兩個控制器，這樣您就可以看見每個控制器是何時重新啟動。
   
    `Set-CloudPlatform -AzureGovt_US`
   
   您將會看見確認訊息。 接受預設值 (**Y**)。
9. 執行下列 Cmdlet 以繼續設定：
   
    `Invoke-HcsSetupWizard`
   
    ![繼續設定精靈](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   當您繼續設定時，精靈會是 Update 2 版本。
10. 接受網路設定。 在您接受每個設定之後，您會看到驗證訊息。
11. 基於安全性理由，裝置系統管理員密碼會在第一個工作階段之後過期，您必須立即變更。 出現提示時，請提供裝置系統管理員密碼。 有效的裝置系統管理員密碼長度必須介於 8 到 15 個字元。 密碼必須包含下列其中三項：大寫、小寫、數字和特殊字元。
    
    <br/>![StorSimple 註冊裝置 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. 安裝精靈的最後一個步驟是向 StorSimple Manager 服務註冊您的裝置。 基於此因素，您需要在[步驟 2：取得服務註冊金鑰中取得的服務註冊金鑰](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key)。 提供註冊金鑰之後，您可能需要等待 2-3 分鐘，才能註冊裝置。
    
    > [!NOTE]
    > 您可以隨時按 Ctrl + C 來結束安裝精靈。 如果您輸入所有網路設定 (Data 0 的 IP 位址、子網路遮罩和閘道器)，則會保留您的項目。
    > 
    > 
    
    ![StorSimple 註冊進度](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. 註冊裝置之後，隨即會出現服務資料加密金鑰。 複製這個金鑰，並將它儲存在安全的位置。 **這個金鑰需要與服務註冊金鑰搭配使用，來向 StorSimple Manager 服務註冊其他裝置。** 如需這個金鑰的詳細資訊，請參閱 [StorSimple 安全性](../articles/storsimple/storsimple-security.md) 。
    
    ![StorSimple 註冊裝置 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > 若要從序列主控台視窗複製文字，只需選取該文字。 然後您應該能夠將它貼到剪貼簿或任何文字編輯器中。
    > 
    > 請勿使用 Ctrl + C 來複製服務資料加密金鑰。 使用 Ctrl + C 將導致安裝精靈結束。 如此一來，裝置系統管理員密碼將不會變更，而裝置將還原為預設密碼。
    > 
    > 
14. 結束序列主控台。
15. 返回 Azure Government 入口網站，並完成下列步驟：
    
    1. 按兩下 StorSimple Manager 服務以存取 [快速入門]  頁面。
    2. 按一下 [檢視連接的裝置] 。
    3. 在 [裝置]  頁面上，藉由查閱狀態來確認裝置已成功連接到服務。 裝置狀態應該是 [線上] 。
       
        ![StorSimple 裝置頁面](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        如果裝置狀態為 [離線] ，請等待數分鐘，讓裝置上線。
       
        如果數分鐘之後裝置仍然離線，請確定您的防火牆網路已依照 [StorSimple 裝置網路需求](../articles/storsimple/storsimple-system-requirements.md)中的說明加以設定。
       
        請確認連接埠 9354 已開啟供輸出通訊使用，因為 StorSimple Manager 服務對裝置服務匯流排通訊也使用此連接埠。

