---
title: "教學課程：Azure Active Directory 與 PolicyStat 整合 | Microsoft Docs"
description: "了解如何使用 PolicyStat 搭配 Azure Active Directory 來啟用單一登入、自動佈建和更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 04a045f41965b093aab71e59cd9b5f328b44de84
ms.openlocfilehash: 8290d90f22a80d5c585192b17cfa0aeba00aa891
ms.lasthandoff: 02/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>教學課程：Azure Active Directory 與 PolicyStat 整合
本教學課程的目的是要示範 Azure 與 PolicyStat 的整合。  

本教學課程中說明的案例假設您已經具有下列項目：

* 有效的 Azure 訂用帳戶
* PolicyStat 租用戶

完成本教學課程之後，您已指派至 PolicyStat 的 Azure AD 使用者就能夠從您的 PolicyStat 公司網站 (服務提供者起始登入)，或使用 [存取面板簡介](active-directory-saas-access-panel-introduction.md)來單一登入應用程式。

本教學課程中說明的案例由下列建置組塊組成：

1. 啟用 PolicyStat 的應用程式整合
2. 設定單一登入
3. 設定使用者佈建
4. 指派使用者

![案例](./media/active-directory-saas-policystat-tutorial/IC808662.png "案例")

## <a name="enabling-the-application-integration-for-policystat"></a>啟用 PolicyStat 的應用程式整合
本節的目的是要說明如何啟用 PolicyStat 的應用程式整合。

**若要啟用 PolicyStat 的應用程式整合，請執行下列步驟：**

1. 在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory] 。
   
   ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
   ![應用程式](./media/active-directory-saas-policystat-tutorial/IC700994.png "應用程式")
4. 按一下頁面底部的 [新增]  。
   
   ![新增應用程式](./media/active-directory-saas-policystat-tutorial/IC749321.png "新增應用程式")
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
   ![從資源庫新增應用程式](./media/active-directory-saas-policystat-tutorial/IC749322.png "從資源庫新增應用程式")
6. 在**搜尋方塊**中，輸入 **PolicyStat**。
   
   ![應用程式資源庫](./media/active-directory-saas-policystat-tutorial/IC808627.png "應用程式資源庫")
7. 在結果窗格中，選取 [PolicyStat]，然後按一下 [完成] 以新增應用程式。
   
   ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
   
## <a name="configuring-single-sign-on"></a>設定單一登入

本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己在 Azure AD 中的帳戶在 PolicyStat 中進行驗證。  

PolicyStat 應用程式需要特定格式的 SAML 判斷提示，需要您加入自訂屬性對應到您的 **saml token 屬性** 設定。  

以下螢幕擷取畫面顯示上述的範例。

![屬性](./media/active-directory-saas-policystat-tutorial/IC808628.png "屬性")

**若要設定單一登入，請執行下列步驟：**

1. 在 Azure 傳統入口網站的 [PolicyStat] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-policystat-tutorial/IC808629.png "設定單一登入")
2. 在 [要如何讓使用者登入 PolicyStat] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。
   
   ![設定單一登入](./media/active-directory-saas-policystat-tutorial/IC808630.png "設定單一登入")
3. 在 [設定應用程式設定] 頁面的 [登入 URL] 文字方塊輸入使用者登入您 URL PolicyStat 應用程式所使用的 URL (如： “https://demo-azure.policystat.com” )，然後按 [下一步]。
   
   ![設定應用程式設定](./media/active-directory-saas-policystat-tutorial/IC808631.png "進行應用程式設定")
4. 在 [設定在 PolicyStat 單一登入] 頁面上，按一下 [下載中繼資料]，然後將中繼資料檔儲存在您的電腦中。
   
   ![設定單一登入](./media/active-directory-saas-policystat-tutorial/IC808632.png "設定單一登入")
5. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 PolicyStat 公司網站。
6. 按一下 [管理] 索引標籤，然後按一下左側導覽窗格中的 [單一登入設定]。
   
   ![系統管理員功能表](./media/active-directory-saas-policystat-tutorial/IC808633.png "系統管理員功能表")
7. 在 [安裝] 區段中，選取 [啟用單一登入整合]。
   
   ![單一登入設定](./media/active-directory-saas-policystat-tutorial/IC808634.png "單一登入設定")
8. 按一下 [設定屬性]，然後在 [設定屬性]  區段中，執行下列步驟：
   
   ![單一登入設定](./media/active-directory-saas-policystat-tutorial/IC808635.png "單一登入設定")
   
   1. 在 [使用者名稱屬性] 文字方塊中，輸入 **uid**。
   2. 在 [名字屬性] 文字方塊中，輸入**名字**。
   3. 在 [姓氏屬性] 文字方塊中，輸入**姓氏**。
   4. 在 [電子郵件屬性] 文字方塊中，輸入**電子郵件地址**。
   5. 按一下 [儲存變更] 。
9. 按一下 [您的 IDP 中繼資料]，然後在 [您的 IDP 中繼資料] 區段中，執行下列步驟：
   
   ![單一登入設定](./media/active-directory-saas-policystat-tutorial/IC808635.png "單一登入設定")
   
   1. 開啟您下載的中繼資料檔案，然後複製內容並貼到 [您的身分識別提供者中繼資料] 文字方塊中。
   2. 按一下 [儲存變更] 。
10. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。
    
    ![設定單一登入](./media/active-directory-saas-policystat-tutorial/IC771723.png "設定單一登入")
  
11. 在頂端的功能表中，按一下 [屬性] **屬性** to open the **SAML Token 屬性** 對話方塊。
    
    ![屬性](./media/active-directory-saas-policystat-tutorial/IC795920.png "屬性")
12. 若要加入必要的屬性對應，請執行下列步驟：
    
    ![屬性](./media/active-directory-saas-policystat-tutorial/IC804823.png "屬性")
    
   1. 按一下 [加入使用者屬性] 。
   2. 在 [屬性名稱] 文字方塊中，輸入 **uid**。
   3. 在 [屬性值] 文字方塊中，選取 [ExtractMailPrefix()]。    
   4. 從 [郵件] 清單選取 [User.mail]。
   5. 按一下頁面底部的 [新增] 。

##<a name="configuring-user-provisioning"></a>設定使用者佈建

為了讓 Azure AD 使用者能夠登入 PolicyStat，必須將他們佈建到 PolicyStat。  

PolicyStat 支援即時使用者佈建。 這表示您不需要手動將使用者新增至 PolicyStat。  
使用者第一次透過單一登入登入時就會被自動新增。

> [!NOTE]
> 您可以使用任何其他的 PolicyStat 使用者帳戶建立工具或 PolicyStat 提供的 API，佈建 AAD 使用者帳戶。
> 
> 

## <a name="assigning-users"></a>指派使用者
若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

**若要指派使用者給 PolicyStat，請執行下列步驟：**

1. 在 Azure 傳統入口網站中建立測試帳戶。
2. 在 [PolicyStat] 應用程式整合頁面中，按一下 [指派使用者]。
   
   ![指派使用者](./media/active-directory-saas-policystat-tutorial/IC808636.png "指派使用者")
3. 選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。
   
   ![是](./media/active-directory-saas-policystat-tutorial/IC767830.png "是")

如果要測試您的單一登入設定，請開啟存取面板。 如需 [存取面板] 的詳細資訊，請參閱 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。


