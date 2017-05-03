
<br>

> [!NOTE]
> 如果系統管理員已提供給您使用者名稱與密碼，很有可能您已經有工作或學校識別碼 (有時也稱做「組織識別碼」)。 如果是這樣，您可以立即開始使用您的 Azure 帳戶來存取需要該帳戶的 Azure資源。 如果您發現無法使用這些資源，您可能需要返回本文以尋求協助。 如需詳細資訊，請參閱[您用來登入的帳戶](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts)和 [Azure 訂用帳戶如何與 Azure AD 相關](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir)。
> 
> 

步驟很簡單。 您必須在 Azure 傳統入口網站中找到您已註冊的身分識別、找出您的預設 Azure Active Directory 網域，然後以 Azure 共同管理員的身分新增網域的新使用者。

## <a name="locate-your-default-directory-in-the-azure-classic-portal"></a>在 Azure 傳統入口網站中找到您的預設目錄
一開始先使用您個人的 Microsoft 帳戶身分識別登入 [Azure 傳統入口網站](https://manage.windowsazure.com) 。 登入後，向下捲動左側的藍色面板，然後按一下 [ACTIVE DIRECTORY] 。

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

讓我們開始在 Azure 中找出您身分識別的部分資訊。 您的主窗格應如下所示，顯示您有一個預設目錄。

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

讓我們找到更多的相關資訊。 按一下預設的目錄列，會將您帶往預設的目錄屬性。  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

若要檢視預設的網域名稱，請按一下 [網域] 。

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

您應該能夠在這裡看到 Azure 帳戶的建立時間，以及 Azure Active Directory 建立了個人的預設網域，此網域是用來做為 onmicrosoft.com 子網域的個人識別碼雜湊值 (這是從文字字串中產生的數字)。 這是您現在要新增新使用者的網域。

## <a name="creating-a-new-user-in-the-default-domain"></a>在預設網域中建立新的使用者
按一下 [使用者]，然後尋找您的單一個人帳戶。 您應該會在 [源自] 欄中看到其為 **Microsoft 帳戶**。 我們想要在您的預設 .onmicrosoft.com Azure Active Directory 網域中建立使用者。

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

我們將在接下來的步驟中遵循 [這些指示](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) 進行，但會使用特定的範例。

按一下頁面底端的 [ **+新增使用者**]。 在隨即出現的頁面中，輸入新的使用者名稱，並將 [使用者類型] 設為 [您組織中的新使用者]。 在此範例中，使用者名稱為 `ahmet`。 選取您先前找到的預設網域，以做為 ahmet 電子郵件地址的網域。 完成時按一下 [下一步] 箭頭。

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

加入更多關於 Ahmet 的詳細資訊，但務必選取適當的 [角色] 值。 您可以很輕鬆地使用**全域管理員**確定系統正在運作中，但如果您可以使用權限較低的角色，也是不錯的主意。 此範例採用**使用者**角色。 (在[依據角色的系統管理員權限](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1)中取得更多資訊。)除非您想要針對運作中的每個記錄使用多重要素驗證，否則請勿啟用多重要素驗證。 當您完成時，按一下 [下一步] 箭頭。

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

按一下 [建立]  按鈕，以產生並顯示 Ahmet 的暫時密碼。

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

複製使用者名稱的電子郵件地址，或使用 [透過電子郵件傳送密碼] 。 您需要該資訊才能立即登入。

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

現在您應該會看到新的使用者，源自 Azure Active Directory 的 **Ahmet 這位開發人員**。 您已使用 Azure Active Directory 建立新的工作或學校身分識別。 不過，這個身分識別還沒有使用 Azure 資源的權限。

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

如果您使用 [透過電子郵件傳送密碼] ，即會傳送下列種類的電子郵件。

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a>為訂用帳戶新增 Azure 的共同管理員權限
現在您必須將新的使用者新增成為訂用帳戶的共同管理員，新的使用者才能夠登入管理入口網站。 若要這樣做，請在左下角面板中按一下 [設定] 。

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

在主要設定區域中，按一下頂端的 [ **管理員** ]，您應該只會看到您個人的 Microsoft 帳戶身分識別。 在頁面底端按一下 [ **+新增** ] 以指定共同管理員。 在此處輸入您所建立新使用者的電子郵件地址，包括預設網域。 如下一個螢幕擷取畫面所示，綠色勾號會出現在預設目錄的使用者旁邊。 請記得選取您想要讓此使用者能夠管理的所有訂用帳戶。

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

完成後，您現在應會看到兩個使用者，包括新的共同管理員身分識別。 登出入口網站。

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-the-new-users-password"></a>登入並變更新使用者的密碼
以您建立的新使用者身分登入。

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

會立即提示您建立新的密碼。

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

您應該會看到如下的成功獎勵。

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a>後續步驟
您現在可以使用新的 Azure Active Directory 身分識別來使用 [Azure 資源群組範本](../articles/xplat-cli-azure-resource-manager.md)。

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
