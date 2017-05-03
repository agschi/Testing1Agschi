## <a name="scenario"></a>案例
為了更清楚說明如何建立 UDR，此文件會使用下列案例。

![影像說明](./media/virtual-network-create-udr-scenario-include/figure1.png)

在此案例中，您將針對「前端子網路」建立一個 UDR，並針對「後端子網路」建立另一個 UDR ，如下所述： 

* **UDR-FrontEnd**。 前端 UDR 將套用到「前端」  子網路且包含一個路由：    
  * **RouteToBackend**。 這個路由會將流向後端子網路的所有流量傳送到 **FW1** 虛擬機器。
* **UDR-BackEnd**。 後端 UDR 將套用到 *後端* 子網路，且包含一個路由：    
  * **RouteToFrontend**。 這個路由會將流向前端子網路的所有流量傳送到 **FW1** 虛擬機器。

這些路由的組合將可確保目的地是從某一個子網路流向另一個子網路的所有流量將會路由至 **FW1** 虛擬機器，此虛擬機器是用來做為虛擬應用裝置。 您也需要針對該 VM 開啟 IP 轉送，以確保它可以接收目的地為其他 VM 的流量。

