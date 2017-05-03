Azure 雲端解決方案建置於虛擬機器 (實體電腦硬體的模擬) 上，而虛擬機器可靈活封裝軟體部署，並且比實體硬體更能強化資源的彙總。 [Docker](https://www.docker.com) 容器和 Docker 生態系統大幅擴充了您可開發、運送和管理分散式軟體的方式。 容器中的應用程式程式碼會與主機 VM 和相同 VM 上的其他容器隔離。 這種隔離方式可讓您擁有更多的開發和部署靈活度。

Azure 提供下列 Docker 值︰

* 建立 Docker 主機有[許多](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)[不同的](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)方式，讓容器能符合您的情況
* [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) 能使用 **marathon** 和 **swarm** 等 Orchestrator 建立容器主機叢集。
* 有 [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md)和[資源群組範本](../articles/resource-group-authoring-templates.md)可簡化部署和更新複雜的分散式應用程式
* 能與大量專屬和開放原始碼組態管理工具進行整合

此外，因為您可以透過程式設計方式在 Azure 上建立 VM 和 Linux 容器，所以您也可以使用 VM 和容器「協調流程」  工具來建立虛擬機器 (VM) 的群組，以及在 Linux 容器和 (現已可使用的) [Windows 容器](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)兩者之內部署應用程式。

本文不只會討論這些高層級的概念，它也包含大量連結供您了解在 Azure 上與容器和叢集使用相關的詳細資訊、教學課程以及產品。 如果您已經了解上述內容，只是需要連結，連結就在[適用於容器的工具](#tools-for-working-with-azure-vms-and-containers)。

## <a name="the-difference-between-virtual-machines-and-containers"></a>虛擬機器與容器之間的差異
虛擬機器是在 [Hypervisor](http://en.wikipedia.org/wiki/Hypervisor)提供的獨立硬體虛擬化環境內執行。 在 Azure 中，[虛擬機器](https://azure.microsoft.com/services/virtual-machines/)服務會為您處理所有作業：您可以藉由選擇作業系統並設定 &mdash;，或藉由上傳自訂 VM 映像，來建立虛擬機器。 虛擬機器是經過長時間測試、歷經考驗而根深蒂固的技術，而且有許多工具可用來管理其中所含的作業系統和應用程式。  主機作業系統中會隱藏 VM 內的應用程式。 從 VM 上的應用程式或使用者觀點來看，VM 就像是自發的實體電腦。

[Linux 容器](http://en.wikipedia.org/wiki/LXC)和使用 docker 工具建立及裝載的容器，不會使用 Hypervisor 來加以隔離。 使用容器時，容器主機會使用 Linux 核心的程序和檔案系統隔離功能，來對容器、其應用程式、某些核心功能及其本身的隔離檔案系統公開。 從容器內執行的應用程式觀點來看，容器就像是唯一的作業系統執行個體。 包含在其中的應用程式看不見處理程序或其容器以外的任何其他資源。

Docker 容器中使用的資源遠少於 VM 中使用的資源。 Docker 容器採用不會與 Docker 主機核心共用的應用程式隔離和執行模型。 容器的磁碟使用量更低，因為它不包含整個作業系統。 啟動時間和所需磁碟空間大幅低於 VM。
Windows 容器對於在 Windows 執行的應用程式提供與 Linux 容器相同的優點。 Windows 容器支援 Docker 映像格式和 Docker API，但它們也可以使用 PowerShell 管理。 Windows 容器可以使用二種容器執行階段：Windows 伺服器容器和 Hyper-V 容器。 Hyper-V 容器將每個容器裝載在高度最佳化的 VM，以提供額外的隔離。 若要深入了解 Windows 容器，請參閱 [關於 Windows 容器](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)。 若要開始在 Azure 中使用 Windows Containers，請了解如何[部署 Azure Container Service 叢集](/articles/container-service/container-service-deployment.md)。

## <a name="what-are-containers-good-for"></a>容器適用的情況？

容器可以提升︰

* 開發和廣泛共用應用程式程式碼的速度
* 應用程式的測試速度和信心
* 應用程式的部署速度和信心

容器是在容器主機 (&mdash;作業系統) 上執行，而在 Azure 中是在 Azure 虛擬機器中執行。 即使您非常喜歡容器的概念，您還是需要一個裝載容器的 VM 基礎結構，但好處是，容器並不在意是在哪一個 VM 上執行 (雖然容器是否需要 Linux 或 Windows 執行環境很重要)。


## <a name="what-are-containers-good-for"></a>容器適用的情況？
容器適用很多情況，但是它們比較適合 ([Azure 雲端服務](https://azure.microsoft.com/services/cloud-services/)和 [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md) 亦同) 建立單一服務、微服務導向的分散式應用程式，這類應用程式的設計是基於多個較小型、可組合的組件，而非基於較大型、強烈相關聯的元件。

尤其是對於像 Azure 這種您在需要時可以租用 VM 的公用雲端環境，特別適合。 您不只可以獲得隔離、快速的部署以及協調流程工具，還能更有效率決定應用程式基礎結構。

例如，您目前有一個部署，有一個大型、高可用性的分散式應用程式，由 9 個 Azure VM 組成。 如果此應用程式的元件可以部署在容器中，您可能只需要使用 4 個 VM，然後將您的應用程式元件部署在 20 個容器內，做為備援與負載平衡。

這只是一個範例，當然，您或許也可以在您的案例中這樣進行，您可以調整使用更多容器，而不是更多 Azure VM 達到最高的使用率，並且比以前更有效率地使用剩餘的整體 CPU 負載。

此外，還有許多不需要微服務方式的案例；您將會最了解微服務和容器是否對您最有幫助。

### <a name="container-benefits-for-developers"></a>容器對開發人員的優點
一般而言，我們很容易看到容器技術持續進步，但是還有更多明確的優點。 我們來看看 Docker 容器的範例。 本主題目前不會深入討論 Docker (請閱讀 [Docker 是什麼？](https://www.docker.com/whatisdocker/)以了解詳細內容，或是 [Wikipedia](http://wikipedia.org/wiki/Docker_%28software%29))，但是 Docker 及其生態系統能夠對開發人員和 IT 專業人員提供極大的優點。

開發人員很快便會樂意使用 Docker 容器，因為它能讓 Linux 和 Windows 容器的使用體驗變得更加輕鬆：

* 開發人員可以使用簡單、累加的命令來建立容易部署的固定映像，而且可以使用 dockerfile 自動化建置這些映像
* 開發人員可以使用簡單、[Git](https://git-scm.com/) 式的發送和提取命令，輕鬆將這些映像共用到[公用](https://registry.hub.docker.com/)或[私用 Docker 登錄](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* 開發人員可以考慮隔離應用程式元件，而不是電腦
* 開發人員可以使用大量的工具，了解 docker 容器和不同的基本映像

### <a name="container-benefits-for-operations-and-it-professionals"></a>容器對作業人員和 IT 專業人員的優點
結合容器和虛擬機器，也對 IT 和作業專業人員有好處。

* 內含的服務與 VM 主機執行環境隔離
* 內含的程式碼可驗證為完全相同
* 內含的服務可以啟動、停止，以及在開發、測試和生產環境之間快速移動

諸如此類的功能 (還有更多) 刺激既有的企業，其中專業的資訊技術組織有一項工作是調整資源 (包括單純處理電力問題)，不僅配合各種讓企業生存下去所需的工作，還能提升客戶滿意度與影響範圍。 小型企業、獨立軟體廠商和新創公司的需求完全相同，但是它們對於需求可能會有不同的描述。

## <a name="what-are-virtual-machines-good-for"></a>虛擬機器適用的情況？
虛擬機器提供雲端運算的骨幹，這是不變的。 虛擬機器雖然啟動速度慢一點、磁碟使用量較大，而且沒有直接對應到微服務架構，但是它們有非常重要的優點：

1. 虛擬機器預設能為主機電腦提供更穩固的預設安全性保護
2. 虛擬機器支援任何主要的作業系統和應用程式組態
3. 虛擬機器具備存在已久的工具生態系統進行命令與控制
4. 虛擬機器為主機容器提供執行環境

最後一項很重要，因為視應用程式的呼叫而定，內含的應用程式仍然需要特定的作業系統和 CPU 類型。 請務必記住您要在 VM 上安裝容器，因為 VM 包含您想要部署的應用程式，容器無法取代 VM 或作業系統。

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>VM 和容器的高階功能比較
下表說明在 VM 和 Linux 容器之間非常高階、不需要太多額外工作的功能差異。 請注意，根據您自己的應用程式需求，可能需要增加或減少某些功能，如同所有軟體一般，額外進行一些工作可以支援更多功能，尤其是安全性部分。

| 功能 | VM | 容器 |
|:--- | --- | --- |
| 「預設」安全性支援 |到比較高的程度 |到稍微較低的程度 |
| 磁碟上需有記憶體 |整套作業系統及應用程式 |僅限應用程式需求 |
| 啟動花費時間 |相當長的時間：作業系統啟動和應用程式載入 |相當短的時間：因為核心已在執行中，只需要啟動應用程式 |
| 可攜性 |經過適當準備則可攜 |在映像格式內可攜；通常較小 |
| 映像自動化 |根據 OS 和應用程式有很大的差異 |[Docker 登錄](https://registry.hub.docker.com/)；其他 |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>建立和管理 VM 群組和容器群組
到目前為止，任何架構師、開發人員或 IT 作業專家可能會想：「我如果可以自動化所有這些事物，這就真的是「資料中心即服務！」。

沒錯，它的確可以，而且有任意數目的系統，您可能已經使用其中一些系統。您可以使用指令碼 (通常使用 [CustomScriptingExtension for Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) 或 [CustomScriptingExtension for Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)) 管理 Azure VM 的群組並插入自訂程式碼。 您可以 (而且可能已經在) 使用 PowerShell 或 Azure CLI 指令碼自動化您的 Azure 部署。

這些功能通常會移轉到如 [Puppet](https://puppetlabs.com/) 和 [Chef](https://www.chef.io/) 等工具，大量自動化建立和設定 VM。 ( [這裡](#tools-for-working-with-containers)有許多搭配 Azure 使用這些工具的連結)。

### <a name="azure-resource-group-templates"></a>Azure 資源群組範本
最近，Azure 發行了 [Azure 資源管理](../articles/resource-manager-deployment-model.md) REST API，以及更新的 PowerShell 和 Azure CLI 工具可輕鬆使用。 您可以使用 [Azure 資源管理員範本](../articles/resource-group-authoring-templates.md) ，搭配使用下列的 Azure 資源管理 API，來部署、修改或重新部署整個應用程式拓撲：

* [使用範本的 Azure 入口網站](https://github.com/Azure/azure-quickstart-templates)&mdash;提示：使用 [DeployToAzure] 按鈕
* [Azure CLI](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure Powershell 模組](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>部署和管理整個 Azure VM 群組和容器群組
有幾個很受歡迎的系統可以部署整個 VM 群組，並且在系統上面安裝 Docker (或其他 Linux 容器主機系統) 做為可自動化的群組。 如需直接連結，請參閱下面的 [容器和工具](#containers-and-vm-technologies) 一節。 有幾個系統可以做到更大或更小的程度，只是這份清單並不詳盡。 這要視您的技能和案例而定，可能不一定有用。

Docker 有自己的 VM 建立工具 ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) 以及一個負載平衡、docker-container 叢集管理工具 ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json))。 此外，[Azure Docker VM 延伸模組](https://github.com/Azure/azure-docker-extension/blob/master/README.md)預設會支援 [`docker-compose`](https://docs.docker.com/compose/)，它可以跨越多個容器部署設定好的應用程式容器。

此外，您可以試試 [Mesosphere 的資料中心作業系統 (DCOS)](http://docs.mesosphere.com/install/azurecluster/)。 DCOS 是根據開放原始碼 [Mesos](http://mesos.apache.org/) 的「分散式系統核心」，可讓您將您的資料中心視為一個可定址的服務。 DCOS 擁有幾個重要系統的內建套件，例如 [Spark](http://spark.apache.org/) 和 [Kafka](http://kafka.apache.org/) (以及其他)，以及例如 [Marathon](https://mesosphere.github.io/marathon/) (容器控制系統) 和 [Chronos](https://mesos.github.io/chronos/) (分散式排程器) 的內建服務。 Mesos 衍生自在 Twitter、AirBnb 和其他 Web 規模的企業學習到的工作。 您也可以使用 **swarm** 做為協調流程引擎。

而 [Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) 則是一個 VM 和容器群組管理的開放原始碼系統，衍生自在 Google 學習到的工作。 您甚至可以使用 [Kubernetes 搭配 Weave 提供網路支援](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)。

[Deis](http://deis.io/overview/) 是一個開放原始碼的「平台即服務」(PaaS)，可以輕鬆部署和管理您自己伺服器上的應用程式。 Deis 建立在 Docker 和 CoreOS 上，提供輕量級 PaaS 以及以 Heroku 為靈感來源的工作流程。 您可以輕鬆[建立 3 個節點的 Azure VM 群組，並且在 Azure 上安裝 Deis](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，然後[安裝 Hello World Go 應用程式](../articles/virtual-machines/linux/deis-cluster.md#deploy-and-scale-a-hello-world-application)。

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html) 是一個 Linux 散發套件，有最佳化的使用量、Docker 支援以及稱為 [rkt](https://github.com/coreos/rkt) 的自有容器系統，也有一個稱為 [fleet](https://coreos.com/using-coreos/clustering/) 的容器群組管理工具。

Ubuntu 是另一個非常受歡迎的 Linux 散發套件，能完整支援 Docker，也支援 [Linux (LXC 式) 叢集](https://help.ubuntu.com/lts/serverguide/lxc.html)。

## <a name="tools-for-working-with-azure-vms-and-containers"></a>使用 Azure VM 和容器的工具
要使用容器和 Azure VM 必須使用工具。 本節提供的清單，只包含與容器、群組和較大型組態相關、一些最有用或最重要的概念與工具，以及搭配這些工具所使用的協調流程工具。

> [!NOTE]
> 這個領域的變化出乎意料地快速，我們將盡全力讓本主題及其連結保持在最新狀態，不過這可能也是個不可能的任務。 請確定搜尋有興趣的主題，以獲得最新資訊！
>
>

### <a name="containers-and-vm-technologies"></a>容器和 VM 技術
一些 Linux 的容器技術：

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS 和 rkt](https://github.com/coreos/rkt)
* [Open Container Project](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Windows 容器連結：

* [Windows 容器](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Visual Studio Docker 連結：

* [Visual Studio 2015 RC Tools for Docker - Preview](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Docker 工具：

* [Docker daemon](https://docs.docker.com/installation/#installation)
* Docker 用戶端
  * [Windows Docker Client on Chocolatey](https://chocolatey.org/packages/docker)
  * [Docker 安裝指示](https://docs.docker.com/installation/#installation)

Microsoft Azure 上的 Docker：

* [Azure 上 Linux 的 Docker VM 延伸模組](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure Docker VM 延伸模組使用者指南](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [透過 Azure 命令列介面 (Azure CL) 使用 Docker VM 延伸模組](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [使用 Azure 入口網站中的Docker VM 擴充程式](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [如何在 Azure 上使用 docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [如何在 Azure 上搭配 swarm 使用 docker](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [在 Azure 上開始使用 Docker 和 Compose](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [使用 Azure 資源群組範本在 Azure 上快速建立 Docker 主機](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [對內含應用程式的內建 `compose`](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) 支援
* [在 Azure 上實作 Docker 私用登錄](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linux 散發套件和 Azure 範例：

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

組態、叢集管理以及容器協調流程：

* [CoreOS 上的 Fleet](https://coreos.com/using-coreos/clustering/)
* Deis

  * [建立一個 3 個節點的 Azure VM 群組、安裝 Deis，然後啟動 Hello World Go 應用程式](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Kubernetes

  * [使用 CoreOS 和 Weave 自動部署 Kubernetes 叢集的完整指南](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [Mesosphere 的資料中心作業系統 (DCOS)。](http://beta-docs.mesosphere.com/install/azurecluster/)
* [Jenkins](https://jenkins-ci.org/) 和 [Hudson](http://hudson-ci.org/)

  * [部落格：適用於 Azure 的 Jenkins 從屬外掛程式](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
  * [GitHub 儲存機制︰適用於 Azure 的 Jenkins 儲存體外掛程式](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [協力廠商︰適用於 Azure 的 Hudson 從屬外掛程式](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [協力廠商︰適用於 Azure 的 Hudson 儲存體外掛程式](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure 自動化](https://azure.microsoft.com/services/automation/)

  * [影片：如何在 Linux VM 上使用 Azure 自動化](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Powershell DSC for Linux

  * [部落格：如何使用 Powershell DSC for Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub：Docker 用戶端 DSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>後續步驟
認識 [Docker](https://www.docker.com) 和 [Windows Server 容器](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)。

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
