---
title: Mes paramètres pour l’intégration de Power BI (portail web) | Microsoft Docs
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- pbi
- power bi
- power bi integration
ms.assetid: 85c2fac7-80bf-45b7-8654-764b5f5231f5
caps.latest.revision: 15
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7d878ea994865096aac36397e9c32c8cf55b359c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="my-settings-for-power-bi-integration-web-portal"></a>Mes paramètres pour l’intégration de Power BI (portail web)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)])

Grâce à la page **Mes paramètres** du [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] , les utilisateurs individuels peuvent gérer leur connexion à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Lorsque vous parcourez les étapes permettant d’épingler un élément de rapport, vous serez automatiquement invité à vous connecter.  Toutefois, vous pouvez utiliser la page **Mes paramètres** pour vous connecter manuellement ou vous déconnecter.  Si l’option de menu **Mes paramètres** n’est pas visible, cela signifie que le serveur de rapports n’est pas intégré à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)].  Pour plus d’informations, consultez [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md).  
  
![ssRS_WebPortal_MySettings](../reporting-services/media/ssrs-webportal-mysettings.png)  
  
## <a name="why-sign-in"></a>Pourquoi se connecter ?

 Lorsque vous vous connectez, vous établissez une relation entre votre compte d’utilisateur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et votre compte [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .  La connexion crée un jeton de sécurité, qui est valable pendant 90 jours. Si le jeton expire et que des éléments sont épinglés à Power BI, une notification apparaît.  
   
 ![ssRS_WebPortal_PowerBI_Notification](../reporting-services/media/ssrs-webportal-powerbi-notification.png)    
   
Pour que les vignettes des tableaux de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] soient actualisées, vous devez vous reconnecter via **Mes paramètres**.  
  
![ssRS_WebPortal_PowerBI_SignIn_Again](../reporting-services/media/ssrs-webportal-powerbi-signin-again.png)  
  
Une fois que vous êtes connecté, un jeton de sécurité est créé.  La mise à jour des vignettes de votre tableau de bord se lance sur leurs calendriers configurés précédemment.  

## <a name="next-steps"></a>Étapes suivantes

[Intégration de Power BI Report Server](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md)   
[Épingler des éléments Reporting Services aux tableaux de bord Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)   
[Tableaux de bord dans Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)  
[Portail Web](../reporting-services/web-portal-ssrs-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
