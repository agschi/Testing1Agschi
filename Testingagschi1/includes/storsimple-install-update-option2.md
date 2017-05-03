<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>從 Azure 傳統入口網站安裝 Update 1.2
1. 在 Azure 入口網站中，移至 [裝置]  頁面，並選取您的裝置。
2. 瀏覽至**裝置** > **設定**。
3. 在 [網路介面] 下，請先確認您有至少一個已啟用 iSCSI 的網路介面。 接著，找到具有已指派閘道的網路介面 (而非 DATA 0)。
4. 停用具有已指派閘道的網路介面，並儲存修改過的組態。 請注意，系統會保留網路介面設定，因此當您稍後重新啟用此網路介面時，入口網站會還原為原始設定。
5. 您現在可以 [使用 Azure 傳統入口網站安裝 Update 1.2](#install-update-12-via-the-azure-classic-portal)。 請遵循從本程序步驟 3 開始的指示。 當您安裝完所有更新後，便可重新啟用停用的網路介面。

