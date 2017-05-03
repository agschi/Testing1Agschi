您可以使用 Visual Studio 的 [ **伺服器總管**] 來建立 Azure 儲存體佇列。

![伺服器總管 Blob][Image1]

1. 在 [檢視] 功能表上，選擇 [伺服器總管]。
2. 在 [伺服器總管] 中，展開您訂用帳戶的 [Azure] 節點，然後展開 [儲存體] 節點以及您在 Azure 儲存體已連線的服務中指定之儲存體帳戶的節點。
3. 選取 [佇列] 節點，然後從內容功能表中選擇 [建立佇列]。
4. 輸入容器的名稱，然後選擇 [ **確定**]。   

根據預設，新容器屬私人性質，您必須指定儲存體存取金鑰才能從此容器下載 Blob。 如果您想要讓容器中的檔案成為公用，請在 [伺服器總管] 中選取容器，再按 `F4` 以顯示 [屬性] 視窗。 將 [公用讀取權限] 設定為 [Blob]。 網際網路上的任何人都可以看到公用容器中的 Blob，但要有適當的存取金鑰，才能修改或刪除這些 Blob。

[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png

<!--HONumber=Jan17_HO3-->


