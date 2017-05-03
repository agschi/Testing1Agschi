# <a name="compute"></a>運算
Azure 可讓您部署及監視在 Microsoft 資料中心內執行的應用程式程式碼。 當您建立應用程式並在 Azure 上執行它時，其程式碼和組態併稱為 Azure 託管服務。 託管服務的管理、向上調整與向下調整、重新設定都很容易，而且可以隨您新版的應用程式程式碼更新。 本文主要著墨於 Azure 託管服務應用程式模型。<a id="compare" name="compare"></a>

## <a name="table-of-contentsa-idgoback-namegobacka"></a>目錄<a id="_GoBack" name="_GoBack"></a>
* [Azure 應用程式模型優點][Azure Application Model Benefits]
* [託管服務核心概念][Hosted Service Core Concepts]
* [託管服務設計考量][Hosted Service Design Considerations]
* [設計可調整的應用程式][Designing your Application for Scale]
* [託管服務的定義和組態][Hosted Service Definition and Configuration]
* [服務定義檔][The Service Definition File]
* [服務組態檔][The Service Configuration File]
* [建立和部署託管服務][Creating and Deploying a Hosted Service]
* [參考][References]

## <a id="benefits"> </a>Azure 應用程式模型優點
當您部署應用程式作為託管服務時，Azure 會在其中一個 Azure 資料中心內的實體機器上，建立一或多個含有您應用程式程式碼的虛擬機器 (VM) 並啟動 VM。 當用戶端存取託管應用程式的要求進到資料中心時，負載平衡器便會將這些要求平均分配給各 VM。 將您的應用程式託管於 Azure 上，有三項主要優點：

* **高可用性。** 高可用性的意思是，Azure 會盡量確保您的應用程式全時運作，不論何時都能回應用戶端要求。 如果您的應用程式終止 (例如，因為遇到無法處理的例外狀況)，則 Azure 會偵測到，並且會自動重新啟動您的應用程式。 如果執行您應用程式的機器發生某種硬體故障，則 Azure 也會偵測到，並且會自動在另一部可用的實體機器上建立新的 VM，然後在該處執行您的應用程式。 注意：為了讓您的應用程式達到 Microsoft 所訂 99.95% 的服務等級協定，至少必須有兩個 VM 執行您的應用程式程式碼。 如此一來，當 Azure 將您的程式碼從故障的 VM 移至新的良好 VM 時，還有另一個 VM 能夠處理用戶端要求。
* **延展性。** Azure 可讓您根據應用程式上的實際負載，輕鬆、動態地變更執行您應用程式程式碼的 VM 數目。 如此一來，您便能根據客戶加諸於應用程式的工作量來調整應用程式，同時只為您所需的 VM 在您所需時段的運作付出費用。 當您想要變更 VM 數目時，Azure 幾分鐘內就會做出回應，方便您隨時動態變更想要執行的 VM 數目。
* **可管理性。** Azure 因為是一種「平台即服務」(PaaS) 供應項目，所以會管理讓這些機器持續執行所需的基礎結構 (硬體本身、電力和網路)。 Azure 也會管理平台，在其上安裝所有正確的修補程式和安全性更新，以及元件更新 (如 .NET Framework 和 Internet Information Server)，確保作業系統具有最新的版本。 因為所有 VM 都是執行 Windows Server 2008，所以 Azure 還提供其他功能，如診斷監視、遠端桌面支援、防火牆和憑證存放區組態。 所有這些功能都是免費提供。 事實上，當您在 Azure 中執行應用程式時，Windows Server 2008 作業系統 (OS) 授權就會內附在裡面。 因為所有 VM 都是執行 Windows Server 2008，所以在 Azure 中執行時，任何在 Windows Server 2008 上執行的程式碼都會運作良好。

## <a id="concepts"> </a>託管服務核心概念
當您在 Azure 中部署應用程式作為託管服務後，該應用程式會以一或多個「角色」的身分執行。 「角色」其實就是指應用程式檔案和組態。 您可以為應用程式定義一或多個角色，而每個角色都有自己專屬的一組應用程式檔案和組態。 您可以針對應用程式中的每個角色，指定要執行的 VM (或「角色執行個體」) 數目。 下圖顯示以角色和角色執行個體將應用程式模型化為託管服務時的兩個簡單範例。

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>圖 1：單一角色有三個執行個體 (VM) 在 Azure 資料中心內執行
![image][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>圖 2：兩個角色各有兩個執行個體 (VM) 在 Azure 資料中心內執行
![image][1]

角色執行個體通常會透過所謂的「輸入端點」來處理進到資料中心的網際網路用戶端要求。 一個角色可以有 0 個以上的輸入端點。 每個端點皆指出一個通訊協定 (HTTP、HTTPS 或 TCP) 和一個連接埠。 一個角色通常是設成具有兩個輸入端點：一個在連接埠 80 接聽 HTTP 要求，一個在連接埠 443 接聽 HTTPS 要求 下圖的範例顯示兩個不同角色使用不同的輸入端點來將用戶端要求導向自己。

![image][2]

當您在 Azure 中建立託管服務時，託管服務會獲派可供用戶端用來存取它的可定址公開 IP 位址。 建立託管服務時，您也必須選取與該 IP 位址對應的 URL 首碼。 此首碼必須是唯一的，因為您其實是在保留 *prefix*.cloudapp.net URL，讓別人無法使用它。 用戶端會使用 URL 與您的角色執行個體通訊。 您通常不會散佈或發佈 Azure *prefix*.cloudapp.net URL。 事實上，您會向屬意的 DNS 註冊機構購買 DNS 名稱，然後設定 DNS 名稱將用戶端要求重新導向此 Azure URL。 如需詳細資料，請參閱[在 Azure 中設定自訂網域名稱][Configuring a Custom Domain Name in Azure]。

## <a id="considerations"> </a>託管服務設計考量
設計在雲端環境中執行的應用程式時，有幾個需要考量之處，如延遲、高可用性和延展性。

在 Azure 中執行託管服務時，決定應用程式程式碼所在位置是很重要的考量。 應用程式通常是被部署至最接近用戶端的資料中心，以減少延遲並展現最佳效能。
不過，如果您對資料及其所在位置有某些管轄區或法律顧慮，可以選擇接近貴公司或接近資料的資料中心。 全球有六個資料中心可以裝載您的應用程式程式碼。 下表顯示可用的位置：

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr>

<td style="width: 100px;">
**國家/區域**


</td>

<td style="width: 200px;">
**子區域**


</td>
</tr>

<tr>

<td>
美國

</td>

<td>
中南部和中北部

</td>
</tr>

<tr>

<td>
歐洲

</td>

<td>
北歐和西歐

</td>
</tr>

<tr>

<td>
亞洲

</td>

<td>
東南亞和東亞

</td>
</tr>
</tbody>
</table>
建立託管服務時，請選取子區域，指出想要讓您程式碼在其中執行的位置。

為了達到高可用性和延展性，請務必將您的應用程式資料保存在可供多個角色執行個體存取的中央儲存機制。 為了協助達成此目標，Azure 提供數種儲存選項，如 Blob、資料表和 SQL Database。 如需這些儲存技術的詳細資訊，請參閱 [Azure 的資料儲存方案][Data Storage Offerings in Azure]一文。 下圖顯示 Azure 資料中心內的負載平衡器如何將用戶端要求分配給不同的角色執行個體，而這些角色執行個體全都可以存取同一個資料儲存體。

![image][3]

您通常會想要將應用程式程式碼和資料放在相同的資料中心內，因為如此一來，當應用程式程式碼存取資料時，延遲會較低 (也就是效能較佳)。 此外，因為資料是在相同的資料中心內移動，所以您無需負擔頻寬費用。

## <a id="scale"> </a>設計可調整的應用程式
有時，您可能會想要採用單一應用程式 (如簡單網站) 並將它託管於 Azure。 但是，您的應用程式通常會由數個一起運作的角色組成。 例如，在下圖中，「網站」角色有兩個執行個體、「訂單處理」角色有三個執行個體，而「報表產生器」角色有一個執行個體。 這些角色會一起運作，而它們的程式碼可以全部封裝在一起成為單一單位，再部署至 Azure。

![image][4]

將應用程式分割為不同角色，並讓每個角色各在一組專屬的角色執行個體 (即 VM) 上執行的主要原因，是為了方便對各角色進行調整。 例如，在假期旺季，許多客戶可能會向您的公司購買產品，因此您可能會想要增加執行「網站」角色的角色執行個體數目，並增加執行「訂單處理」角色的角色執行個體數目。 假期旺季過後，您可能會收到許多退回的產品，因此還是會需要許多「網站」執行個體，但是需要較少的「訂單處理」執行個體。 在一年的其他日子，您可能只需要一些「網站」和「訂單處理」執行個體。 但在上述所有日子，您都只需要一個「報表產生器」執行個體。 Azure 中角色部署的彈性可讓您輕鬆地調整應用程式，以符合您的商務需求。

在託管服務內的角色執行個體彼此溝通是很常見的情況。 例如，「網站」角色在接受客戶的訂單後，將訂單處理工作卸交給「訂單處理」角色執行個體。 將工作從某一組角色執行個體續轉給另一組執行個體的最佳方式，是使用 Azure 所提供的佇列技術 (「佇列服務」或「服務匯流排佇列」)。 使用佇列是整個過程中十分重要的一環。 佇列可讓託管服務獨立對各角色進行調整，讓您可以根據成本來平衡工作量。 如果佇列中的訊息數目隨著時間演進而增加，您可以向上調整「訂單處理」角色執行個體的數目。 如果佇列中的訊息數目隨著時間演進而減少，您可以向下調整「訂單處理」角色執行個體的數目。 如此一來，就只需要支付處理實際工作量所需的執行個體。

佇列也帶來可靠性。 向下調整「訂單處理」角色執行個體的數目時，Azure 會決定要終止的執行個體。 它可能會決定終止還在處理佇列訊息的執行個體。 不過，因為訊息處理未順利完成，所以另一個「訂單處理」角色執行個體會再次看到該訊息，而接收該訊息進行處理。 因為佇列訊息有這樣的可見度，所以訊息最終一定會獲得處理。 佇列也擔任負載平衡器的角色，因為其會將訊息有效分配給所有任何向其要求訊息的角色執行個體。

針對「網站」角色執行個體，您可以監視流入它們的流量，並決定將它們的數目向上或向下調整。 佇列可讓您獨立地調整「網站」角色執行個體數目，而不會影響到「訂單處理」角色執行個體。 這是十分強大的功能，可讓您具有許多彈性。 當然，如果您的應用程式還由其他角色組成，您可以新增更多佇列作為角色之間的溝通管道，以運用相同的調整與成本優勢。

## <a id="defandcfg"> </a>託管服務的定義和組態
若要將託管服務部署至 Azure，您還必須有服務定義檔和服務組態檔。 這兩個檔案都是 XML 檔案，且可讓您宣告性地指定託管服務的部署選項。 服務定義檔會描述構成您託管服務的所有角色和其溝通方式。 服務組態檔會描述每個角色的執行個體數目，以及用來設定每個角色執行個體的設定。

## <a id="def"> </a>服務定義檔
如前所述，服務定義檔 (CSDEF) 是一個 XML 檔案，其中描述構成完整應用程式的各種角色。 如需此 XML 檔案的完整結構描述，請至：[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx][]。
CSDEF 檔案會針對您想要讓應用程式具有的每個角色，各包含一個 WebRole 或 WorkerRole 元素。 部署角色作為 Web 角色 (使用 WebRole 元素)，表示程式碼將在含有 Windows Server 2008 和 Internet Information Server (IIS) 的角色執行個體上執行。
部署角色作為背景工作角色 (使用 WorkerRole 元素)，表示角色執行個體上將有 Windows Server 2008 (將不會安裝 IIS)。

您當然可以建立背景工作角色，並讓該角色使用某種其他機制來接聽連入的 Web 要求 (例如，您的程式碼可以建立和使用 .NET HttpListener)，然後再加以部署。 因為角色執行個體都是執行 Windows Server 2008，所以您的程式碼可以執行 Windows Server 上執行的應用程式通常可執行的任何作業。
2008.

針對每個角色，您可以指出想要該角色的執行個體使用的 VM 大小。 下表顯示目前可用的各種 VM 大小以及各自的屬性：

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr align="left" valign="top">

<td>
**VM 大小**


</td>

<td>
**CPU**


</td>

<td>
**RAM**

</td>

<td>
**磁碟**


</td>

<td>
**尖峰網路 I/O**


</td>
</tr>

<tr align="left" valign="top">

<td>
**超小型**


</td>

<td>
1 x 1.0 GHz

</td>

<td>
768 MB

</td>

<td>
20GB

</td>

<td>
\~5 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**小型**


</td>

<td>
1 x 1.6 GHz

</td>

<td>
1.75 GB

</td>

<td>
225GB

</td>

<td>
\~100 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**中型**


</td>

<td>
2 x 1.6 GHz

</td>

<td>
3.5 GB

</td>

<td>
490GB

</td>

<td>
\~200 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**大型**


</td>

<td>
4 x 1.6 GHz

</td>

<td>
7 GB

</td>

<td>
1TB

</td>

<td>
\~400 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**超大型**


</td>

<td>
8 x 1.6 GHz

</td>

<td>
14 GB

</td>

<td>
2TB

</td>

<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
您用作角色執行個體的每個 VM 會依每小時計費，而您的角色執行個體傳送到資料中心外的任何資料也都要計費。 進入資料中心的資料則不需計費。 如需詳細資訊，請參閱 [Azure 價格][Azure 價格]。 一般會建議使用許多小型角色執行個體，而不是少數大型執行個體，這樣您的應用程式比較容易從失敗當中恢復。 畢竟，您的角色執行個體愈少，要是其中一個角色執行個體發生災難性失敗，應用程式整體受到的影響就愈大。 而且，如前所述，您必須為每個角色至少必須部署兩個執行個體，才能獲得 Microsoft 所訂 99.95% 的服務等級協定。

在服務定義檔 (CSDEF) 中，您也會指定應用程式中每個角色的許多屬性。 以下是一些對您較有用的項目：

* **憑證**。 如果您想要加密資料，或您的 Web 服務支援 SSL，您就會使用憑證。 所有憑證都需要上傳至 Azure。 如需詳細資訊，請參閱[在 Azure 中管理憑證][Managing Certificates in Azure]。 此 XML 設定會將先前上傳的憑證安裝至角色執行個體的憑證存放區中，供您的應用程式程式碼取使用。
* **組態設定名稱**。 代表您想要應用程式在角色執行個體上執行時讀取的值。 組態設定的實際值是設定於服務組態檔 (CSCFG) 中，您隨時都可以更新該檔案，而且不需要重新部署程式碼。 事實上，您可以使用這類方式來編寫應用程式，以偵測變更的組態值，而不需要停機。
* **輸入端點**。 您可以在這裡指定想要透過 *prefix*.cloadapp.net URL 向外界公開的任何 HTTP、HTTPS 或 TCP 端點 (含連接埠)。 Azure 部署您的角色時，會自動在角色執行個體上設定防火牆。
* **內部端點**。 您可以在這裡指定想要向其他被當成應用程式一部分來部署之角色執行個體來公開的任何 HTTP 或 TCP 端點。 內部端點可讓應用程式內的所有角色執行個體彼此溝通，但不會供應用程式外的任何角色執行個體存取。
* **匯入模組**。 這些會選擇性地在您的角色執行個體上安裝有用的元件。 元件的種類包括診斷監視、遠端桌面和 Azure Connect (可讓您的角色執行個體透過安全通道來存取內部部署資源)。
* **本機儲存體**。 這會在角色執行個體上配置子目錄，供您的應用程式使用。 [Azure 的資料儲存方案][Data Storage Offerings in Azure]一文會提供詳細說明。
* **啟動工作**。 啟動工作提供一種方法，讓您在角色執行個體啟動時在其上安裝必備元件。 必要時，可讓這些工作晉升為以系統管理員身分執行。

## <a id="cfg"> </a>服務組態檔
服務組態檔 (CSCFG) 是一個 XML 檔案，其中描述不需重新部署應用程式就能變更的設定。 如需此 XML 檔案的完整結構描述，請至：[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]。
CSCFG 檔案會針對您應用程式中的每個角色各包含一個 Role 元素。 以下是一些您可以在 CSCFG 檔案中指定的項目：

* **作業系統版本**。 此屬性可讓您選取要讓所有執行您應用程式程式碼之角色執行個體來使用的作業系統 (OS) 版本。 此作業系統稱為「客體 OS」，而且每個新版本都包含客體 OS 發行當時可用的最新安全性修補程式和更新。 如果您將 osVersion 屬性值設定為 "\*"，則 Azure 會在有新的客體 OS 版本可用時，自動更新每個角色執行個體上的客體 OS。 不過，選取特定客體 OS 版本，即等於選擇略過自動更新。 例如，將 osVersion 屬性設為值 "WA-GUEST-OS-2.8\_201109-01"，會讓您的所有角色執行個體都取得下列網頁所述的內容：[http://msdn.microsoft.com/library/hh560567.aspx][http://msdn.microsoft.com/library/hh560567.aspx]。 如需客體 OS 版本的詳細資訊，請參閱[管理 Azure 客體 OS 的升級]。
* **執行個體**。 此元素的值指出您想要佈建來執行特定角色之程式碼的角色執行個體數目。 因為您可以將新的 CSCFG 檔案上傳至 Azure (而不需要重新部署應用程式)，所以很簡單就可以變更此元素的值，並上傳新的 CSCFG 檔案，以動態增加或減少執行應用程式程式碼的角色執行個體數目。 如此一來，您便可根據實際工作量需求，輕鬆地向上調整或向下調整應用程式，同時控制要為執行的角色執行個體負擔的費用。
* **組態設定值**。 此元素指出各設定 (其定義於 CSDEF 檔案中) 的值。 您的角色可以在執行時讀取這些值。 這些組態設定值通常用於對 SQL Database 或 Azure Storage 的連接字串，但是也可以用於任何您想要的用途。

## <a id="hostedservices"> </a>建立和部署託管服務
若要建立託管服務，您必須先移至 [Azure 管理入口網站]，然後指定 DNS 首碼和您希望程式碼最終執行於的資料中心來佈建託管服務。 接著，在開發環境中，建立服務定義檔 (CSDEF)、建置應用程式程式碼，並將所有這些檔案封裝 (壓縮) 成一個服務封裝檔 (CSPKG)。 您也必須準備服務組態檔 (CSCFG)。 為了部署您的角色，您會透過 Azure 服務管理 API 來上傳 CSPKG 和 CSCFG 檔案。 部署好之後，Azure 會在資料中心內佈建角色執行個體 (根據組態資料)、從封裝中擷取您的應用程式程式碼、將它複製至角色執行個體，然後啟動執行個體。 現在，您的程式碼已啟動並執行。

下圖顯示您在開發電腦上建立的 CSPKG 和 CSCFG 檔案。 CSPKG 檔案包含 CSDEF 檔案以及兩個角色的程式碼。 透過 Azure 服務管理 API 上傳 CSPKG 和 CSCFG 檔案之後，Azure 會在資料中心內建立角色執行個體。 在此範例中，CSCFG 檔案指出 Azure 應該為角色 \#1 建立三個執行個體，而為角色 \#2 建立兩個執行個體。

![image][5]

如需關於部署、升級和重新設定角色的詳細資訊，請參閱[部署和更新 Azure 應用程式][Deploying and Updating Azure Applications]一文。<a id="Ref" name="Ref"></a>

## <a id="references"> </a>參考
* [建立 Azure 託管服務][Creating a Hosted Service for Azure]
* [在 Azure 中管理託管服務][Managing Hosted Services in Azure]
* [將應用程式移轉到 Azure][Migrating Applications to Azure]
* [設定 Azure 應用程式][Configuring an Azure Application]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>作者：Jeffrey Richter (Wintellect)</p>

</div>

[Azure Application Model Benefits]: #benefits
[Hosted Service Core Concepts]: #concepts
[Hosted Service Design Considerations]: #considerations
[Designing your Application for Scale]: #scale
[Hosted Service Definition and Configuration]: #defandcfg
[The Service Definition File]: #def
[The Service Configuration File]: #cfg
[Creating and Deploying a Hosted Service]: #hostedservices
[References]: #references
[0]: ./media/application-model/application-model-3.jpg
[1]: ./media/application-model/application-model-4.jpg
[2]: ./media/application-model/application-model-5.jpg
[Configuring a Custom Domain Name in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
[Data Storage Offerings in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
[3]: ./media/application-model/application-model-6.jpg
[4]: ./media/application-model/application-model-7.jpg

[Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
[Managing Certificates in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
[管理 Azure 客體 OS 的升級]: http://msdn.microsoft.com/library/ee924680.aspx
[Azure 管理入口網站]: http://manage.windowsazure.com/
[5]: ./media/application-model/application-model-8.jpg
[Deploying and Updating Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
[Creating a Hosted Service for Azure]: http://msdn.microsoft.com/library/gg432967.aspx
[Managing Hosted Services in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
[Migrating Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
[Configuring an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx


<!--HONumber=Jan17_HO3-->


