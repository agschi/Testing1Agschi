

Azure 是用來實作 dev/test 或概念證明設定的絕佳平台，因為它不需要太多投資，就能測試實作解決方案的特定方法。 但是，您必須能夠區分 dev/test 環境的簡單做法，以及更困難且詳細的做法，後者適用於 IT 工作負載之功能完整且準備好用於生產的實作。

這個指導方針識別出許多領域，而規劃這些領域是 Azure 中 IT 工作負載成功的關鍵。 此外，規劃還提供建立必要資源的順序。 儘管有一些彈性，但是我們建議您在規劃與進行決策時遵循這篇文章裡的順序。

這篇文章摘錄自〈 [Azure 實作方針](http://blogs.msdn.com/b/thecolorofazure/archive/2014/05/13/azure-implementation-guidelines.aspx) 〉部落格文章的內容。 感謝 Santiago Cánepa (Microsoft 應用程式開發經理) 和 Hugo Salcedo (Microsoft 應用程式開發經理) 提供的原始資料。

> [!NOTE]
> 同質群組已取代。 此處未描述其用法。 如需詳細資訊，請參閱 [關於區域 VNet 和同質群組](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md)。
> 
> 

## <a name="1-naming-conventions"></a>1.命名慣例
在 Azure 中建立任何項目之前，應先建立良好的命名慣例。 命名慣例可確保所有資源都擁有可預測的名稱，有助於降低與這些資源管理相關聯的系統管理負擔。

您可以選擇遵循為整個組織所定義的一組特定命名慣例，或適用於特定 Azure 訂用帳戶或帳戶的命名慣例。 儘管對組織內的個人來說，在使用 Azure 資源時建立隱含規則非常容易，但當小組需要在 Azure 上致力於某個專案時，該模型並不會適當地調整範圍。

您應該預先針對命名慣例組合取得一致的意見。 有些關於命名慣例的考量，會影響組成這些慣例的規則組合。

### <a name="affixes"></a>詞綴
建立特定資源時，Azure 會使用某些預設值來簡化與這些資源相關聯的資源管理。 例如，在為新的雲端服務建立第一部虛擬機器時，Azure 傳統入口網站會嘗試針對虛擬機器的新雲端服務名稱使用該虛擬機器的名稱。

因此，有利於識別需要詞綴來識別該類型的資源類型。 此外，請明確指定詞綴的所在位置：

* 名稱開頭 (前置)
* 名稱結尾 (後置)

例如，以下是兩個適用於裝載計算引擎之資源群組的可能名稱：

* Rg-CalculationEngine (前置)
* CalculationEngine-Rg (後置)

詞綴指的是可說明特定資源的各個層面。 下列表格顯示一些常用範例。

| 層面 | 範例 | 注意事項 |
| --- | --- | --- |
| Environment |dev、stg、prod |取決於每個環境的用途與名稱。 |
| 位置 |usw (West US)、use (East US 2) |取決於資料中心的區域或組織的區域。 |
| Azure 元件、服務或產品 |Rg (適用於資源群組)、Svc (適用於雲端服務)、VNet (適用於虛擬網路) |取決於資源提供支援的產品。 |
| 角色 |sql、ora、sp、iis |取決於虛擬機器的角色。 |
| 執行個體 |01、02 和 03 等 |適用於有一個以上執行個體的資源。 例如，雲端服務中負載平衡的 Web 伺服器。 |

建立命名慣例時，請確定它們會清楚描述要針對每個資源類型使用哪些詞綴，以及使用的位置 (前置或後置)。

### <a name="dates"></a>日期
從資源名稱來判斷建立日期通常非常重要。 我們建議使用 YYYYMMDD 日期格式。 此格式可確保不僅會記錄完整的日期，而且這兩個名稱只有日期不同的資源將同時以依字母順序和依時間先後順序的方式來排序。

### <a name="naming-resources"></a>命名資源
您應該在命名慣例中定義每個資源類型，慣例中應包含規則來定義為每個所建立資源指派名稱的方式。 這些規則應套用到所有資源類型，例如：

* 訂用帳戶
* 帳戶
* 儲存體帳戶
* 虛擬網路
* 子網路
* 可用性設定組
* 資源群組
* 雲端服務
* 虛擬機器
* 端點
* 網路安全性群組
* 角色

若要確保名稱可提供足夠的資訊來判斷其指稱的是哪一個資源，您應使用描述性名稱。

### <a name="computer-names"></a>電腦名稱
當系統管理員建立虛擬機器時，Microsoft Azure 會要求他們提供最多 15 個字元的虛擬機器名稱。 Azure 會使用該虛擬機器名稱，做為 Azure 虛擬機器資源名稱。 Azure 會使用與虛擬機器上所安裝作業系統之電腦名稱相同的名稱。 但是，這些名稱不一定相同。

若虛擬機器是從已經包含作業系統的 .vhd 映像檔中所建立，Azure 中的虛擬機器名稱可能會與虛擬機器的作業系統電腦名稱不同。 這種情況可能會增加虛擬機器管理的困難度，因此我們不建議使用。 將 Azure 虛擬機器資源的名稱指派為該虛擬機器作業系統上所指派的相同電腦名稱。

我們建議讓 Azure 虛擬機器名稱與基礎作業系統電腦名稱相同。 因此，請遵循〈 [Microsoft NetBIOS 電腦命名慣例](https://support.microsoft.com/kb/188997/)〉中所述的 NetBIOS 命名規則。

### <a name="storage-account-names"></a>儲存體帳戶名稱
儲存體帳戶具備負責管理其名稱的特殊規則。 您只能使用小寫字母和數字。 如需詳細資訊，請參閱 [建立儲存體帳戶](../articles/storage/storage-create-storage-account.md#create-a-storage-account) 。 此外，結合 core.windows.net 的儲存體帳戶名稱應該是全域有效且唯一的 DNS 名稱。 例如，如果儲存體帳戶名稱為 mystorageaccount，則以下產生的 DNS 名稱應該是唯一的：

* mystorageaccount.blob.core.windows.net
* mystorageaccount.table.core.windows.net
* mystorageaccount.queue.core.windows.net

### <a name="azure-building-block-names"></a>Azure 建置組塊名稱
Azure 建置組塊是 Azure 提供的應用程式層級服務，儘管 IaaS 資源可能會用到某些服務 (例如 SQL Database、流量管理員及其他功能)，但通常是提供給利用 PaaS 功能的應用程式。

這些服務依賴在 Azure 中建立並註冊的構件陣列。 您還需要在命名慣例中將它們納入考量。

### <a name="implementation-guidelines-recap-for-naming-conventions"></a>複習實作命名慣例的指導方針
決策：

* 哪些是您的 Azure 資源命名慣例？

工作：

* 就詞綴、階層、字串值及其他適用於 Azure 資源的規則等方面來定義命名慣例。

## <a name="2-subscriptions-and-accounts"></a>2.訂用帳戶與帳戶
若要使用 Azure，您需要一個以上的 Azure 訂用帳戶。 像是雲端服務或虛擬機器的資源存在於這些訂用帳戶的內容中。

* 企業客戶通常會有 Enterprise 註冊，這是階層中最上層的資源，而且會與一或多個帳戶相關聯。
* 對於消費者與不具 Enterprise 註冊的客戶來說，最上層的資源是帳戶。
* 訂用帳戶會關聯至帳戶，而且每個帳戶可以有一或多個訂用帳戶。 Azure 會在訂用帳戶層級記錄計費資訊。

由於帳戶/訂用帳戶關係中有兩個階層層級的限制，因此，請務必將帳戶和訂用帳戶的命名慣例與計費需求保持一致。 舉例來說，如果有一家全球性公司使用 Azure，他們可能選擇針對每個區域擁有一個帳戶，並在區域層級管理訂用帳戶。

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

例如，您可以使用下列結構。

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

根據同一個範例，如果某個區域決定擁有一個以上與特定群組相關聯的訂用帳戶，則命名慣例必須包括在帳戶或訂用帳戶名稱上為額外項目進行編碼的方式。 這個組織允許訊息傳遞計費資料在計費報告期間產生新的階層層級。

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

組織應看起來如下。

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Microsoft 透過可下載的檔案，針對單一帳戶或企業合約中的所有帳戶提供詳細的計費資訊。 您可以處理這個檔案 (例如，使用 Microsoft Excel)。 這個程序會內嵌資料將資源分割為個別欄 (此資源會為一個以上的階層層級進行編碼)，然後使用樞紐分析表或 PowerPivot 來提供動態報告功能。

### <a name="implementation-guidelines-recap-for-subscriptions-and-accounts"></a>複習實作訂用帳戶和帳戶的指導方針
決策：

* 您需要裝載 IT 工作負載或基礎結構的是哪種訂用帳戶與帳戶的組合？

工作：

* 使用您的命名慣例來建立訂用帳戶與帳戶的組合。

## <a name="3-storage"></a>3.儲存體
Azure 儲存體是許多 Azure 解決方案不可或缺的部分。 Azure 儲存體提供服務來儲存檔案資料、非結構化資料和訊息，同時也是支援虛擬機器的基礎結構的一部分。

Azure 中有兩種可用的儲存體帳戶。 標準儲存體帳戶可供您存取 blob 儲存體 (用於存放 Azure 虛擬機器磁碟)、資料表儲存體、佇列儲存體和檔案儲存體。 進階儲存體是設計來供高效能的應用程式使用 (例如，AlwaysOn 叢集中的 SQL Server)，目前僅支援 Azure 虛擬機器磁碟。

儲存體帳戶會繫結至延展性目標。 請參閱〈 [Microsoft Azure 訂用帳戶及服務限制、配額與限制](../articles/azure-subscription-service-limits.md#storage-limits) 〉，以熟悉目前的 Azure 儲存體限制。 另請參閱〈 [Azure 儲存體的延展性與效能目標](../articles/storage/storage-scalability-targets.md)〉。

Azure 會建立含有一個作業系統磁碟、一個暫存磁碟，以及零或多個選用資料磁碟的虛擬機器。 作業系統磁碟和資料磁碟都是 Azure 頁面 Blob，而暫存磁碟會儲存在本機機器所在的節點上。 這會使得暫存磁碟不適合用於系統回收期間必須持續存在的資料，因為該虛擬機器可能會以無訊息模式從某個節點移轉到其他節點，因而遺失該磁碟上的所有資料。 請勿在暫存磁碟機上儲存任何資料。

作業系統磁碟和資料磁碟的大小上限為 1023 GB (一個 GB 是 1024<sup>3</sup> 位元組)，因為 blob 的大小上限為 1024 GB 且必須包含 VHD 檔案的中繼資料 (頁尾)。 您可以在 Windows 中實作磁碟等量來超越這個限制。

### <a name="striped-disks"></a>等量磁碟
除了能夠建立大於 1023 GB 的磁碟，在許多情況下，針對資料磁碟使用等量，可藉由允許多個 blob 備份單一磁碟區的儲存體來增強效能。 從單一邏輯磁碟寫入和讀取資料所需的 I/O 會透過等量速度以平行方式繼續進行。

根據虛擬機器的大小而定，Azure 會強制限制資料磁碟數量和可用頻寬。 如需詳細資訊，請參閱 [虛擬機器的大小](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

如果您針對 Azure 資料磁碟使用磁碟等量，請考量下列指導方針：

* 資料磁碟應一律為最大大小 (1023 GB)
* 連結虛擬機器大小所允許的資料磁碟數目上限
* 使用儲存體空間組態
* 使用儲存體等量組態
* 避免使用 Azure 資料磁碟快取選項 (快取原則 = 無)

如需詳細資訊，請參閱〈 [儲存體空間 - 適用於效能的設計](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx)〉。

### <a name="multiple-storage-accounts"></a>多個儲存體帳戶
使用多個儲存體帳戶來備份與多個虛擬機器相關聯的磁碟，以確保這些磁碟的彙總 I/O 會完全低於適用於這其中每一個儲存體帳戶的延展性目標。

我們建議您一開始先為每個儲存體帳戶部署一部虛擬機器。

### <a name="storage-layout-design"></a>儲存體配置設計
為了實作這些策略以實作虛擬機器的磁碟子系統並使其具備良好效能，IT 工作負載或基礎結構通常會利用多個儲存體帳戶。 這些儲存體帳戶將會裝載多個 VHD blob。 在某些情況下，會有一個以上的 Blob 關聯至一部虛擬機器中的單一磁碟區。

這種情況會增加管理工作的困難度。 設計適用於儲存體的健全策略 (包括適當命名基礎磁碟和相關聯的 VHD Blob) 就是關鍵所在。

### <a name="implementation-guidelines-recap-for-storage"></a>複習實作儲存體的指導方針
决策：

* 您需要進行磁碟等量以建立大於 500 TB 的磁碟嗎？
* 您需要進行磁碟等量以獲得工作負載的最佳效能嗎？
* 您需要用來裝載 IT 工作負載或基礎結構的是哪種儲存體帳戶組合？

工作：

* 使用您的命名慣例來建立儲存體帳戶組合。 您可以使用 Azure入口網站、Azure 傳統入口網站或 **New-AzureStorageAccount** PowerShell Cmdlet。

## <a name="4-cloud-services"></a>4.雲端服務
雲端服務是 Azure 服務管理中的基本建置組塊，適用於 PaaS 和 IaaS 服務。 對於 PaaS，雲端服務代表其執行個體可以彼此通訊之角色的關聯。 雲端服務會關聯至公開虛擬 IP (VIP) 位址和負載平衡器，其會取得來自網際網路的連入流量，並將它負載平衡至設定來接收該流量的角色。

在 IaaS 的案例中，雲端服務可提供類似功能，儘管在大多數案例中，負載平衡器的功能是用來將流量轉接到特定的 TCP 或 UDP 連接埠 (從網際網路到該雲端服務內的多部虛擬機器)。

> [!NOTE]
> 雲端服務不存在於 Azure 資源管理員中。 如需資源管理員優點的介紹，請參閱〈 [Azure 資源管理員提供的 Azure 運算、網路和儲存體提供者](../articles/virtual-machines/virtual-machines-windows-compare-deployment-models.md)〉。
> 
> 

雲端服務名稱在 IaaS 中特別重要，因為 Azure 會使用它們做為磁碟預設命名慣例的一部分。 雲端服務名稱只能包含字母、數字和連字號。 欄位中的第一個和最後一個字元，必須是字母或數字。

Azure 會公開雲端服務名稱，因為它們會關聯至網域 “cloudapp.net” 中的 VIP。 為了得到更好的應用程式使用者體驗，您應該視需要設定虛名，來取代完整格式的雲端服務名稱。 這通常是使用您公用 DNS 中的 CNAME 記錄來完成，其會將來源的公用 DNS 名稱 (例如 www.contoso.com) 對應至裝載資源之雲端服務的 DNS 名稱 (例如，裝載適用於 www.contoso.com 之 Web 伺服器的雲端服務)。

此外，用於雲端服務的命名慣例可能需要容許例外狀況，因為不論 Microsoft Azure 租用戶為何，雲端服務名稱在所有其他 Microsoft Azure 雲端服務中是唯一的。

雲端服務必須考量的一個重要限制是，對於雲端服務中的所有虛擬機器一次只能執行一個虛擬機器管理作業。 當您對雲端服務中的一個虛擬機器執行虛擬機器管理作業時，您必須等候它完成才能對另一個虛擬機器執行新的管理作業。 因此，您應該保持雲端服務中虛擬機器為較少的數量。

Azure 訂用帳戶最多可支援 200 個雲端服務。

### <a name="implementation-guidelines-recap-for-cloud-services"></a>複習實作雲端服務的指導方針
決策：

* 您需要用來裝載 IT 工作負載或基礎結構的是哪種雲端服務組合？

工作：

* 使用您的命名慣例來建立雲端服務組合。 您可以使用 Azure 傳統入口網站或 **New-AzureService** PowerShell Cmdlet。

## <a name="5-virtual-networks"></a>5.虛擬網路
下一個邏輯步驟是在解決方案中建立支援跨虛擬機器進行通訊所需的虛擬網路。 儘管能夠只在一個雲端服務內為 IT 工作負載裝載多部虛擬機器，但還是建議您使用虛擬網路。

虛擬網路是適用於虛擬機器的容器，您也可以為其指定子網路、自訂位址，以及 DNS 設定選項。 位於相同虛擬網路的虛擬機器可以直接與同一個虛擬網路內的其他電腦通訊，而不論它們是屬於哪一個雲端服務的成員。 在虛擬網路中，這個通訊會維持私人狀態，因而不需透過公用端點進行通訊。 如果虛擬機器會連線到公司網路，則此通訊可使用安裝於虛擬網路中或內部部署的 DNS 伺服器，透過 IP 位址或依名稱來進行。

### <a name="site-connectivity"></a>站台連線能力
如果內部部署的使用者和電腦不需持續連線到 Azure 虛擬網路中的虛擬機器，則可建立僅限雲端的虛擬網路。

![](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

這通常適用於網際網路對應的工作負載，例如，以網際網路為基礎的 Web 伺服器。 您可以使用遠端桌面連線、遠端 PowerShell 工作階段、安全殼層 (SSH) 連線，以及點對站 VPN 連線來管理這些虛擬機器。

由於它們不會連線到您的內部部署網路，因此，僅限雲端的虛擬網路可以使用任何部分的私人 IP 位址空間。

如果內部部署的使用者和電腦要求持續連線到 Azure 虛擬網路中的虛擬機器，請建立跨單位的虛擬網路，然後使用 ExpressRoute 或網站間 VPN 連線，將它連線到您的內部部署網路。

![](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

在這個設定中，Azure 虛擬網路本質上是您的內部部署網路中具備雲端功能的延伸模組。

由於它們會連線到您的內部部署網路，因此，跨單位的虛擬網路必須使用組織所使用的一部分位址空間 (它是唯一的)，而且路由基礎結構必須支援路由到該部分的流量，做法是將它轉送到您的內部部署 VPN 裝置。

若要允許封包從跨單位的虛擬網路傳輸到內部部署網路，您必須設定相關的內部部署位址首碼組合，來做為適用於虛擬網路之區域網路定義的一部分。 根據虛擬網路的位址空間及相關的內部部署位置組合而定，區域網路中可以有多個位址首碼。

您可以將僅限雲端的虛擬網路轉換為跨單位的虛擬網路，但是，它很可能會要求您為虛擬網路位址空間、子網路，以及使用 Azure 指派的靜態 IP 位址 (稱為動態 IP，DIP) 重新編號。 因此，在建立虛擬網路之前，請謹慎考量您所需的虛擬網路類型 (僅限雲端與跨單位)。

### <a name="subnets"></a>子網路
子網路允許您組織相關的資源，可能是邏輯上相關 (例如，一個適用於與同一個應用程式相關聯之虛擬機器的子網路)，也可能是實際上相關 (例如，每個雲端服務一個子網路)，或者採用子網路隔離技術來提高安全性。

對於跨單位的虛擬網路，應該使用您針對內部部署資源所使用的相同慣例來設計子網路，請記住， **Azure 一律會針對每個子網路使用位址空間的前三個 IP 位址**。 若要決定子網路所需的位址數目，請計算您現在所需的虛擬機器數目、估計未來成長量，然後使用下列表格來決定子網路的大小。

| 所需的虛擬機器數目 | 所需的主機位元數 | 子網路的大小 |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> 針對一般的內部部署子網路，適用於含有 n 個主機位元之子網路的主機位址數目上限是 2<sup>n</sup> – 2。 針對 Azure 子網路，適用於含有 n 個主機位元之子網路的主機位址數目上限是 2<sup>n</sup> – 5 (2 加 3 適用於 Azure 在每個子網路上使用的位址)。
> 
> 

如果您選擇的子網路大小太小，就必須在子網路中為虛擬機器重新編號並重新部署。

### <a name="implementation-guidelines-recap-for-virtual-networks"></a>複習實作虛擬網路的指導方針
决策：

* 您需要使用何種類型的虛擬網路來裝載 IT 工作負載或基礎結構 (僅限雲端或跨單位部署)？
* 如果是跨單位的虛擬網路，您現在需要多少位址空間來裝載子網路和虛擬機器，並且在未來進行合理範圍的擴充？

工作：

* 定義適用於虛擬網路的位址空間。
* 定義子網路組合以及適用於每個子網路的位址空間。
* 針對跨單位的虛擬網路，定義適用於內部部署位置的區域網路位址空間組合，而虛擬網路中的虛擬機器需要連接這類位置。
* 使用您的命名慣例來建立虛擬網路。 您可以使用 Azure 入口網站或 Azure 傳統入口網站。

## <a name="6-availability-sets"></a>6.可用性集合
在 Azure PaaS 中，雲端服務包含一或多個角色來執行應用程式程式碼。 角色可擁有一或多個虛擬機器執行個體，而網狀架構會自動佈建這些執行個體。 Azure 可能會在任何指定的時間更新這些角色中的執行個體，但是，由於它們屬於相同的角色，因此，Azure 知道不能同時更新它們全部，以防止角色的服務中斷。

在 Azure IaaS 中，角色的概念並不重要，因為每一部 IaaS 虛擬機器都代表含有單一執行個體的角色。 為了暗示 Azure 不要同時關閉一或多部相關聯的機器 (例如，針對其所在節點進行的作業系統更新)，因而引進了可用性集合的概念。 可用性集合會告知 Azure 不要同時關閉同一個可用性集合中的所有機器，以防止服務中斷。 可用性集合的虛擬機器成員具備 99.95% 執行時間服務等級協定。

可用性集合必須是解決方案中高可用性規劃的一部分。 可用性集合會定義為單一雲端服務內的虛擬機器組合，其具備相同的可用性集合名稱。 您可以在建立雲端服務之後建立可用性集合。

### <a name="implementation-guidelines-recap-for-availability-sets"></a>複習實作可用性集合的指導方針
決策：

* 在您的 IT 工作負載或基礎結構中，需要針對各種角色和層級使用多少個可用性集合？

工作：

* 使用您的命名慣例來定義可用性集合的組合。 您可以在建立虛擬機器時，將該虛擬機器關聯至可用性集合，或者可以在建立虛擬機器之後，將該虛擬機器關聯至可用性集合。

## <a name="7-virtual-machines"></a>7.虛擬機器
在 Azure PaaS 中，Azure 會管理虛擬機器及其相關聯的磁碟。 您必須建立雲端服務和角色並為其命名，然後 Azure 會建立與這些角色相關聯的執行個體。 在 Azure IaaS 的案例中，您可以決定是否要為雲端服務、虛擬機器及相關聯的磁碟提供名稱。

為了降低管理負擔，Azure 傳統入口網站會使用電腦名稱做為相關聯雲端服務的預設名稱建議 (在此案例中，客戶選擇建立新的雲端服務做為虛擬機器建立精靈的一部分)。

此外，Azure 會使用雲端服務名稱、電腦名稱及建立日期的組合，來為磁碟及其支援的 VHD Blob 命名。

一般來說，磁碟數目會比虛擬機器數目多很多。 您應該謹慎操作虛擬機器，以防止損壞磁碟。 此外，您可以在不刪除支援的 Blob 的情況下刪除磁碟。 如果出現這種情況，該 blob 將保留於儲存體帳戶中，直到您手動刪除為止。

### <a name="implementation-guidelines-recap-for-virtual-machines"></a>複習實作虛擬機器的指導方針
決策：

* 您需要針對 IT 工作負載或基礎結構提供多少部虛擬機器？

工作：

* 使用您的命名慣例來定義每個虛擬機器名稱。
* 使用 Azure 入口網站、Azure 傳統入口網站、 **New-AzureVM** PowerShell Cmdlet、Azure CLI 或資源管理員範本來建立虛擬機器。

## <a name="example-of-an-it-workload-the-contoso-financial-analysis-engine"></a>IT 工作負載的範例：Contoso 財務分析引擎
Contoso Corporation 開發了新一代財務分析引擎，並具備尖端的專屬演算法，以瞄準未來的市場貿易。 他們想要使其客戶能夠在 Azure 中使用這個引擎來做為一組伺服器，其中包括：

* 兩部 (最終會更多部) 以 IIS 為基礎的 Web 伺服器，其會在 Web 層中執行自訂的 Web 服務
* 兩部 (最終會更多部) 以 IIS 為基礎的應用程式伺服器，其會在應用程式層中執行計算
* 含有 AlwaysOn 可用性群組的 SQL Server 2014 叢集 (有兩部 SQL Server 和一個多數節點見證)，可在資料庫層中儲存歷程和持續進行計的計算資料
* 在驗證層中有兩個適用於自封式樹系和網域的Active Directory 網域控制站，這是透過 SQL Server 叢集來要求
* 所有的伺服器都位於兩個子網路上；適用於 Web 伺服器的前端子網路和適用於應用程式伺服器的後端子網路、SQL Server 2014 叢集，以及網域控制站

![](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

在網際網路上，從 Contoso 用戶端連入的安全 Web 流量需要在 Web 伺服器之間達成負載平衡。 利用來自 Web 伺服器的 HTTP 要求形式計算要求流量，需要在應用程式伺服器之間達成負載平衡。 此外，該引擎必須設計為高可用性。

所產生的設計必須包含下列各項：

* Contoso Azure 訂用帳戶和帳戶
* 儲存體帳戶
* 具有兩個子網路的虛擬網路
* 適用於具備類似角色之伺服器組合的可用性集合
* 虛擬機器
* 單一資源群組

以上各項將遵循下列 Contoso 命名慣例：

* Contoso 使用 [IT workload]-[location]-[Azure resource] 做為首碼。 針對此範例，"azfae" (Azure 財務分析引擎) 是 IT 工作負載名稱，而 "use" (East US 2) 是位置，因為 Contoso 大多數最初的客戶都位於美國東岸。
* 儲存體帳戶會使用 contosoazfaeusesa[description] 請注意，已將 contoso 新增為首碼來提供唯一性，而儲存體帳戶名稱不支援使用連字號。
* 虛擬網路會使用 AZFAE-USE-VN[number]。
* 可用性集合會使用 azfae-use-as-[role]。
* 虛擬機器名稱會使用 azfae-use-vm-[vmname]。

### <a name="azure-subscriptions-and-accounts"></a>Azure 訂用帳戶與帳戶
Contoso 正在使用名稱為 Contoso Enterprise Subscription 的企業訂用帳戶來提供這個 IT 工作負載的計費。

### <a name="storage-accounts"></a>儲存體帳戶
Contoso 判斷他們需要兩個儲存體帳戶：

* **contosoazfaeusesawebapp** ，適用於 Web 伺服器、應用程式伺服器，以及網域控制站及其額外資料磁碟的標準儲存體
* **contosoazfaeusesasqlclust** ，適用於 SQL Server 叢集伺服器及其額外資料磁碟的高階儲存體

### <a name="a-virtual-network-with-subnets"></a>具有子網路的虛擬網路
由於虛擬網路不需要持續連線到 Contoso 內部部署網路，因此，Contoso 決定使用僅限雲端的虛擬網路。

他們透過 Azure 入口網站，使用下列設定來建立僅限雲端的虛擬網路：

* 名稱：AZFAE-USE-VN01
* 位置：East US 2
* 虛擬網路位址空間：10.0.0.0/8
* 第一個子網路：
  * 名稱：FrontEnd
  * 位址空間：10.0.1.0/24
* 第二個子網路：
  * 名稱：BackEnd
  * 位址空間：10.0.2.0/24

### <a name="availability-sets"></a>可用性集合
為了維持這四個財務分析引擎層級的高可用性，Contoso 決定使用四個可用性集合：

* **azfae-use-as-dc** ，適用於網域控制站
* **azfae-use-as-web** ，適用於 Web 伺服器
* **azfae-use-as-app** ，適用於應用程式伺服器
* **azfae-use-as-sql** ，適用於 SQL Server 叢集中的伺服器

這些可用性集合將與虛擬機器一同建立。

### <a name="virtual-machines"></a>虛擬機器
Contoso 決定為其 Azure 虛擬機器使用下列名稱：

* **azfae-use-vm-dc01** ，適用於第一個網域控制站
* **azfae-use-vm-dc02** ，適用於第二個網域控制站
* **azfae-use-vm-web01** ，適用於第一部 Web 伺服器
* **azfae-use-vm-web02** ，適用於第二部 Web 伺服器
* **azfae-use-vm-app01** ，適用於第一部應用程式伺服器
* **azfae-use-vm-app02** ，適用於第二部應用程式伺服器
* **azfae-use-vm-sql01** ，適用於 SQL Server 叢集中的第一部 SQL Server
* **azfae-use-vm-sql02** ，適用於 SQL Server 叢集中的第二部 SQL Server
* **azfae-use-vm-sqlmn01** ，適用於 SQL Server 叢集中的多數節點見證

以下是產生的組態。

![](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

這個設定包含下列各項：

* 含有兩個子網路 (前端和後端) 且僅限雲端的虛擬網路
* 兩個儲存體帳戶
* 四個可用性集合，每一層財務分析引擎都有一組
* 適用於四個層級的虛擬機器
* 適用於以 HTTPS 為基礎之 Web 流量 (從網際網路到 Web 伺服器) 的外部負載平衡組
* 適用於未加密之 Web 流量 (從 Web 伺服器到應用程式伺服器) 的內部負載平衡組
* 單一資源群組

## <a name="additional-resources"></a>其他資源
[Microsoft Azure 訂用帳戶及服務限制、配額與限制](../articles/azure-subscription-service-limits.md#storage-limits)

[虛擬機器的大小](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure 儲存體的延展性與效能目標](../articles/storage/storage-scalability-targets.md)

[資料中心延伸模組參考架構圖表](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84)

[Azure 資源管理員提供的 Azure 運算、網路和儲存提供者](../articles/virtual-machines/virtual-machines-windows-compare-deployment-models.md)

