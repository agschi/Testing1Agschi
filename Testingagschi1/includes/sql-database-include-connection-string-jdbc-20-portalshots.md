<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>從 Azure 入口網站取得連接字串
使用 [Azure 入口網站](https://portal.azure.com/) 來取得用戶端程式與 Azure SQL Database 進行互動所需的連接字串：

1. 按一下 [瀏覽] >  [SQL Database]。
   
    ![選取 SQL][1-select-sql]
2. 在 [SQL 資料庫]  刀鋒視窗左上角附近的篩選文字方塊中輸入您的資料庫名稱。
   
    ![選取資料庫][2-select-database]
3. 按一下資料庫的資料列。
4. 在刀鋒視窗顯示您的資料庫之後，為了閱讀方便您可以按一下標準最小化控制項來摺疊用於瀏覽和資料庫篩選的刀鋒視窗。
5. 在您資料庫的刀鋒視窗上，按一下 [顯示資料庫連接字串] 。
6. 如果您想要使用 JDBC 連線庫，請複製標示為 **JDBC**的字串。
   
    ![複製資料庫的 JDBC 連接字串][3-get-connection-string]
7. 將連接字串資訊貼上您的用戶端程式碼。  您必須使用您真正的密碼取代 {your_password_here}。

如需詳細資訊，請參閱：<br/>[連接字串與組態檔](https://msdn.microsoft.com/library/ms378428.aspx)。

<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png


[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-string]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-jdbc.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
