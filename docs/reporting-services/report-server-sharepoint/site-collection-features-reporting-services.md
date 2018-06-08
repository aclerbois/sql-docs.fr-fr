---
title: Fonctionnalités de collection de sites de Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 100ec0e739dc397b8173259abcfaf7c8e6cdfd9e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-site-collection-features"></a>Fonctionnalités de collection de sites de Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Le mode SharePoint de Reporting Services fournit trois fonctionnalités de collection de sites SharePoint. Ces fonctionnalités prennent en charge l’environnement général de création de rapports du mode SharePoint de Reporting Services, [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)], une fonctionnalité du complément SQL Server 2016 Reporting Services et les opérations de gestion de Reporting Services dans l’Administration centrale de SharePoint.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.
  
## <a name="site-collection-features"></a>Fonctionnalités de collection de sites

 Le tableau ci-dessous décrit les fonctionnalités de collection de sites de Reporting Services.  
  
|Fonctionnalité|Description|  
|-------------|-----------------|  
|**Fonctionnalité Administration centrale du serveur de rapports**|Active les fonctionnalités de gestion de l’intégration avec un serveur de rapports Reporting Services. Cette fonctionnalité est uniquement installée et utilisée dans la collection de sites Administration centrale de SharePoint.<br /><br /> La fonctionnalité d’intégration du serveur de rapports est activée automatiquement dans la collection de sites Administration centrale de SharePoint après l’installation du complément [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] pour les produits SharePoint. Dans certaines circonstances, vous devrez activer cette fonctionnalité manuellement. Pour activer la fonctionnalité de serveur de rapports, utilisez les pages Reporting Services de la page Paramètres du site de l’Administration centrale de SharePoint.<br /><br /> La version [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Reporting Services et les versions ultérieures du complément pour les produits SharePoint activent la fonctionnalité d’intégration du serveur de rapports pour toutes les collections de sites existantes pendant l’installation du complément. En outre, la fonctionnalité est automatiquement active pour les nouvelles collections de sites.|  
|**Fonctionnalité d'intégration du serveur de rapports**|Permet la création de rapports détaillés au moyen de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Reporting Services.<br /><br /> Cette fonctionnalité est activée par défaut.|  
|**Fonctionnalité d'intégration de Power View**|Active l’exploration interactive de données et la présentation visuelle dans les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et les bases de données tabulaires Analysis Services.<br /><br /> Cette fonctionnalité est accessible via les menus contextuels des sources de données suivantes :<br /><br /> **.rdlx**<br /><br /> **.rsds**<br /><br /> fichier de connexion **.bism** <br /><br /> <br /><br /> Si [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] n'apparaît pas dans les menus contextuels, vérifiez que **Fonctionnalité d'intégration de Power View** est activé.<br /><br /> Cette fonctionnalité est désactivée par défaut.|  

## <a name="next-steps"></a>Étapes suivantes

[Activer les fonctionnalités d'intégration Report Server et Power View dans SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)   
[Paramètres et fonctions du site Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/site-settings-and-features-reporting-services.md)   
[Activer la fonctionnalité Synchronisation de fichiers de serveur de rapports dans l'Administration centrale de SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
