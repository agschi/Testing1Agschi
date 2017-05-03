遵循下列步驟，在執行 Windows Server 的虛擬機器上安裝並執行 MongoDB。

> [!IMPORTANT]
> MongoDB 安全性功能，例如驗證和 IP 位址繫結，均非預設為已啟用。 安全性功能應該在將 MongoDB 部署到生產環境前加以啟用。  如需詳細資訊，請參閱[安全性和驗證](http://www.mongodb.org/display/DOCS/Security+and+Authentication)。
>
>

1. 使用遠端桌面連線到虛擬機器時，請從虛擬機器上的 [ **開始** ] 功能表中開啟 Internet Explorer。
2. 選取右上方的 [工具]  按鈕。  在 [網際網路選項] 中，選取 [安全性] 索引標籤，接著選取 [受信任的網站] 圖示，最後按一下 [網站] 按鈕。 將 *https://\*.mongodb.org* 新增至受信任的網站清單。
3. 移至[下載 - MongoDB](https://www.mongodb.com/download-center#community)。
4. 尋找**社群伺服器**的**目前穩定版本**，在 Windows 資料行中選取最新的 **64 位元**版本。 進行下載，然後執行 MSI 安裝程式。
5. MongoDB 通常會安裝在 C:\Program Files\MongoDB。 在桌面上搜尋環境變數，然後將 MongoDB 二進位檔路徑新增至 PATH 變數。 例如，您可能會在電腦的 C:\Program Files\MongoDB\Server\3.4\bin 發現二進位檔。
6. 在您在先前步驟中建立的資料磁碟中建立 MongoDB 資料和記錄目錄 (例如磁碟機 **F:**)。 在 [開始] 功能表中，選取 [命令提示字元] 以開啟命令提示字元視窗。  輸入：

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. 若要執行資料庫，執行：

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    在 mongod.exe 伺服器啟動和預先配置日誌檔案時，所有記錄訊息都會傳送至 *F:\MongoLogs\mongolog.log* 檔案。 MongoDB 可能需要花費數分鐘來預先配置日誌檔案，並開始接聽連線。 當您的 MongoDB 執行個體正在執行時，命令提示字元會專注於這項工作。
8. 若要啟動 MongoDB 管理殼層，請從 [開始] 功能表中開啟另一個命令視窗，並輸入下列命令：

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    資料庫由插入項目建立。
9. 或者，您可以利用服務形式安裝 mongod.exe：

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    隨即安裝一個名為 MongoDB 的服務，其說明為 "Mongo DB"。 `--logpath` 選項必須用來指定記錄檔，因為執行中的服務沒有命令視窗可以顯示輸出。  `--logappend` 選項指出重新啟動服務會導致輸出附加在現有的記錄檔案中。  `--dbpath` 選項指出資料目錄的位置。 如需更多服務相關的命令列選項，請參閱[服務相關的命令列選項][MongoWindowsSvcOptions]。

    若要啟動服務，請執行此命令：

        C:\> net start MongoDB
10. 現在，MongoDB 已安裝並正在執行，您必須在 Windows 防火牆中開啟一個連接埠，才能遠端連線至 MongoDB。  在 [開始] 功能表中，選取 [管理員工具]，然後選取 [具備進階安全性的 Windows 防火牆]。
11. a) 在左側窗格中，選取 [內送規則]。  在右側的 [動作] 窗格中，選取 [新增規則...]。

    ![Windows 防火牆][Image1]

    b) 在 [新內送規則精靈] 中，選取 [連接埠] 並按一下 [下一步]。

    ![Windows 防火牆][Image2]

    c) 選取 [TCP] 以及 [指定本機連接埠]。  指定連接埠為 "27017" (MongoDB 接聽的預設連接埠) 並按一下 [下一步]。

    ![Windows 防火牆][Image3]

    選取 [允許連線] 並按 [下一步]。

    ![Windows 防火牆][Image4]

    e) 再次按 [下一步]。

    ![Windows 防火牆][Image5]

    f) 指定規則名稱，例如 "MongoPort"，並按一下 [結束]。

    ![Windows 防火牆][Image6]

12. 如果您在建立虛擬機器時沒有設定 MongoDB 的端點，您現在可以進行此動作。 您需要防火牆規則和端點兩者，才能遠端連線至 MongoDB。

  在 Azure 入口網站中，依序按一下 [虛擬機器 (傳統)]、新虛擬機器的名稱以及 [端點]。

    ![端點][Image7]

13. 按一下 [新增] 。

14. 新增名為 "Mongo" 的端點，通訊協定為 **TCP**，且**公開**以及**私人**連接埠均設為 "27017"。 開啟此連接埠可允許 MongoDB 遠端存取。

    ![端點][Image9]

> [!NOTE]
> 連接埠 27017 是 MongoDB 使用的預設連接埠。 在啟動 mongod.exe 伺服器時指定 `--port` 參數，即可變更此預設連接埠。 請務必為先前指示中的防火牆和 "Mongo" 端點指定相同的連接埠號碼。
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
