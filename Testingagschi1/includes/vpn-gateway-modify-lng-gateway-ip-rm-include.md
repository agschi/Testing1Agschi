若要修改閘道 IP 位址，請使用 'New-AzureRmVirtualNetworkGatewayConnection' Cmdlet。 'Set' Cmdlet 目前不支援修改閘道 IP 位址。

### <a name="gwipnoconnection"></a>修改閘道 IP 位址 - 沒有閘道連線
若要修改尚未擁有連線之區域網路閘道的閘道 IP 位址，請使用以下範例。 您也可以同時修改位址首碼。 請務必使用區域網路閘道的現有名稱來覆寫目前的設定。 否則，您會建立新的區域網路閘道，而不是覆寫現有閘道。

使用下列範例，並將值替換為您自己的值：

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>修改閘道 IP 位址 - 現有閘道連線
如果閘道連線已存在，您需要先移除連線。 連線移除之後，您可以修改閘道 IP 位址，然後重新建立新連線。 您也可以同時修改位址首碼。 這會導致您 VPN 連線的停機時間。

> [!IMPORTANT]
> 請勿刪除 VPN 閘道。 如果您這樣做，您必須重新執行各個步驟來重新建立閘道。 此外，您必須以新的 VPN 閘道 IP 位址來更新內部部署 VPN 裝置。
> 
> 

1. 移除連線。 您可以使用 'Get-AzureRmVirtualNetworkGatewayConnection' Cmdlet 來找到連線的名稱。

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. 修改 'GatewayIpAddress' 值。 您也可以同時修改位址首碼。 請務必使用區域網路閘道的現有名稱來覆寫目前的設定。 否則，您會建立新的區域網路閘道，而不是覆寫現有閘道。

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. 建立連線。 在此範例中，我們會設定 IPsec 連線類型。 當您重新建立連線時，請使用針對設定指定的連線類型。 對於其他連線類型，請參閱 [PowerShell Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) 頁面。  若要取得 VirtualNetworkGateway 名稱，您可以執行 'Get-AzureRmVirtualNetworkGateway' Cmdlet。
   
    設定變數。

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    建立連線。

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```