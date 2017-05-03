組織使用更多的 [軟體即服務 (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) 應用程式來提高產能，因為雲端技術和工具變得更容易使用。 當 SaaS App 的數目增加時，會提高系統管理員對於管理帳戶和存取權限的挑戰性，而對於使用者的挑戰則是記住其不同的密碼。 個別管理這些應用程式會建立額外的工作且安全性變低。

* 必須持續追蹤許多密碼的員工傾向使用較不安全的方法來記憶它們，可能是寫下密碼，或者在許多帳戶間使用相同的密碼。
* 當新員工報到或舊員工離職時，他們的所有帳戶都必須個別佈建或取消佈建。
* 此外，員工可能在未經 IT 同意的情況下開始使用 SaaS App 來工作，這表示表示他們在 IT 系統管理員尚未核准且無法監視的系統上建立自己的帳戶。  

所有這些挑戰的解決方案就是單一登入 (SSO)。 這是管理多個 App 以及為使用者提供一致性單一登入體驗的最簡單方式。 Azure Active Directory (Azure AD) 提供穩固的 SSO 解決方案，且具有許多可用且預先整合的應用程式，以及系統管理員能夠快速設定新的 App 並開始佈建使用者的教學課程。

## <a name="how-does-azure-active-directory-integrate-apps"></a>Azure Active Directory 如何整合 App？
Azure AD 可讓您整合 App 和佈建的帳戶。 這可透過下列兩種方法之一來完成。

* 如果 App 已預先整合於 App 程式庫中，您可以瀏覽至該入口網站來安裝 App 並進行設定，以允許使用 SSO。 針對任何程式庫 App，您一開始可以遵循 App 程式庫和 Azure 入口網站中顯示的簡單逐步指示來啟用單一登入。
* 如果 App 不在持程式庫中，您仍然可以將 Azure AD 中大部分的 App 設定為自訂 App。 這需要設定更技術性的專業知識。 您可以加入支援 SAML 2.0 的任何應用程式做為同盟應用程式，或者加入具有 HTML 登入頁面的任何應用程式做為密碼 SSO 應用程式。

如果使用者已針對未受到 IT 管理的 SaaS App 建立自己的帳戶，則 [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) 工具可提供解決方案。 此工具會監視網路流量，來識別整個組織中正在使用哪些 App，以及使用每個 App 的人數。 IT 可以使用此資訊來了解使用者偏好使用哪些 App，並決定要整合到 Azure AD 以使用 SSO 的 App。  

當您將 App 整合到 Azure AD 時，可以將使用者已建立的應用程式身分識別對應至其各自的 Azure AD 身分識別。  

