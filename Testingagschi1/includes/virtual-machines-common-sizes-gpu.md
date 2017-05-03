NC 和 NV 大小也稱為已啟用 GPU 功能的執行個體。 這些是包含 NVIDIA GPU 卡並已針對不同情況和使用案例進行最佳化的特製化虛擬機器。 NV 大小會針對遠端視覺效果、串流、遊戲、編碼及利用 OpenGL 和 DirectX 這類架構的 VDI 案例進行最佳化和設計。 NC 大小則是對運算密集型和網路密集型應用程式和演算法 (包括以 CUDA 和 OpenCL 為基礎的應用程式及模擬) 有更深入的最佳化。 


NV 執行個體是由 NVIDIA 的 Tesla M60 GPU 卡和 NVIDIA GRID 提供技術支援，適用於桌面加速應用程式和虛擬桌面，可供客戶從中將其資料或模擬視覺化。 使用者將能夠在 NV 執行個體上，將其圖形密集型工作流程視覺化以獲得較佳的圖形功能，此外還能夠執行單精確度工作負載，例如編碼和轉譯。 Tesla M60 以雙 GPU 設計提供 4096 個 CUDA 核心，最多可達 36 個 1080p H.264 的串流。 

NC 執行個體是由 NVIDIA 的 Tesla K80 卡提供技術支援。 使用者現在可以藉由將 CUDA 用於能源探勘應用程式、當機模擬、光線追蹤轉譯、深入學習等等，更快速地處理資料。 Tesla K80 以雙 GPU 設計提供 4992 個 CUDA 核心，最多可達 2.91 個兆級浮點運算 (Teraflop) 的雙精確度效能及最多 8.93 個兆級浮點運算的單精確度效能。

## <a name="nv-instances"></a>NV 執行個體

| 大小 | CPU 核心 | 記憶體：GiB | 本機 SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 |

1 GPU = 1/2 M60 卡。

**受支援的作業系統**

* Windows Server 2016、Windows Server 2012 R2 - 請參閱[適用於 Windows 的 N 系列驅動程式設定](../articles/virtual-machines/windows/n-series-driver-setup.md)

## <a name="nc-instances"></a>NC 執行個體

| 大小 | CPU 核心 | 記憶體：GiB | 本機 SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 |

1 GPU = 1/2 K80 卡。

*支援 RDMA

**受支援的作業系統**

* Windows Server 2016、Windows Server 2012 R2 - 請參閱[適用於 Windows 的 N 系列驅動程式設定](../articles/virtual-machines/windows/n-series-driver-setup.md)
* Ubuntu 16.04 LTS - 請參閱[適用於 Linux 的 N 系列驅動程式設定](../articles/virtual-machines/linux/n-series-driver-setup.md)

<br>

