---
title: Azure Active Directory ile ilgili SSS | Microsoft Belgeleri
description: "Azure Active Directory SSS bölümünde, Azure ve Azure Active Directory, parola yönetimi ve uygulama erişimi konusunda sorular yanıtlanmaktadır."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/07/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 23c49393a115d9cd0ac3a1b0f146e9dcc780347e
ms.openlocfilehash: 42be5d71d8f22a2eb06f7ca0ebd4c33fb3d8bebe


---
# <a name="azure-active-directory-faq"></a>Azure Active Directory ile ilgili SSS
Azure Active Directory (Azure AD), kimlik, erişim yönetimi ve güvenliği tüm yönleriyle kapsayan bir hizmet olarak kimlik (IDaaS) çözümüdür.

Daha fazla bilgi için bkz. [Azure Active Directory nedir?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Azure ve Azure Active Directory erişimi
**S: Klasik Azure portalında \(https://manage.windowsazure.com\) Azure AD'ye erişmeye çalıştığımda neden "Abonelik bulunamadı" yanıtını alıyorum?**

**Y:** Klasik Azure portalına erişmek için her kullanıcının bir Azure aboneliğiyle birlikte izinleri olmalıdır. Ücretli bir Office 365 veya Azure AD aboneliğiniz varsa tek seferlik etkinleştirme adımı için [http://aka.ms/accessAAD](http://aka.ms/accessAAD) sayfasına gidin. Aksi takdirde, ücretsiz bir [Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) veya ücretli aboneliği etkinleştirmeniz gerekir.

Daha fazla bilgi için bkz.

* [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)
* [Azure'da Office 365 aboneliğinize yönelik dizini yönetme](active-directory-manage-o365-subscription.md)

- - -
**S: Azure AD, Office 365 ve Azure arasında ne gibi bir ilişki söz konusudur?**

**Y:** Azure AD, tüm web hizmetlerine yönelik ortak kimlik ve erişim işlevleri sunar. Office 365, Microsoft Azure, Intune veya diğer uygulamalardan hangisini kullanırsanız kullanın bu hizmetlerin tümü için oturum açma ve erişim yönetimini etkinleştirmek üzere zaten Azure AD'yi kullanırsınız.

Web hizmetlerini kullanacak şekilde ayarlanmış tüm kullanıcılar, bir veya daha fazla Azure AD örneğinde kullanıcı hesabı olarak tanımlanır. Bulut uygulama erişimi gibi ücretsiz Azure AD özellikleri için bu hesapları ayarlayabilirsiniz.

Enterprise Mobility + Security gibi ücretli Azure AD hizmetleri, kurumsal ölçekte kapsamlı yönetim ve güvenlik çözümleriyle Office 365 ve Microsoft Azure gibi diğer web hizmetlerini tamamlar.
- - -
**S:  Neden Azure portalında oturum açabiliyorken klasik Azure portalında oturum açamıyorum?**

**Y:** Azure portalı geçerli bir abonelik gerektirmezken, klasik portal geçerli bir abonelik gerektirir.  Bir aboneliğiniz yoksa, klasik portalda oturum açamazsınız.
- - -
**S:  Abonelik Yöneticisi ile Dizin Yöneticisi arasındaki farklar nelerdir?**

**C:** Varsayılan olarak, Azure’a kaydolurken size Abonelik Yöneticisi rolü atanır. Abonelik yöneticisi bir Microsoft hesabı ya da Azure aboneliğinin ilişkili olduğu dizinden bir çalışma veya okul hesabı kullanabilir.  Bu rol Azure portaldaki hizmetleri yönetme yetkisine sahiptir.

Başkalarının aynı aboneliği kullanarak oturum açması ve hizmetlere erişmesi gerekiyorsa bu kişileri ortak yönetici olarak ekleyebilirsiniz. Bu rol hizmet yöneticisi ile aynı erişim ayrıcalıklarına sahiptir, ancak aboneliklerin Azure dizinleriyle ilişkisini değiştiremez.  Abonelik yöneticileri hakkında ek bilgi için bkz. [Azure yönetici rolleri ekleme veya değiştirme](../billing-add-change-azure-subscription-administrator.md) ve [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md).


Azure AD, dizin ve kimlikle ilgili özelliklerin yönetilmesine ilişkin farklı bir yönetim rolleri dizisine sahiptir.  Bu yöneticiler, Azure portal veya klasik Azure portalında çeşitli özelliklere erişebilir. Yöneticinin rolü, kullanıcı oluşturma veya düzenleme, diğer kullanıcılara yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme gibi yetkileri belirler.  Azure AD dizin yöneticileri ve rolleri hakkında daha fazla bilgi için bkz. [Azure Active Directory’de yönetici rolü atama](active-directory-assign-admin-roles.md).

Ayrıca, Enterprise Mobility + Security gibi ücretli Azure AD hizmetleri, kurumsal ölçekte kapsamlı yönetim ve güvenlik çözümleriyle Office 365 ve Microsoft Azure gibi diğer web hizmetlerini tamamlar.

- - -
**S: Azure AD kullanıcı lisanslarımın ne zaman sona ereceğini gösteren bir rapor var mı?**

**C:** Hayır.  Bu özellik şu an kullanılamıyor.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Karma Azure AD ile çalışmaya başlama


**S: Ortak çalışan olarak eklendiğimde kiracıdan nasıl ayrılabilirim?**

**Y:** Başka bir kuruluşun kiracısına ortak çalışan olarak eklendiğinizde, kiracılar arasında geçiş yapmak için sağ üst köşedeki "kiracı değiştiricisi" seçeneğini kullanabilirsiniz.  Şu anda, davet eden kuruluştan ayrılma seçeneği mevcut değildir ve Microsoft bu işlevselliği sunmak için çalışmaktadır.  Bu özellik kullanılabilir oluncaya kadar, davet eden kuruluştan sizi kiracısından kaldırmasını isteyebilirsiniz.
- - -
**S: Şirket içi dizinimi Azure AD'ye nasıl bağlayabilirim?**

**Y:** Şirket içi dizininizi Azure AD'ye bağlamak için Azure AD Connect'i kullanabilirsiniz.

Daha fazla bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

- - -
**S: Şirket içi dizinim ve bulut uygulamalarım arasında SSO'yu nasıl ayarlarım?**

**Y:** Çoklu oturum açmayı (SSO) yalnızca şirket içi dizininiz ve Azure AD arasında ayarlamanız yeterlidir. Bulut uygulamalarınıza Azure AD üzerinden eriştiğiniz sürece, hizmet otomatik olarak kullanıcılarınızı şirket içi kimlik bilgileriyle doğru şekilde kimlik doğrulaması gerçekleştirmeye yönlendirir.

Şirket içi konumdan SSO uygulama işlemi, Active Directory Federation Services (ADFS) gibi federasyon çözümleri veya parola karma eşitlemesini yapılandırma işlemi ile kolayca gerçekleştirilebilir. Azure AD Connect yapılandırma sihirbazını kullanarak her iki seçeneği de kolayca dağıtabilirsiniz.

Daha fazla bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

- - -
**S: Azure AD, kuruluşumdaki kullanıcılar için bir self servis portal sağlar mı?**

**Y:** Evet, AD size kullanıcı self servis ve uygulama erişimi için [Azure AD Erişim Paneli](http://myapps.microsoft.com)'ni sağlar. Office 365 müşterisiyseniz aynı işlevlerin birçoğunu Office 365 portalında da bulabilirsiniz.

Daha fazla bilgi için bkz. [Erişim Paneli'ne Giriş](active-directory-saas-access-panel-introduction.md).

- - -
**S: Azure AD, şirket içi altyapımı yönetmeme yardımcı olur mu?**

**Y:** Evet. Azure AD Premium sürümü size Azure AD Connect Health olanağını sağlar. Azure AD Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur.  

Daha fazla bilgi için bkz. [Bulutta şirket içi kimlik altyapınızı ve eşitleme hizmetlerinizi izleme](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Parola yönetimi
**S: Azure AD parola geri yazma özelliğini parola eşitleme olmadan kullanabilir miyim? (Bu senaryoda Azure AD self servis parola sıfırlama (SSPR) özelliğinin parola geri yazma ile birlikte kullanılması ve parolaların buluta depolanmaması mümkün mü?)**

**Y:** Geri yazmayı etkinleştirmek için Active Directory parolalarınızı Azure AD ile eşitlemeniz gerekmez. Birleştirilmiş bir ortamda Azure AD çoklu oturum açma (SSO), kullanıcının kimliğini doğrulamak için şirket içi dizini kullanır. Bu senaryoda, şirket içi parolanın Azure AD'de izlenmesi gerekmez.

- - -
**S: Bir parolanın Active Directory şirket içi konumuna geri yazılması ne kadar zaman alır?**

**Y:** Parola geri yazma işlemi gerçek zamanlı olarak gerçekleşir.

Daha fazla bilgi için bkz. [Parola yönetimine başlarken](active-directory-passwords-getting-started.md).

- - -
**S: Parola geri yazma özelliğini bir yönetici tarafından yönetilen parolalarla kullanabilir miyim?**

**Y:** Evet, parola geri yazma özelliğini etkinleştirdiyseniz bir yönetici tarafından gerçekleştirilen parola işlemleri, şirket içi ortamınıza geri yazılır.  

Parola ile ilgili daha fazla soru yanıtı için bkz. [Parola yönetimi ile ilgili sık sorulan sorular](active-directory-passwords-faq.md).
- - -
**S:  Parolamı değiştirirken mevcut Office 365/Azure AD parolamı hatırlayamazsam ne olur?**

**Y:** Bu tür bir durum için birkaç seçenek vardır.  Varsa, self servis parola sıfırlama (SSPR) özelliğini kullanın.  SSPR’nin çalışıp çalışmaması nasıl yapılandırıldığına bağlıdır.  Daha fazla bilgi için bkz. [Parola sıfırlama portalının çalışması](active-directory-passwords-learn-more.md#how-does-the-password-reset-portal-work).

Office 365 kullanıcıları için yöneticiniz [Kullanıcı parolalarını sıfırlama](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US) bölümünde özetlenen adımları kullanarak parolayı sıfırlayabilir.

Azure AD hesapları için yöneticiler aşağıdakilerden birini kullanarak parolaları sıfırlayabilir:

- [Azure portalda hesapları sıfırlama](active-directory-users-reset-password-azure-portal.md)
- [Klasik portalda hesapları sıfırlama](active-directory-create-users-reset-password.md)
- [PowerShell’i kullanma](https://docs.microsoft.com/en-us/powershell/msonline/v1/Set-MsolUserPassword?redirectedfrom=msdn)


- - -
## <a name="application-access"></a>Uygulama erişimi
**S: Azure AD ile önceden tümleştirilmiş olan uygulamaların ve özelliklerinin listesini nereden bulabilirim?**

**Y:** Azure AD, Microsoft'a, uygulama hizmeti sağlayıcılarına ve iş ortaklarına ait 2.600'ü aşkın önceden tümleştirilmiş uygulama içerir. Önceden tümleştirilmiş tüm uygulamalar çoklu oturum açmayı (SSO) destekler. SSO, uygulamalarınıza erişmek için kurumsal kimlik bilgilerinizi kullanmanıza olanak tanır. Uygulamalardan bazıları aynı zamanda otomatik hazırlama ve sağlamayı kaldırma özelliklerini destekler.

Önceden tümleştirilmiş uygulamaların tam listesi için bkz. [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**S: İhtiyacım olan uygulama Azure AD marketinde yoksa ne olur?**

**Y:** Azure AD Premium ile istediğiniz uygulamayı ekleyip yapılandırabilirsiniz. Uygulamanızın özelliklerine ve tercihlerinize bağlı olarak, SSO'yu ve otomatik hazırlamayı yapılandırabilirsiniz.  

Daha fazla bilgi için bkz.

* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](active-directory-saas-custom-apps.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)

- - -
**S: Kullanıcılar Azure AD'yi kullanarak uygulamalarda nasıl oturum açar?**

**Y:** Azure AD, kullanıcılara uygulamalarını görüntülemeleri ve bunlara erişmeleri için birçok yol sağlar. Bunlardan bazıları aşağıda verilmiştir:

* Azure AD erişim paneli
* Office 365 uygulama başlatıcı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Daha fazla bilgi için bkz. [Azure AD tümleşik uygulamalarını kullanıcılara dağıtma](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**S: Azure AD, uygulamalar için kimlik doğrulaması ve çoklu oturum açmaya ne gibi farklı yollarla olanak tanır?**

**Y:** Azure AD; SAML 2.0, OpenID Connect, OAuth 2.0 ve WS-Federasyon gibi birçok standartlaştırılmış kimlik doğrulaması ve yetkilendirme protokolünü destekler. Azure AD aynı zamanda, yalnızca form tabanlı kimlik doğrulamasını destekleyen uygulamalar için parola kasası oluşturma ve otomatik oturum açma işlevlerini de destekler.  

Daha fazla bilgi için bkz.

* [Azure AD için Kimlik Doğrulama Senaryoları](active-directory-authentication-scenarios.md)
* [Active Directory kimlik doğrulama protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Çoklu oturum açma özelliği, Azure Active Directory ile nasıl kullanılır?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**S: Şirket içi olarak çalıştırdığım uygulamaları ekleyebilir miyim?**

**Y:** Azure AD Uygulama Ara Sunucusu, size seçtiğiniz şirket içi web uygulamaları için kolay ve güvenli erişim sağlar. Bu uygulamalara, Azure AD'de hizmet olarak yazılım (SaaS) uygulamalarınıza eriştiğiniz gibi erişebilirsiniz. VPN kullanmanız veya ağ altyapınızı değiştirmeniz gerekmez.  

Daha fazla bilgi için bkz. [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md).

- - -
**S: Belirli bir uygulamaya erişen kullanıcılar için çok faktörlü kimlik doğrulamasını nasıl isteyebilirim?**

**Y:** Azure AD koşullu erişimi ile her bir uygulama için benzersiz bir erişim ilkesi atayabilirsiniz. İlkenizde, çok faktörlü kimlik doğrulamasını her zaman veya kullanıcılar yerel ağa bağlı olmadığında gerekli kılabilirsiniz.  

Daha fazla bilgi için bkz. [Office 365'e ve Azure Active Directory'ye bağlı diğer uygulamalara erişimi güvence altına alma](active-directory-conditional-access.md).

- - -
**S: SaaS uygulamaları için otomatik kullanıcı hazırlama nedir?**

**Y:** Birçok popüler bulut SaaS uygulamasında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmek için Azure AD kullanın.

Daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatik hale getirme](active-directory-saas-app-provisioning.md).

- - -
**S:  Azure AD ile güvenli bir LDAP bağlantısı oluşturabilir miyim?**

**Y:** Hayır.  Azure AD, LDAP protokolünü desteklemez.



<!--HONumber=Feb17_HO2-->


