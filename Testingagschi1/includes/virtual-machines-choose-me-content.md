<a name="tellmevm"></a>

## <a name="tell-me-about-virtual-machines"></a>我想了解虛擬機器
Azure 虛擬機器可讓您在雲端中建立和使用虛擬機器。 提供所謂的「基礎結構即服務 (IaaS)」 ，虛擬機器技術可用於許多方面。 部分範例如下：

* **適用於開發和測試用途的虛擬機器 (VM)** 開發群組通常會使用 VM，因為它們提供了一個快速、簡單的方法，用以建立為應用程式撰寫程式碼並進行測試時所需之特定組態的電腦。 Azure 虛擬機器能夠以直接且經濟的方式建立和使用這些 VM，並且於不再需要時加以刪除。
* **在雲端執行應用程式。** 在公用雲端中執行某些應用程式符合經濟效益考量。 例如，具有大型尖峰需求的應用程式。 雖然您能夠建置擁有足夠硬體來因應尖峰需求的資料中心，但硬體可能在大部分的時間都處於使用量很低的狀態。 在 Azure 上執行此應用程式可讓您在需要時支付額外的 VM，而在您不需要時關閉它們。 另外，假設您立即需要隨選運算資源，但是不想受約束。 同樣地，Azure 會是正確的選擇。
* **將您自己的資料中心擴展到公用雲端。** 當您使用 Azure 虛擬網路時，您的組織可以建立虛擬網路 (VNET) 做為您內部部署網路的延伸，並新增 VM 至該 VNET。 這可讓您在 Azure VM 上執行應用程式，例如 [SharePoint](../articles/virtual-machines/windows/sharepoint-farm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)、[SQL Server](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) 等等。 這種方法可能比在您資料中心的 VM 中執行還要更容易部署或成本較低。   
* **災害復原。** IaaS 型災害復原可以讓您在確實需要運算資源時才付費使用，而不需要將成本持續浪費在不常使用的備份資料中心上。  例如，如果主要資料中心故障，您可以建立在 Azure 上執行的 VM 來執行基本的應用程式，然後於不再需要時關閉這些應用程式。

就像其他虛擬機器一樣，Azure 中的 VM 具有作業系統、儲存體和網路功能，而且可以執行各式各樣的應用程式。 您可以使用 Azure 或其合作夥伴之一所提供的映像，或使用您自己的映像。 其中包含下列的各種版本、版次和組態：

* Linux 伺服器，例如 Suse、Ubuntu 和 CentOS
* Windows Server 
* SQL Server
* BizTalk Server 
* SharePoint Server

虛擬機器是使用虛擬硬碟 (VHD) 來儲存其作業系統 (OS) 和資料。 VHD 也能夠使用於您可以選擇用來安裝 OS 的映像。 下圖說明此特點，以及用來建立及管理您的 VM 的兩項工具。

<a name="fig_createvms"></a>
![vm_diagram](./media/virtual-machines-choose-me-content/diagram.png)

**圖：Azure 虛擬機器提供「基礎結構即服務」。**

可以使用以瀏覽器為基礎的入口網站、支援指令碼處理的命令列工具，或直接透過 REST API 管理 VM。 RightScale 及 ScaleXtreme 等 Microsoft 合作夥伴也提供透過 REST API 進行的管理服務。 

除了作業系統以外，VM 的其他設定選項包括：

* 大小，這可決定像是您可以連接多少磁碟和處理能力等項目的因素。 Azure 提供了各種不同的大小，以支援許多類型的用法。 如需詳細資訊，請參閱 [虛擬機器的大小](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。  
* 將裝載您新 VM 的 Azure 區域，例如美國、歐洲或亞洲。 
* VM 延伸模組，可提供您虛擬機器額外功能，例如執行防毒軟體，或使用 Windows PowerShell 的期望狀態設定功能。

VM 應考量的其他優點包括：

**預付方案** -- Azure 可依據 VM 的大小和作業系統，以每小時價格方式收費。 針對不足一小時的部分，Azure 只會收取使用分鐘數的費用。 儲存體是個別定價與收費。 如需詳細資訊，請參閱 [虛擬機器定價](https://azure.microsoft.com/pricing/details/virtual-machines/)。

**恢復功能** -- Azure 會監視裝載各個執行中 VM 的實體硬體。 如果執行 VM 的實體伺服器失敗，Azure 會在發現這個狀況時，將 VM 移至新硬體並重新啟動 VM。 此程序有時稱為服務修復。 Azure 也會透過在 Blob 儲存體中保留 VHD 的備援複本，以保護虛擬機器的資料。 

