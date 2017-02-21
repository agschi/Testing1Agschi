---
title: "在 Azure App Service 中创建 Java Web 应用 | Microsoft Docs"
description: "本教程演示了如何将 Java Web 应用部署到 Azure App Service。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6e73cc3-8b71-4742-a197-3edeabc6a289
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 3451e6d13119bacc66e9ccd861862edea5a5b4fe


---
# <a name="create-a-java-web-app-in-azure-app-service"></a>在 Azure App Service 中创建 Java Web 应用
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

本教程演示如何通过使用 [Azure 门户]  [在 Azure App Service 中创建 Java Web 应用]。 Azure 门户是可用于管理 Azure 资源的 Web 界面。

> [!NOTE]
> 若要完成本教程，您需要一个 Microsoft Azure 帐户。 如果没有帐户，可以[激活 Visual Studio 订户权益]，或者[注册免费试用帐户]。
> 
> 如果要在注册 Azure 帐户之前就开始使用 Azure 应用服务，请转到 [试用应用服务]。 在那里，可以立即在应用服务中创建短期的入门级 Web 应用；无需信用卡，也无需做出承诺。
> 
> 

## <a name="java-application-options"></a>Java 应用程序选项
在 App Service Web 应用中设置 Java 应用程序可以有多种方法。 

1. 创建应用，然后配置 **应用程序设置**。
   
    应用服务提供多个使用默认配置的 Tomcat 和 Jetty 版本。 如果要托管的应用程序可以使用某个内置版本，则这种设置 Web 容器的方法是最简单的，如果你只想将 war 文件上传至 Web 容器，这种方法则是最适合的。 使用这种方法时，先在 Azure 门户中创建一个应用，然后转到应用的“应用程序设置”  边栏选项卡，选择 Java 版本以及所需的 Java Web 容器。 使用此方法时，Java 和 Web 容器均从“程序文件”运行。 其他方法会将 Web 容器和 JVM（可能）放在磁盘空间中。 使用此模型时，用户不具有编辑文件系统中此部分文件的权限。 这意味着无法执行某些操作，如配置 *server.xml* 文件或将库文件放入 */lib* 文件夹。 有关详细信息，请参阅本教程后面的 [创建和配置 Java Web 应用](#portal) 部分。
2. 使用 Azure 应用商店中的模板。
   
    Azure 应用商店包括可使用 Tomcat 或 Jetty web 容器自动创建和配置 Java Web 应用的模板。 这些模板创建的 Web 容器可进行配置。 有关详细信息，请参阅本教程中的 [使用 Azure 应用商店的 Java 模板](#marketplace) 部分。
3. 创建一个应用，然后手动复制和编辑配置文件 
   
    你可能需要托管一个自定义 Java 应用程序，该程序不部署在 App Service 所提供的任何 Web 容器中。 例如：
   
   * 你的 Java 应用程序需要的 Tomcat 或 Jetty 版本不受 App Service 的直接支持，或者库中不提供该版本。
   * 你的 Java 应用程序接受 HTTP 请求，不以 WAR 的形式部署到预先存在的 Web 容器中。
   * 你想要从头开始自行配置 Web 容器。 
   * 你想要使用的 Java 版本在 App Service 中不受支持，因此想要自行上载它。
     
     对于这样的情况，你可以通过 Azure 门户创建一个应用，然后手动提供相应的运行时文件。 在本示例中，这些文件将占用你的应用服务计划的存储空间配额。 有关详细信息，请参阅 [将自定义 Java Web 应用上载到 Azure]。

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a> 创建和配置 Java Web 应用
本部分演示如何使用门户的“应用程序设置”  边栏选项卡创建 Web 应用并将其配置为适用于 Java。

1. 登录到 [Azure 门户]。
2. 单击“新建”>“Web + 移动”>“Web 应用”。
   
    ![新的 Web 应用][newwebapp]
3. 在“Web 应用”框中输入 Web 应用的名称。
   
    该名称在 azurewebsites.net 域中必须是唯一的，因为 Web 应用的 URL 将是 {name}.azurewebsites.net。 如果你输入的名称不是唯一的，则会在文本框中显示一个红色的感叹号。
4. 选择“资源组”或新建一个。
   
    有关资源组的详细信息，请参阅 [Resource Manager 概述]。
5. 选择“应用服务计划/位置”或新建一个。
   
    有关 App Service 计划的详细信息，请参阅 [Azure App Service 计划概述]。
6. 单击“创建” 。
   
    ![创建 Web 应用][newwebapp2]
7. 创建 Web 应用后，单击“Web 应用”>“{你的 Web 应用}”。
   
    ![选择 Web 应用][selectwebapp]
8. 在“Web 应用”边栏选项卡中，单击“设置”。
9. 单击“应用程序设置” 。
10. 选择所需的 **Java 版本**。 
11. 选择所需的 **Java 次要版本**。 如果选择“最新”，应用将使用该 Java 主要版本的应用服务提供的最新次要版本。 **最新**项对于从“应用程序设置”创建的 Java 应用唯一。 若从库中创建 Java 应用，则必须管理自己的容器和 JVM 更改。 
12. 选择所需的 **Web 容器**。 若选择的容器名称以 **最新**开头，应用将保留为应用服务中提供的该 Web 容器主要版本的最新版本。 
    
    ![Web 容器版本][versions]
13. 单击“保存” 。
    
    你的 Web 应用将在几分钟之内变成基于 Java 的 Web 应用，并配置为使用所选的 Web 容器。
14. 单击“Web 应用”>“{你的新 Web 应用}”。
15. 单击 **URL** 浏览到新站点。
    
    该网页确认你已创建基于 Java 的 Web 应用。

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a> 使用 Azure 应用商店中的 Java 模板
本部分介绍如何使用 Azure 应用商店创建 Java Web 应用。 相同的常规流还可用于创建基于 Java 的移动应用或 API 应用。 

1. 登录到 [Azure 门户]
2. 单击“新建”>“应用商店”。
   
    ![新的应用商店][newmarketplace]
3. 单击“Web + 移动” 。
   
    可能需要向左滚动才能看到“应用商店”边栏选项卡，可在其中选择“Web + 移动”。
4. 在搜索文本框中，输入 Java 应用服务器的名称，如 **Apache Tomcat** 或 **Jetty**，然后按 Enter。
5. 在搜索结果中，单击 Java 应用程序服务器。
   
    ![Web 移动 Jetty][webmobilejetty]
6. 在第一个 **Apache Tomcat** 或 **Jetty** 边栏选项卡中，单击“创建”。
   
    ![Jetty 门户边栏选项卡][jettyblade]
7. 在下一个 **Apache Tomcat** 或 **Jetty** 边栏选项卡中，在“Web 应用”框中输入 Web 应用的名称。
   
    该名称在 azurewebsites.net 域中必须是唯一的，因为 Web 应用的 URL 将是 {name}.azurewebsites.net。 如果你输入的名称不是唯一的，则会在文本框中显示一个红色的感叹号。
8. 选择“资源组”或新建一个。
   
    有关资源组的详细信息，请参阅 [Resource Manager 概述]。
9. 选择“应用服务计划/位置”或新建一个。
   
    有关 App Service 计划的详细信息，请参阅 [Azure App Service 计划概述]。
10. 单击“创建” 。
    
    ![Jetty 门户创建][jettyportalcreate2]
    
    不久之后（通常不到一分钟），Azure 将创建出新的 Web 应用。
11. 单击“Web 应用”>“{你的新 Web 应用}”。
12. 单击 **URL** 浏览到新站点。
    
    ![Jetty URL][jettyurl]
    
    Tomcat 附带了一组默认的页面；如果选择 Tomcat，就会看到类似于以下示例的页面。
    
    ![使用 Apache Tomcat 的 Web 应用][tomcat]
    
    如果选择 Jetty，会看到类似于以下示例的页面。 Jetty 没有默认页面集，因此在此处重新使用用于空 Java 网站的 JSP。
    
    ![使用 Jetty 的 Web 应用][jetty]

现在你已经使用应用容器创建了 Web 应用，请参阅 [后续步骤](#next-steps) 部分，了解有关如何将应用程序上传到该 Web 应用的信息。

## <a name="next-steps"></a>后续步骤
现在，你有了一台在 Azure App Service 的 Web 应用中运行的 Java 应用程序服务器。 若要将你自己的代码部署到 Web 应用，请参阅 [将应用程序或网页添加到 Java Web 应用]。

有关如何在 Azure 中开发 Java 应用程序的详细信息，请参阅 [Java 开发中心]。

<!-- URL List -->

[将应用程序或网页添加到 Java Web 应用]: ./web-sites-java-add-app.md
[Azure App Service 计划概述]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure 门户]: https://portal.azure.com/
[激活 Visual Studio 订户权益]: http://go.microsoft.com/fwlink/?LinkId=623901
[注册免费试用帐户]: http://go.microsoft.com/fwlink/?LinkId=623901
[试用应用服务]: https://azure.microsoft.com/try/app-service/
[在 Azure App Service 中创建 Java Web 应用]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java 开发中心]: /develop/java/
[Resource Manager 概述]: ../azure-resource-manager/resource-group-overview.md
[将自定义 Java Web 应用上载到 Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png



<!--HONumber=Feb17_HO3-->


