下表依照閘道 SKU 顯示閘道類型和預估的彙總輸送量。 此資料表適用於資源管理員與傳統部署模型。 閘道 SKU 之間的價格並不相同。 如需詳細資訊，請參閱 [VPN 閘道價格](https://azure.microsoft.com/pricing/details/vpn-gateway)。

請注意下表未顯示 UltraPerformance 閘道 SKU。 如需 UltraPerformance SKU 的相關資訊，請參閱 [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) 文件。

|  | **VPN 閘道輸送量 (1)** | **VPN 閘道最大 IPsec 通道 (2)** | **ExpressRoute 閘道輸送量** | **VPN 閘道與 ExpressRoute 共存** |
| --- | --- | --- | --- | --- |
| **基本 SKU (3)(5)(6)** |100 Mbps |10 |500 Mbps (6) |否 |
| **標準 SKU (4)(5)** |100 Mbps |10 |1000 Mbps |是 |
| **高效能 SKU (4)** |200 Mbps |30 |2000 Mbps |是 |

* (1) VPN 輸送量是根據相同 Azure 區域中 VNet 之間度量的概略估計。 這不是網際網路上跨單位連線的保證輸送量。 這是可能的最大輸送量測量。
* (2) 通道的數目請參閱路由式 VPN。 原則式 VPN 只能支援一個站對站 VPN 通道。
* (3) 基本 SKU 不支援 BGP。
* (4) 此 SKU 不支援原則式 VPN。 只有基本 SKU 提供支援。
* (5) 此 SKU 不支援主動-主動 S2S VPN 閘道連線。 只有高效能 SKU 支援主動-主動。
* (6) 基本 SKU 已被 ExpressRoute 取代。
