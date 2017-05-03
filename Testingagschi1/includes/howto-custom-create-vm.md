# <a name="how-to-create-a-custom-virtual-machine"></a>如何建立自訂虛擬機器 (英文)
*自訂*虛擬機器會參考您使用 [從資源庫] 方法建立的虛擬機器，因其比使用 [快速建立] 方法可讓您設定更多選項。 這些選項包含︰

* 更多用以建立虛擬機器 (VM) 的映像選擇
* 將 VM 連線到虛擬網路
* 將 VM 加入現有的雲端服務
* 將 VM 加入可用性設定組

> [!IMPORTANT]
> 如果要讓虛擬機器使用虛擬網路，以便依主機名稱來連接虛擬機器，或設定跨單位連線，則必須在建立虛擬機器時指定虛擬網路。 只有在建立虛擬機器時，才能將虛擬機器設定為加入虛擬網路。 如需虛擬網路的詳細資訊，請參閱 [Azure 虛擬網路概觀](http://go.microsoft.com/fwlink/p/?LinkID=294063)。
> 
> 

1. 登入 [Azure 入口網站](http://manage.windowsazure.com)。
2. 按一下命令列上的 [新增] 。
3. 依序按一下 [計算]、[虛擬機器] 及 [從資源庫]。
4. 選取您要使用的映像，然後按一下箭號繼續進行。
5. 如果有多個映像版本可供使用，請在 [版本發行日期] 中挑選您要使用的版本。
6. 在 [虛擬機器名稱] 中，輸入您要用於虛擬機器的名稱。
7. 使用 [階層] 與 [大小]，以選擇適用的虛擬機器大小。 您選取的大小會影響虛擬機器的最大組態與定價。 如需組態詳細資料，請參閱 [Azure 的虛擬機器和雲端服務大小](http://go.microsoft.com/fwlink/p/?LinkID=389844)。
8. 在 [新增使用者名稱] 中，輸入您要用於管理伺服器的系統管理帳戶名稱。
9. 在 [新增密碼] 中，輸入系統管理帳戶的強式密碼。 在 [確認密碼] 中，重新輸入相同密碼。
10. 按一下箭頭以繼續。
11. 在 [雲端服務] 中，執行下列其中一項：
    
    * 如果這是雲端服務中的第一個或唯一的虛擬機器，請選取 [Create a new cloud service] 。 然後，在 [Cloud Service DNS Name] 中，輸入使用 3 到 24 個小寫字母和數字的名稱。 透過雲端服務連絡虛擬機器時，使用的 URI 中將包含此名稱。
    * 如果要將此虛擬機器加入雲端服務，請在清單中選取它。
    
    > [!NOTE]
    > 如需關於將虛擬機器放在相同雲端服務中的詳細資訊，請參閱 [如何在雲端服務中連接虛擬機器](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/)。
    > 
    > 
12. 在 [區域/同質群組/虛擬網路] 中，選取您要用於虛擬機器的區域、同質群組或虛擬網路。 如需有關同質群組的詳細資訊，請參閱[關於虛擬網路的同質群組](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md)。
13. 在 [儲存體帳戶] 中，為 VHD 檔案選取現有的儲存體帳戶，或使用自動產生的儲存體帳戶。 每個區域只會自動建立一個儲存體帳戶。 您利用此設定建立的所有其他虛擬機器均位於此儲存體帳戶。 您的儲存體帳戶限制為 20 個。
14. 如果要將虛擬機器加入可用性設定組，請在 [可用性設定組] 中選取 [建立可用性設定組]，或新增至現有的可用性設定組。
    
    **注意**：可用性集合中的虛擬機器會部署到不同的容錯網域。 將多個虛擬機器放在相同的可用性設定組中，有助於確保應用程式在網路故障、本機磁碟硬體故障和任何規劃停機期間仍可使用。
15. 在 [端點] 下，檢閱將建立以允許連線至虛擬機器的新端點，例如透過遠端桌面或安全殼層 (SSH) 用戶端。 您也可以立即加入端點，或在稍後建立端點。 如需有關稍後建立端點的指示，請參閱[如何設定虛擬機器的端點](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。
16. 在 [VM Agent] 下，決定是否安裝 VM 代理程式。 此代理程式提供環境讓您安裝延伸模組，以協助您與虛擬機器互動。 如需詳細資訊，請參閱 [管理延伸模組](http://go.microsoft.com/FWLink/p/?LinkID=390493)。
17. 按一下箭號來建立虛擬機器。
    
    ![成功建立自訂虛擬機器](./media/howto-custom-create-vm/VMSuccessWindows.png)

## <a name="next-steps"></a>後續步驟
虛擬機器建立後，將會自動啟動。 當入口網站顯示狀態為執行中，您便可以登入虛擬機器。 如需指示，請參閱下列文章︰

* [如何登入執行 Linux 的虛擬機器](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [如何登入執行 Windows Server 的虛擬機器](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

