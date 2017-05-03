## <a name="understand-planned-vs-unplanned-maintenance"></a>了解規劃和未規劃維護
有兩種可影響虛擬機器可用性的 Microsoft Azure 平台事件：規劃維護和非規劃維護。

* **規劃的維護事件** 是由 Microsoft 對基礎 Azure 平台進行的定期更新，為虛擬機器在其中執行的平台基礎結構改善整體可靠性、效能和安全性。 這些更新大多數都會在不影響虛擬機器或雲端服務的情況下執行。 但有時候，這些更新還是需要重新啟動虛擬機器，才能將必要的更新套用至平台基礎結構。
* **未規劃的維護事件** 會在虛擬機器中的硬體或實體基礎結構產生某些方面的錯誤時發生。 這可能包含本機網路錯誤、本機磁碟錯誤，或其他機架層級的錯誤。 Azure 平台會在偵測到此類錯誤時，自動從裝載虛擬機器且狀況不良的實體機器，將虛擬機器移轉至狀況良好的實體機器。 這類事件非常稀少，但可能會導致虛擬機器重新啟動。

為了減少一或多個這些事件造成的停機所帶來的影響，建議您為虛擬機器使用下列高可用性的最佳做法：

* [針對備援在可用性設定組中設定多部虛擬機器]
* [將每個應用程式層設定至不同的可用性設定組中]
* [將負載平衡器與可用性設定組結合]
* [將受控磁碟使用於可用性設定組中的 VM]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>針對備援在可用性設定組中設定多部虛擬機器
若要為應用程式提供備援，建議您在可用性設定組中，將兩部以上的虛擬機器組成群組。 這項組態可以確保在規劃或未規劃的維護事件發生期間，至少有一部虛擬機器可以使用，且符合 99.95% 的 Azure SLA。 如需相關資訊，請參閱 [虛擬機器的 SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)。

> [!IMPORTANT]
> 避免一個可用性設定組中只有一部執行個體虛擬機器。 此組態中的 VM 不符合 SLA 的保證，而且會在 Azure 規劃的維護事件期間發生停機狀況，除非單一 VM 是使用 [Azure 進階儲存體](../articles/storage/storage-premium-storage.md)。 使用進階儲存體的單一 VM 適用 Azure SLA。

基礎 Azure 平台會為可用性集合中的每部虛擬機器指派一個**更新網域**和一個**容錯網域**。 在指定的可用性設定組中，預設指派五個非使用者可設定的更新網域 (接著可以增加 Resource Manager 部署，以提供最多 20 個更新網域)，表示虛擬機器群組和可同時重新啟動的基礎實體硬體。 當一個可用性設定組中設定了超過五部虛擬機器，會將第六部虛擬機器放入與第一部虛擬機器相同的更新網域中，而第七部則會放入與第二部相同的更新網域中，以此類推。 重新啟動的更新網域順序可能不會在規劃的維護事件期間循序進行，而只會一次重新啟動一個更新網域。

容錯網域定義共用通用電源和網路交換器的虛擬機器群組。 根據預設，可用性設定組中設定的虛擬機器會分置於最多三個容錯網域中，以進行 Resource Manager 部署 (如果是傳統，則是兩個容錯網域)。 將虛擬機器放入可用性設定組，並無法保護應用程式不會遭受作業系統錯誤或特定應用程式錯誤，而只會限制可能的實體硬體錯誤、網路中斷或電源中斷所帶來的影響。

<!--Image reference-->
   ![更新網域和容錯網域組態的概念圖](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>受控磁碟的容錯網域和可用性設定組
若 VM 使用 [Azure 受控磁碟](../articles/storage/storage-faq-for-disks.md)，VM 會在使用受管理的可用性設定組時配合使用受控磁碟容錯網域。 此一配合行為可確保連接到 VM 的所有受控磁碟都位於相同的受控磁碟容錯網域。 在受管理的可用性設定組中只能建立使用受控磁碟的 VM。 受控磁碟容錯網域數目會依區域而異，每個區域會有兩個或三個受控磁碟容錯網域。


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>將每個應用程式層設定至不同的可用性設定組中
若虛擬機器幾乎都一樣，且對應用程式而言具有相同用途，建議您針對每個應用程式層設定可用性設定組。  若您在相同的可用性設定組中放入兩個不同的階層，則可以一次重新啟動位於相同應用程式層的所有虛擬機器。 您可以藉由在可用性設定組中為每個層設定至少兩部虛擬機器，確保每個層中至少有一部虛擬機器可供使用。

例如，您可以在一個可用性設定組中，將所有虛擬機器放入執行 IIS、Apache、Nginx 等的應用程式前端中。 請確認相同的可用性設定組中只有放入前端的虛擬機器。 同樣地，也要確認資料層的虛擬機器僅會放在它們自己的可用性設定組中，如同複寫的 SQL Server 虛擬機器或 MySQL 虛擬機器。

<!--Image reference-->
   ![應用程式層](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>將負載平衡器與可用性設定組結合
將 [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) 與可用性設定組結合，以獲得最多的應用程式備援能力。 Azure 負載平衡器會在多部虛擬機器之間分配流量。 我們的標準層虛擬機器中包含 Azure 負載平衡器。 並非所有的虛擬機器階層都包含 Azure Load Balancer。 如需關於負載平衡虛擬機器的詳細資訊，請參閱 [負載平衡虛擬機器](../articles/virtual-machines/virtual-machines-linux-load-balance.md)。

若負載平衡器沒有設定為平衡多部虛擬機器之間的流量，則所有計劃性維護事件都只會影響處理流量的虛擬機器，並導致應用程式層中斷。 將同一個層的多部虛擬機器放在相同的負載平衡器和可用性設定組下，可讓至少一個執行個體持續處理流量。

## <a name="use-managed-disks-for-vms-in-availability-set"></a>將受控磁碟使用於可用性設定組中的 VM
如果您目前使用 VM 搭配非受控磁碟，強烈建議您[將可用性設定組中的 VM 轉換為受控磁碟](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set)。

[受控磁碟](../articles/storage/storage-managed-disks-overview.md)可確保可用性設定組中的 VM 磁碟彼此充分隔離，以避免單一失敗點，為可用性設定組提供更高的可靠性。 它的作法是自動以不同的儲存體縮放單位 (戳記) 來放置磁碟。 如果因為硬體或軟體失敗造成戳記失敗，則只有磁碟在這些戳記上的 VM 執行個體才會失敗。 

如果您打算使用 VM 搭配[非受控磁碟](../articles/storage/storage-about-disks-and-vhds-windows.md#types-of-disks)，請針對 VM 的虛擬硬碟 (VHD) 在其中儲存為[分頁 Blob](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) 的儲存體帳戶，遵循以下的最佳做法。 

1. **將與 VM 相關聯的所有磁碟 (OS 和資料) 保留於相同的儲存體帳戶中**
2. 將更多VHD 新增至儲存體帳戶之前，**請檢閱儲存體帳戶中非受控磁碟數目的[限制](../articles/storage/storage-scalability-targets.md)**
3. **針對可用性設定組中的每個 VM 使用個別的儲存體帳戶。** 請勿與相同可用性設定組中的多個 VM 共用儲存體帳戶。 如果遵循上述最佳做法，即可接受位於不同可用性設定組的 VM 共用儲存體帳戶。

<!-- Link references -->
[針對備援在可用性設定組中設定多部虛擬機器]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[將每個應用程式層設定至不同的可用性設定組中]: #configure-each-application-tier-into-separate-availability-sets
[將負載平衡器與可用性設定組結合]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[將受控磁碟使用於可用性設定組中的 VM]: #use-managed-disks-for-vms-in-availability-set

