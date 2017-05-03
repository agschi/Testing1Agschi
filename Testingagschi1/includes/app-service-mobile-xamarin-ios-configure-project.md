#### <a name="configure-the-ios-project-in-xamarin-studio"></a>在 Xamarin Studio 中設定 iOS 專案
1. 在 Xamarin.Studio 中，開啟 **Info.plist**，然後使用您稍早以新應用程式識別碼建立的套件組合識別碼，來更新 [套件組合識別碼]。

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. 向下捲動到 [背景模式]。 選取 [啟用背景模式] 方塊與 **[遠端通知]** 方塊。

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. 按兩下 [方案面板] 中的專案，以開啟 [專案選項]。
4. 選擇 [建置] 下的 [iOS 套件組合簽署]，然後選取對應的身分識別以及您為此專案設定的 [佈建設定檔]。

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   這將確保專案使用新的設定檔進行程式碼簽署。 如需官方 Xamarin 裝置佈建文件，請參閱 [Xamarin 裝置佈建]。

#### <a name="configure-the-ios-project-in-visual-studio"></a>在 Visual Studio 中設定 iOS 專案
1. 在 Visual Studio 中，以滑鼠右鍵按一下專案，然後按一下 [屬性] 。
2. 在 [屬性] 頁面中，按一下 [iOS 應用程式] 索引標籤，然後使用您稍早建立的識別碼更新 [識別碼]。

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. 在 [iOS 套件組合簽署] 索引標籤中，選取對應的身分識別以及您為此專案設定的佈建設定檔。

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    這將確保專案使用新的設定檔進行程式碼簽署。 如需官方 Xamarin 裝置佈建文件，請參閱 [Xamarin 裝置佈建]。
4. 按兩下 Info.plist 以開啟，並啟用 [背景模式] 下的 **RemoteNotifications**。

[Xamarin 裝置佈建]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
