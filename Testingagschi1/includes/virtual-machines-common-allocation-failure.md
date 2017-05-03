
若本文中未提及您的 Azure 問題，請前往 [MSDN 及 Stack Overflow 上的 Azure 論壇](https://azure.microsoft.com/support/forums/)。 您可以在這些論壇上張貼您的問題，或將問題貼到 Twitter 上的 @AzureSupport。 此外，您也可以選取 [Azure 支援](https://azure.microsoft.com/support/options/)網站上的 [取得支援]，來提出 Azure 支援要求。

## <a name="general-troubleshooting-steps"></a>一般疑難排解步驟
### <a name="troubleshoot-common-allocation-failures-in-the-classic-deployment-model"></a>疑難排解傳統部署模型中常見的配置失敗
這些步驟可協助解決虛擬機器中的許多配置失敗：

* 將 VM 調整為不同的大小。<br>
   按一下 [全部瀏覽]**** >  [虛擬機器 (傳統)]**** > 您的虛擬機器 > [設定]**** >  [大小]****。 如需詳細步驟，請參閱 [調整虛擬機器的大小](https://msdn.microsoft.com/library/dn168976.aspx)。
* 從雲端服務刪除所有 VM，然後重新建立 VM。<br>
   按一下 [全部瀏覽]**** >  [虛擬機器 (傳統)]**** > 您的虛擬機器 > [刪除]****。 然後，按一下 [新增]  >  [計算] > [虛擬機器映像]。

### <a name="troubleshoot-common-allocation-failures-in-the-azure-resource-manager-deployment-model"></a>疑難排解 Azure 資源管理員部署模型中常見的配置失敗
這些步驟可協助解決虛擬機器中的許多配置失敗：

* 停止 (解除配置) 相同可用性設定組中的所有 VM，然後重新啟動每一部 VM。<br>
   若要停止，請按一下 [資源群組]**** > 您的資源群組 > [資源]**** > 您的可用性設定組 > [虛擬機器]**** > 您的虛擬機器 > [停止]****。
  
    所有 VM 都停止之後，請選取第一個 VM，然後按一下 [啟動] 。

## <a name="background-information"></a>背景資訊
### <a name="how-allocation-works"></a>配置的運作方式
Azure 資料中心的伺服器分割成叢集。 通常會嘗試向多個叢集提出配置要求，但配置要求可能帶有某些條件約束，而強制 Azure 平台只嘗試向一個叢集提出要求。 在本文中，這種情況稱為「釘選到叢集」。 下圖 1 說明於嘗試向多個叢集提出一般配置的情況。 圖 2 說明釘選到叢集 2 的配置案例，因為叢集 2 是現有雲端服務 CS_1 或可用性設定組的裝載位置。
![配置圖表](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>配置失敗的原因
當配置要求已釘選到叢集時，由於可用的資源集區較小，很可能找不到可用的資源。 此外，如果配置要求已釘選到叢集，但該叢集不支援您所要求的資源類型，即使叢集有可用的資源，您的要求仍會失敗。 下圖 3 說明由於唯一候選叢集沒有可用的資源，導致已釘選的配置失敗的情況。 圖 4 說明因唯一候選叢集不支援所要求的 VM 大小 (雖然叢集有可用的資源)，而導致已釘選的配置失敗的情況。

![釘選配置失敗](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-the-classic-deployment-model"></a>傳統部署模型中的詳細疑難排解步驟特定配置失敗案例
以下是造成釘選配置要求的常見配置案例。 我們將在本文稍後深入探討每一個案例。

* 調整 VM 大小，或將 VM 或角色執行個體加入至現有的雲端服務
* 重新啟動已部分停止 (已取消配置) 的 VM
* 重新啟動已完全停止 (已取消配置) 的 VM
* 預備/生產部署 (僅適用於平台即服務)
* 同質群組 (VM/服務鄰近性)
* 同質群組式虛擬網路

發生配置錯誤時，請查看以下是否有任何案例符合您所遇到的錯誤。 使用 Azure 平台傳回的配置錯誤來識別對應的案例。 如果您的要求已釘選，請移除一些釘選條件約束，向更多叢集開放您的要求，以提高配置成功的機會。

一般而言，只要錯誤不是表示「不支援所要求的 VM 大小」，您永遠都可以稍後再重試，因為到時叢集可能釋放足夠的資源來滿足您的要求。 如果問題在於不支援所要求的 VM 大小，請嘗試不同的 VM 大小。 否則，唯一的選項便是將釘選條件約束移除。

有兩個常見的失敗案例與同質群組有關。 在過去，同質群組是用來提供 VM/服務執行個體的鄰近性，或用來啟用虛擬網路的建立。 在引進區域虛擬網路之後，建立虛擬網路已不再需要同質群組。 由於 Azure 基礎結構中的網路延遲縮短，原本建議使用同質群組來支援 VM/服務鄰近性的情況已有所改變。

圖 5 顯示 (釘選的) 配置案例的分類。
![釘選配置分類](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> 每個配置案例中列出的錯誤是簡短格式。 請參閱[錯誤字串查詢](#Error string lookup)來取得詳細的錯誤字串。
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>配置案例：調整 VM 大小，或將 VM 或角色執行個體加入至現有的雲端服務
**Error**

Upgrade_VMSizeNotSupported 或 GeneralError

**叢集釘選的原因**

必須在託管現有雲端服務的原始叢集上，嘗試要求調整 VM 大小，或將 VM 或角色執行個體加入現有的雲端服務。 建立新的雲端服務可讓 Azure 平台尋找另一個擁有可用資源，或支援您所要求之 VM 大小的叢集。

**因應措施**

如果錯誤是 Upgrade_VMSizeNotSupported*，請嘗試不同的 VM 大小。 如果使用不同的 VM 大小不可行，但可接受使用不同的虛擬 IP 位址 (VIP)，請建立新的雲端服務來裝載新的 VM，並將新的雲端服務加入至執行現有 VM 的區域虛擬網路。 如果現有的雲端服務不使用區域虛擬網路，您仍然可以為新的雲端服務建立新的虛擬網路，然後將 [現有的虛擬網路連接到新的虛擬網路](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/)。 深入了解 [區域虛擬網路](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)。

如果錯誤是 GeneralError*，很可能是叢集支援資源的類型 (例如特定的 VM 大小)，但叢集目前沒有可用的資源。 類似上述案例，透過建立新的雲端服務 (請注意，新的雲端服務必須使用不同的 VIP) 新增所需的計算資源，並使用區域虛擬網路連接您的雲端服務。

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>配置案例：重新啟動已部分停止 (已取消配置) 的 VM
**錯誤**

GeneralError*

**叢集釘選的原因**

部分解除配置表示您已停止 (已取消配置) 雲端服務中的一或多部 VM，而不是所有 VM。 停止 (取消配置) VM 時會釋放相關聯的資源。 因此，重新啟動已停止 (已取消配置) 的 VM 是一項新的配置要求。 在部分已取消配置的雲端服務中重新啟動 VM，相當於將 VM 加入現有的雲端服務。 必須在裝載現有雲端服務的原始叢集上嘗試提出配置要求。 另外建立一個雲端服務可讓 Azure 平台尋找另一個有可用資源，或支援您所要求之 VM 大小的叢集。

**因應措施**

如果可接受使用不同的 VIP，請刪除已停止 (已取消配置) 的 VM (但保留相關聯的磁碟)，並透過不同的雲端服務重新加回 VM。 使用區域虛擬網路連接您的雲端服務：

* 如果現有的雲端服務使用區域虛擬網路，只要將新的雲端服務加入至相同的虛擬網路即可。
* 如果現有的雲端服務不使用區域虛擬網路，請為新的雲端服務建立新的虛擬網路，然後將 [現有的虛擬網路連接到新的虛擬網路](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/)。 深入了解 [區域虛擬網路](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)。

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>配置案例：重新啟動已完全停止 (已取消配置) 的 VM
**錯誤**

GeneralError*

**叢集釘選的原因**

完整取消配置表示您已停止 (已取消配置) 雲端服務上的所有 VM。 必須在裝載雲端服務的原始叢集上嘗試提出配置要求以重新啟動這些 VM。 建立新的雲端服務可讓 Azure 平台尋找另一個擁有可用資源，或支援您所要求之 VM 大小的叢集。

**因應措施**

如果可接受使用不同的 VIP，請刪除原始已停止 (已取消配置) 的 VM (但保留相關聯的磁碟)，並刪除對應的雲端服務 (已停止 (已取消配置) VM 時，就已釋放相關聯的計算資源)。 建立新的雲端服務以加回 VM。

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>配置案例：預備/生產環境部署 (僅適用於平台即服務)
**Error**

New_General *或 New_VMSizeNotSupported*

**叢集釘選的原因**

雲端服務的預備環境部署和生產環境部署裝載於相同的叢集。 新增第二個部署時，將會在裝載第一個部署的相同叢集中嘗試提出對應的配置要求。

**因應措施**

請刪除第一個部署和原始的雲端服務，然後重新部署雲端服務。 這個動作可能將第一個部署安排到有足夠可用資源可滿足這兩個部署的叢集，或安排到支援所要求 VM 大小的叢集。

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>配置案例：同質群組 (VM/服務鄰近性)
**Error**

New_General *或 New_VMSizeNotSupported*

**叢集釘選的原因**

任何指派給同質群組的計算資源都繫結至一個叢集。 該同質群組中新的計算資源要求，將於裝載現有資源的相同叢集中嘗試提出。 無論新的資源是透過新的雲端服務，或是透過現有的雲端服務來建立，都是如此。

**因應措施**

如果同質群組不是必要的，請勿使用同質群組或將計算資源分組為多個同質群組。

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>配置案例：同質群組式虛擬網路
**Error**

New_General *或 New_VMSizeNotSupported*

**叢集釘選的原因**

在導入區域虛擬網路之前，您必須先將虛擬網路與同質群組產生關聯。 如此一來，將會依上一節「配置案例：同質群組 (VM/服務鄰近性)」所述的相同條件約束，繫結放入同質群組中的計算資源。 計算資源會繫結至單一叢集。

**因應措施**

如果您不需要同質群組，請為您要加入的新資源建立新的區域虛擬網路，然後 [將現有的虛擬網路連接到新的虛擬網路](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/)。 深入了解 [區域虛擬網路](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)。

此外，您也可以 [將同質群組式虛擬網路移轉至區域虛擬網路](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)，然後再重新加入需要的資源。

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager 部署模型中的詳細疑難排解步驟特定配置失敗案例
以下是造成釘選配置要求的常見配置案例。 我們將在本文稍後深入探討每一個案例。

* 調整 VM 大小，或將 VM 或角色執行個體加入至現有的雲端服務
* 重新啟動已部分停止 (已取消配置) 的 VM
* 重新啟動已完全停止 (已取消配置) 的 VM

發生配置錯誤時，請查看以下是否有任何案例符合您所遇到的錯誤。 使用 Azure 平台傳回的配置錯誤來識別對應的案例。 如果您的要求已釘選到現有的叢集，請移除一些釘選條件約束，向更多叢集開放您的要求，以提高配置成功的機會。

一般而言，只要錯誤不是表示「不支援所要求的 VM 大小」，您永遠都可以稍後再重試，因為到時叢集可能釋放足夠的資源來滿足您的要求。 如果問題在於不支援所要求的 VM 大小，請參閱以下的因應措施。

## <a name="allocation-scenario-resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>配置案例：調整 VM 大小，或將 VM 加入至現有的可用性設定組
**Error**

Upgrade_VMSizeNotSupported *或 GeneralError*

**叢集釘選的原因**

必須在裝載現有可用性設定組的原始叢集上，嘗試要求調整 VM 大小，或將 VM 加入至現有的可用性設定組。 建立新的可用性設定組可讓 Azure 平台尋找另一個有可用資源，或支援您所要求的 VM 大小的叢集。

**因應措施**

如果錯誤是 Upgrade_VMSizeNotSupported*，請嘗試不同的 VM 大小。 如果使用不同的 VM 大小不可行，請停止可用性設定組中的所有 VM。 您可以變更虛擬機器的大小，以便將 VM 配置到支援所需 VM 大小的叢集。

如果錯誤是 GeneralError*，很可能是叢集支援資源的類型 (例如特定的 VM 大小)，但叢集目前沒有可用的資源。 如果 VM 可以屬於不同的可用性設定組，請在不同的可用性設定組 (位於相同區域) 中建立新的 VM。 然後，這個新的 VM 就可以加入至相同的虛擬網路。  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>配置案例：重新啟動已部分停止 (已取消配置) 的 VM
**錯誤**

GeneralError*

**叢集釘選的原因**

部分解除配置表示您已停止 (已取消配置) 可用性設定組中的一或多部 VM，而不是所有 VM。 停止 (取消配置) VM 時會釋放相關聯的資源。 因此，重新啟動已停止 (已取消配置) 的 VM 是一項新的配置要求。 在部分已取消配置的可用性設定組中重新啟動 VM，相當於將 VM 加入現有的可用性設定組。 必須在裝載現有可用性設定組的原始叢集上嘗試提出配置要求。

**因應措施**

停止可用性設定組中的所有 VM，再重新啟動第一個 VM。 如此可確保執行新的配置嘗試，而且可以選取有容量可用的新叢集。

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>配置案例：重新啟動已完全停止 (已取消配置)
**錯誤**

GeneralError*

**叢集釘選的原因**

完整取消配置表示您已停止 (已取消配置) 可用性設定組中的所有 VM。 提出配置要求來重新啟動這些 VM 時，將會以支援所需大小的所有叢集為目標。

**因應措施**

選取新的 VM 大小以進行配置。 如果仍無法解決問題，請稍後再試。

## <a name="error-string-lookup"></a>錯誤字串查詢
**New_VMSizeNotSupported***

「由於部署要求條件約束，無法佈建此部署所需的 VM 大小 (或 VM 大小的總和)。 可能的話，請嘗試放寬約束條件 (例如虛擬網路繫結)、部署至不具有其他部署的託管服務及不同的同質群組 (或不具有同質群組的託管服務)，或嘗試部署至不同的區域。」

**New_General***

「配置失敗；無法滿足要求中的條件約束。 要求的新服務部署繫結至同質群組，或以虛擬網路為目標，或此託管服務下已經有部署。 任何這些情況會將新的部署侷限於特定的 Azure 資源。 請稍後重試，或嘗試減少 VM 大小或角色執行個體的數量。 或者，可能的話，移除先前提到的條件約束，或嘗試部署至不同的區域。」

**Upgrade_VMSizeNotSupported***

「無法升級部署。 在支援現有部署的資源中，可能沒有所要求的 VM 大小 XXX。 請稍後再試、嘗試使用不同的 VM 大小或較少的角色執行個體，或在空的託管服務下以建立新的同質群組或沒有同質群組繫結來建立部署。」

**GeneralError***

「伺服器發生內部錯誤。 請重試要求。」 或「無法產生服務的配置。」

