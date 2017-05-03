Azure Marketplace 提供由 Microsoft、協力廠商公司及開放原始碼軟體計劃所開發的各種熱門 Web 應用程式。 除了用來連線到 [Azure 預覽入口網站](http://go.microsoft.com/fwlink/?LinkId=529715)的瀏覽器以外，從 Azure Marketplace 建立的 Web 應用程式不需要安裝任何軟體。 

在本教學課程中，您將了解：

* 如何透過 Azure Marketplace 建立新的 Web 應用程式。
* 如何透過 Azure 預覽入口網站部署 Web 應用程式。

您將建立使用預設範本的 WordPress 部落格。 下圖顯示完成的應用程式：

![Wordpress 部落格][13]

> [!NOTE]
> 如果您想在註冊 Azure 帳戶前開始使用 Azure App Service，請移至 [試用 App Service](https://azure.microsoft.com/try/app-service/)，即可在 App Service 中立即建立短期入門 Web 應用程式。 不需要信用卡；沒有承諾。
> 
> 

## <a name="create-a-web-app-in-the-portal"></a>在入口網站中建立 Web 應用程式
1. 登入 Azure 預覽入口網站。
2. 您可以按一下 **Marketplace** 圖示來開啟 Azure Marketplace：
   
    ![Marketplace 圖示][marketplace]
   
    或按一下儀表板右上角的 [新增] 圖示，然後選取清單底部的 **Marketplace**。
   
    ![建立新的][5]
3. 選取 [ **Web + Mobile**]。 搜尋 **WordPress**，然後按一下 **WordPress** 圖示。
   
    ![WordPress 來源清單][7]
4. 讀取 WordPress 應用程式的描述之後，選取 [建立] 。
5. 按一下 [ **Web 應用程式**]，然後提供設定 Web 應用程式所需的值。
   
    ![設定您的應用程式][8]
6. 按一下 [ **資料庫**]，然後提供設定 MySQL 資料庫所需的值。 
   
    ![設定資料庫][database]
7. 提供新資源群組的名稱。
   
    ![設定資源群組][groupname]
8. 如有必要，按一下 [訂用帳戶] ，然後指定要使用的訂用帳戶。 
9. 當您完成定義 Web 應用程式時，按一下 [建立] ，然後靜待新的 Web 應用程式建立。
   
   建立應用程式之後，您將會看到包含 Web 應用程式與資料庫的資源群組。
   
   ![顯示群組][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>啟動和管理 WordPress Web 應用程式
1. 按一下您的新 Web 應用程式，以查看應用程式的相關詳細資料。
   
    ![啟動儀表板][10]
2. 在 [Essentials] 頁面上，按一下 [瀏覽] 或 [Url] 底下的連結，以開啟 Web 應用程式的歡迎頁面。
   
    ![網站 URL][browse]
3. 如果您尚未安裝 WordPress，請輸入 WordPress 所需的適當組態資訊，然後按一下 [安裝 WordPress]  以完成組態設定，並開啟 Web 應用程式的登入頁面。
4. 按一下 [登入]  ，然後輸入您的認證。  
5. 您的新 WordPress Web 應用程式看起來類似下面的 Web 應用程式。    
   
    ![您的 WordPress 網站][13]

[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png


<!--HONumber=Feb17_HO3-->


