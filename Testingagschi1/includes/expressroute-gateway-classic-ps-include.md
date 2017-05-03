您必須先建立 VNET 和閘道器子網路，才能進行下列工作。 如需詳細資訊，請參閱 [使用傳統入口網站設定虛擬網路](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) 一文。   

## <a name="add-a-gateway"></a>新增閘道
使用以下的命令建立閘道器。 所有的值請務必替換成您自己的值。

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>確認已建立閘道
使用下列命令，確認已建立閘道。 這個命令也會擷取閘道器識別碼，您在其他作業會需要它。

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>調整閘道器大小
有幾個 [閘道 SKU](../articles/expressroute/expressroute-about-virtual-network-gateways.md)。 您可以使用下列命令隨時變更閘道器 SKU。

> [!IMPORTANT]
> 此命令不適用於 UltraPerformance 閘道。 若要將您的閘道變更為 UltraPerformance 閘道，請先移除現有的 ExpressRoute 閘道，然後建立新的 UltraPerformance 閘道。 若要從 UltraPerformance 閘道降級您的閘道，請先移除 UltraPerformance 閘道，然後建立新的閘道。 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>移除閘道器
使用以下的命令移除閘道器

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>