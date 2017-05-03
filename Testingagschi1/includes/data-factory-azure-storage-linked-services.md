## <a name="azure-storage-linked-service"></a>Azure 儲存體連結服務
「Azure 儲存體連結服務」可讓您使用「帳戶金鑰」將 Azure 儲存體帳戶連結至 Azure Data Factory。 這可將 Azure 儲存體的全域存取權提供給 Data Factory。 下表提供 Azure 儲存體連結服務專屬 JSON 元素的描述。

| 屬性 | 說明 | 必要 |
|:--- |:--- |:--- |
| 類型 |類型屬性必須設為： **AzureStorage** |是 |
| connectionString |針對 connectionString 屬性指定連接到 Azure 儲存體所需的資訊。 |是 |

請參閱下列文章的步驟來檢視/複製 Azure 儲存體帳戶金鑰：[檢視、複製和重新產生儲存體存取金鑰](../articles/storage/storage-create-storage-account.md#manage-your-storage-account)。

**範例：**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

## <a name="azure-storage-sas-linked-service"></a>Azure 儲存體 SAS 連結服務
共用存取簽章 (SAS) 可提供您儲存體帳戶中資源的委派存取。 這表示您可以在無需分享您帳戶存取金鑰的情況下，將您儲存體帳戶中的物件有限權限授與用戶端，該用戶端便可在指定的時間期間內及使用指定的權限集來進行存取。 SAS 是一種 URI，URI 會在其查詢參數中包含通過驗證存取儲存體資源的所有必要資訊。 若要使用 SAS 存取儲存體資源，用戶端只需在適當的建構函式或方法中傳入 SAS 即可。 如需 SAS 的詳細資訊，請參閱 [共用存取簽章：了解 SAS 模型](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)

Azure 儲存體 SAS 連結服務可讓您使用共用存取簽章 (SAS)，將 Azure 儲存體帳戶連結到 Azure Data Factory。 這可將儲存體中所有/特定資源 (Blob/容器) 的受限制/時間界限存取權提供給 Data Factory。 下表提供 Azure 儲存體 SAS 連結服務專屬 JSON 元素的描述。 

| 屬性 | 說明 | 必要 |
|:--- |:--- |:--- |
| 類型 |類型屬性必須設為： **AzureStorageSas** |是 |
| sasUri |指定 Azure 儲存體資源 (例如 Blob、容器或資料表) 的共用存取簽章 URI。 如需詳細資訊，請參閱下列附註。 |是 |

**範例：**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

建立 **SAS URI**時，請考慮下列事項：  

* Azure Data Factory 僅支援 **服務 SAS**，不支援帳戶 SAS。 如需這兩種類型的詳細資料，請參閱 [共用存取簽章的類型](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) 。
* 需根據連結的服務 (讀取、寫入、讀取/寫入) 在 Data Factory 中的使用方式，在物件上設定適當的讀取/寫入「權限」  。
*  必須適當地設定。 請確定 Azure 儲存體物件的存取權不會在管線的作用中期間內過期。
* URI 應根據需要在正確的容器/Blob 或資料表層級建立。 Azure Blob 的 SAS URI 可允許 Data Factory 服務存取該特定的 Blob。 Azure Blob 容器的 SAS URI 可允許 Data Factory 服務逐一查看該容器中的 Blob。 如果您稍後需要提供更多/較少物件的存取權，或需要更新 SAS URI，請記得使用新的 URI 更新連結的服務。   

