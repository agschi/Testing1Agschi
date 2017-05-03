###<a name="does-azure-charge-for-traffic-between-vnets"></a>Azure 會對 VNet 之間的流量收費嗎？
相同區域內的 VNet 對 VNet 流量雙向皆為免費。 跨區域 VNet 對 VNet 輸出流量會根據來源區域的輸出 VNet 間資料傳輸費率收費。 如需詳細資訊，請參閱[價格頁面](https://azure.microsoft.com/pricing/details/vpn-gateway/)。

###<a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>VNet 對 VNet 流量是否會透過網際網路傳輸？
否。 VNet 對 VNet 流量會透過 Microsoft Azure 骨幹傳輸，而非透過網際網路。

### <a name="is-vnet-to-vnet-traffic-secure"></a>VNet 對 VNet 流量是否安全？
是，它是由 IPsec/IKE 加密保護。

###<a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>我需要將 VNet 連接在一起的 VPN 裝置嗎？
否。 將多個 Azure 虛擬網路連接在一起並不需要 VPN 裝置，除非需要跨單位連線能力。

###<a name="do-my-vnets-need-to-be-in-the-same-region"></a>我的 VNet 需要位於相同區域嗎？
否。 虛擬網路可位於相同或不同的 Azure 區域 (位置)。

###<a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>可以使用 VNet 對 VNet 以及多站台連線嗎？
是。 虛擬網路連線能力可以與多站台 VPN 同時使用。

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>一個虛擬網路可以連接多少內部部署網站和虛擬網路？
請參閱[閘道需求](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements)表格。

###<a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>是否可以使用 VNet 對 VNet 連線來連接 VNet 外部的 VM 或雲端服務？
否。 VNet 對 VNet 支援連接虛擬網路。 但是不支援連接不在虛擬網路中的虛擬機器或雲端服務。

###<a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>雲端服務或負載平衡端點是否可以跨越 VNet？
否。 即使虛擬網路連接在一起，雲端服務或負載平衡端點也無法跨虛擬網路。

###<a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>是否可以使用 PolicyBased VPN 類型進行 VNet 對 VNet 或多站台連線？
否。 VNet 對 VNet 和多站台連線需要 VPN 類型為 RouteBased (前稱為動態路由) 的 Azure VPN 閘道。

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>是否可以將 RouteBased VPN 類型的 VNet 連線到另一個 PolicyBased VPN 類型的 VNet？
否，這兩個虛擬網路必須使用路由式 (先前稱為動態路由) VPN。

###<a name="do-vpn-tunnels-share-bandwidth"></a>VPN 通道是否共用頻寬？
是。 虛擬網路的所有 VPN 通道一起共用 Azure VPN 閘道上可用的頻寬，以及 Azure 中相同的 VPN 閘道運作時間 SLA。

###<a name="are-redundant-tunnels-supported"></a>是否支援備援通道？
當一個虛擬網路閘道設定為主動-主動時，支援成對虛擬網路之間的備援通道。

###<a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>VNet 對 VNet 組態的位址空間是否可以重疊？
否。 您的 IP 位址範圍不能重疊。

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>在連接的虛擬網路和內部部署本機網站之間是否可以有重疊的位址空間？
否。 您的 IP 位址範圍不能重疊。





<!--HONumber=Feb17_HO3-->


