## <a name="create-a-backup-vault"></a>建立備份保存庫
若要將 Windows Server 或 Data Protection Manager (DPM) 的檔案和資料備份至 Azure，或將 IaaS VM 備份至 Azure，您必須在要儲存資料的地區區域中建立一個備份保存庫。

下列步驟將引導您建立用來儲存備份的保存庫。

1. 登入 [管理入口網站](https://manage.windowsazure.com/)
2. 按一下 [新增]  >  [資料服務]  >  [復原服務]  >  [備份保存庫]，並選擇 [快速建立]。
   
    ![建立保存庫](./media/backup-create-vault/createvault1.png)
3. 針對 [ **名稱** ] 參數，輸入易記名稱來識別備份保存庫。 每個訂用帳戶皆需為唯一名稱。
4. 針對 [ **區域** ] 參數，選取備份保存庫的地區區域。 選項會決定將您的備份資料傳送到哪個地理區域。 透過選擇靠近您位置的地理區域，您可以在備份至 Azure 時減少網路延遲。
5. 按一下 [ **建立保存庫** ] 以完成工作流程。 要等備份保存庫建立好，可能需要一些時間。 若要檢查狀態，可以監控位於入口網站底部的通知。
   
    ![正在建立保存庫](./media/backup-create-vault/creatingvault1.png)
6. 建立備份保存庫之後，將會有一則訊息告訴您已成功建立保存庫。 該保存庫也會在「復原服務」的資源中列為 [ **使用中**]。
   
    ![正在建立保存庫狀態](./media/backup-create-vault/backupvaultstatus1.png)

### <a name="azure-backup---storage-redundancy-options"></a>Azure 備份 - 儲存體備援選項
> [!IMPORTANT]
> 識別儲存體備援選項的最佳時機，在於保存庫建立之後，以及任何電腦註冊至保存庫之前。 一旦項目已註冊至保存庫，儲存體備援選項即遭到鎖定且無法修改。
> 
> 

您的業務需求應會決定 Azure 備份後端儲存體的儲存體備援性。 如果您使用 Azure 作為主要的備份儲存體端點 (例如您正從 Windows Server 備份至 Azure)，您應該考慮挑選 (預設值) 異地備援儲存體選項。 您會在備份保存庫的 [設定]  選項下方看到此選項。

![GRS](./media/backup-create-vault/grs.png)

#### <a name="geo-redundant-storage-grs"></a>異地備援儲存體 (GRS)
GRS 可維護六個資料複本。 有了 GRS，您的資料會在主要區域內複寫三次，並在與主要區域相距甚遠的次要區域內複寫三次，提供最高等級的持久性。 將資料儲存在 GRS 時，若主要區域發生故障，則 Azure 備份可確保您的資料持續儲存在兩個不同區域中。

#### <a name="locally-redundant-storage-lrs"></a>本機備援儲存體 (LRS)
本機備援儲存體 (LRS) 可維護三個資料複本。 LRS 會在單一區域的單一設備內複寫三次。 LRS 可保護您的資料免於一般硬體故障，但無法避免整體 Azure 設備故障。

如果您使用 Azure 作為第三個備份儲存體端點 (例如您使用 SCDPM 讓本機備份複製內部部署，並使用 Azure 以符合長期保留需要)，您應該考慮從備份保存庫的 [設定] 選項選擇本機備援儲存體。 這將減少在 Azure 中儲存資料的成本，同時針對您可能會接受第三個複本的資料提供較低層級的持久性。

![LRS](./media/backup-create-vault/lrs.png)

