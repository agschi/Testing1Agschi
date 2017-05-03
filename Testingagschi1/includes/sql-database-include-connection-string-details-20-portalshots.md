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
5. 請記下 **SQL 資料庫**名稱和**伺服器名稱**。  使用者名稱會是 yourusername@yourserver.
   
    ![取得連線詳細資料][3-get-connection-details]
6. 將連線詳細資料貼上您的用戶端程式碼。  您必須使用您真正的密碼取代 {your_password_here}。

<!--
Could not find a good link for PHP

For more information, see:<br/>[Connection Strings and Configuration Files](https://msdn.microsoft.com/library/ms378428.aspx).
-->


<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png

[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-details]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-details.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
