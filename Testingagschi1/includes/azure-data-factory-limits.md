資料處理站是一種多租用戶服務，並具有以下的預設限制以確保客戶訂用帳戶不會受到彼此工作負載的影響。 您只要連絡支援人員，即可將您訂用帳戶的大部分限制調整至其最大限制。

| **Resource** | **預設限制** | **上限** |
| --- | --- | --- |
| Azure 訂用帳戶中的 Data Factory |50 |[請連絡支援人員](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 資料處理站中的管線 |2500 |[請連絡支援人員](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 資料處理站中的資料集 |5000 |[請連絡支援人員](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每個資料集的並行配量 |10 |10 |
| 管線物件的每個物件位元組大小<sup>1</sup> |200 KB |200 KB |
| 資料集和已連結服務的每個物件位元組大小<sup>1</sup> |100 KB |2000 KB |
| 訂用帳戶中的 HDInsight 隨選叢集核心<sup>2</sup> |60 |[請連絡支援人員](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 雲端資料移動單位 <sup>3</sup> |32 |[請連絡支援人員](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 管線活動執行的重試計數 |1000 |MaxInt (32 位元) |

<sup>1</sup>管線、 資料集和連結的服務物件代表您工作負載的邏輯群組。 這些物件的限制與您可使用 Azure Data Factory 服務移動或處理的資料量無關。 資料處理站可視需要調整以處理數 PB 的資料。

<sup>2</sup> 隨選 HDInsight 核心並非配置在包含資料處理站的訂用帳戶之中。 因此，上述限制為 Data Factory 針對隨選 HDInsight 核心所強制的核心限制，不同於您 Azure 訂用帳戶相關聯的核心限制。

<sup>3</sup> 雲端資料移動單位 (DMU) 正用於雲端到雲端複製作業。 它是一項量值，代表 Data Factory 中單一單位的能力 (CPU、記憶體和網路資源配置的組合)。 在某些情況下，利用更多 DMU 可以達到更高的複製輸送量。 如需詳細資料，請參閱[雲端資料移動單位](../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units)小節。

| **Resource** | **預設下限** | **下限** |
| --- | --- | --- |
| 排程間隔 |15 Minuten |15 Minuten |
| 重試嘗試間的間隔 |1 秒 |1 秒 |
| 重試逾時值 |1 秒 |1 秒 |

### <a name="web-service-call-limits"></a>Web 服務呼叫限制
Azure Resource Manager 有 API 呼叫限制。 您可使用 [Azure 資源管理員 API 限制](../articles/azure-subscription-service-limits.md#resource-group-limits)內的速率進行 API 呼叫。
