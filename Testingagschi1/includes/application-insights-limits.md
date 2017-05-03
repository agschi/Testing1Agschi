每個應用程式 (亦即每個檢測金鑰) 都有一些度量和事件的數目限制。 限制取決於您選擇的[定價方案](https://azure.microsoft.com/pricing/details/application-insights/)。

| **Resource** | **預設限制** | **注意**
| --- | --- | --- |
| 每日資料總量 | 500 GB | 您可以設定上限來減少資料。 如果您需要更多資料，請寄送郵件至 AIDataCap@microsoft.com。
| 每月免費的資料量<br/> (基本定價方案) | 1 GB | 每 GB 收費的額外資料量。
| 節流 | 32 k 事件數/秒 | 此限制會測量超過一分鐘。
| 資料保留 | 90 天 | 此資源適用於[搜尋](../articles/application-insights/app-insights-diagnostic-search.md)、[分析](../articles/application-insights/app-insights-analytics.md)和[計量瀏覽器](../articles/application-insights/app-insights-metrics-explorer.md)。
| [可用性多步驟測試](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests)詳述的結果保留期 | 90 天 | 此資源會提供每個步驟的詳細結果。
| 事件大小上限 | 64 K | 
| 屬性和度量名稱長度 | 150 | 如需詳細資訊，請參閱下方註解
| 屬性值字串長度 | 8,192 | 如需詳細資訊，請參閱下方註解
| 追蹤和例外狀況訊息長度 | 10 k | 如需詳細資訊，請參閱下方註解
| 每個應用程式的[可用性測試](../articles/application-insights/app-insights-monitor-web-app-availability.md)計數  | 10 |

如需詳細資訊，請參閱[關於 Application Insights 中的價格和配額](../articles/application-insights/app-insights-pricing.md)。

如需資料欄位限制的詳細資訊，請參閱[依照類型結構描述](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
