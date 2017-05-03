.NET 應用程式可以使用 **StackExchange.Redis** 快取用戶端，該用戶端可在 Visual Studio 中使用能簡化快取用戶端應用程式組態的 NuGet 套件來加以設定。 

> [!NOTE]
> 如需詳細資訊，請參閱 [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github 頁面和 [StackExchange.Redis 快取用戶端文件](http://github.com/StackExchange/StackExchange.Redis#documentation)。
> 
> 

若要在 Visual Studio 中使用 StackExchange.Redis NuGet 套件來設定用戶端應用程式，請在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選擇 [管理 NuGet 套件]。 

![Manage NuGet packages](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

在搜尋文字方塊中輸入 **StackExchange.Redis** 或 **StackExchange.Redis.StrongName**、從結果選取需要的版本，然後按一下 [安裝]。

> [!NOTE]
> 如果您偏好使用強式名稱版本的 **StackExchange.Redis** 用戶端程式庫，請選取 **StackExchange.Redis.StrongName**；否則選取 **StackExchange.Redis**。
> 
> 

![StackExchange.Redis NuGet package](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

NuGet 套件會為您的用戶端應用程式下載並新增必要的組件參考，以利用 StackExchange.Redis 快取用戶端來存取 Azure Redis 快取。

> [!NOTE]
> 如果您先前已經設定專案以使用 StackExchange.Redis，您就可以從 **NuGet 套件管理員**檢查套件更新。 若要檢查及安裝 StackExchange.Redis NuGet 套件的已更新版本，請按一下 [NuGet 套件管理員] 視窗中的 [更新]。 如果 StackExchange.Redis NuGet 套件有更新可用，您可以更新您的專案來使用已更新版本。
> 
> 

從 [工具] 功能表按一下 [NuGet 套用管理員]、[套件管理員主控台]，然後從 [Package Manager Console] 視窗執行下列命令，也可以安裝 StackExchange.Redis NuGet 套件。
    
```
Install-Package StackExchange.Redis
```
