

建立 Azure 的 Web 應用程式專案時，您可以在 Azure 中佈建虛擬機器。 您可以接著使用其他軟體來設定虛擬機器，或將虛擬機器用於診斷或偵測用途。

若要在建立 Web 應用程式時建立虛擬機器，請依照下列步驟進行：

1. 在 Visual Studio 中，按一下 [檔案]  >  [新增]  >  [專案]  >  [Web]，然後選擇 [ASP.NET Web 應用程式] \(在 [Visual C#] 或 [Visual Basic] 節點底下)。
2. 在 [新增 ASP.NET 專案] 對話方塊中，選取您要的 Web 應用程式類型，然後在對話方塊的 Azure 區段 (位於右下角) 中，確定已勾選 [雲端中的主機] 核取方塊 (在某些安裝中，此核取方塊會標示為 [建立遠端資源])。
   
    ![][0]
3. 針對此範例，請在 Microsoft Azure 底下的下拉式清單方塊中，選擇 [虛擬機器 (v1)]，然後按一下 [確定] 按鈕。
4. 如果系統提示您登入，請登入 Azure。 [建立虛擬機器]  對話方塊隨即出現。
   
    ![][2]
5. 在 [DNS 名稱]  方塊中，輸入虛擬機器的名稱。 DNS 名稱在 Azure 中必須是唯一的。 如果您輸入的名稱無法使用，便會出現紅色的驚嘆號。
6. 在 [映像]  清單中，選擇您想要做為虛擬機器基礎的映像。 您可以選擇任何標準 Azure 虛擬機器映像，或是您已上傳到 Azure 的映像。
7. 除非您計畫安裝其他的 Web 伺服器，否則請將 [Enable IIS and Web Deploy]  核取方塊保持為已勾選。 如果停用 [Web 部署]，您將無法從 Visual Studio 發佈。 您可以將 IIS 和 Web 部署加入任何已封裝的 Windows Server 映像，包括您專屬的自訂映像。
8. 在 [大小]  清單中，選擇虛擬機器的大小。
9. 指定此虛擬機器的登入認證。 請記下這些資訊，因為您在透過遠端桌面存取機器時將會需要這些資訊。
10. 在 [位置]  清單中，選擇要裝載虛擬機器的區域。
11. 按一下 [確定]  按鈕開始建立虛擬機器。 您可以在 [輸出]  視窗中查看作業的進度。
    
    ![][3]
12. 佈建虛擬機器時，系統會在解決方案的 **PublishScripts** 節點中建立發佈的指令碼。 發佈的指令碼會在 Azure 中執行與佈建虛擬機器。 [輸出]  視窗會顯示狀態。 指令碼會執行下列動作以設定虛擬機器。
    
    * 如果虛擬機器不存在，則建立虛擬機器。
    * 只有當指定的區域中沒有名稱開頭為 `devtest`的儲存體帳戶存在時，才會建立此名稱的儲存體帳戶。
    * 為虛擬機器建立作為容器的雲端服務，並為 Web 應用程式建立 Web 角色。
    * 在虛擬機器上設定 Web 部署。
    * 在虛擬機器上設定 IIS 和 ASP.NET。
    
    ![][4]
13. (選擇性) 您可以連線到新的虛擬機器。 在**伺服器總管**中，展開**虛擬機器**節點，選擇您所建立的虛擬機器節點，並在其捷徑功能表上，選擇 [使用遠端桌面連線]。 或者，您可以在 [Cloud Explorer] 中的捷徑功能表上選擇 [在入口網站中開啟]，並連線至虛擬機器。
    
    ![][5]

## <a name="next-steps"></a>後續步驟
如果您想要自訂您所建立的已發佈指令碼，請閱讀 [使用 Windows PowerShell 指令碼發佈至開發和測試環境](http://msdn.microsoft.com/library/dn642480.aspx)中更深入的資訊。

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
