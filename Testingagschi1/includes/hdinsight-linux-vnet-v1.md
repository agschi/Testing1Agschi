> 您不能在 Linux 的 HDInsight 使用 v1 (傳統) Azure 虛擬網路。 虛擬網路必須是 v2 (Azure 資源管理員)，才能在 Azure Preview 入口網站中的 HDInsight 叢集建立程序期間列出來做為選項，或者在以 Azure CLI 或 Azure PowerShell 建立叢集時使用。
> 
> 如果您擁有 v1 網路上的資源，而您想要讓 HDInsight 透過虛擬網路直接存取這些資源，請參閱[將傳統 VNet 連線到新的 VNet](../articles/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) ，以取得如何將 v2 虛擬網路連線到 v1 虛擬網路的相關資訊。 一旦建立此連線之後，您便可以在 v2 虛擬網路中建立 HDInsight 叢集。
> 
> 



<!--HONumber=Jan17_HO3-->


