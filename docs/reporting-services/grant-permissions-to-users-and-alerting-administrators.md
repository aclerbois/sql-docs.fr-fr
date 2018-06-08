---
title: Accorder des autorisations aux utilisateurs et alerter les administrateurs | Microsoft Docs
ms.custom: ''
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 166808e1-ada7-48d2-bda8-8f7c017fb3aa
caps.latest.revision: 11
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 136406ea820f89ff52df5921511227af80178054
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="grant-permissions-to-users-and-alerting-administrators"></a>Accorder des autorisations aux utilisateurs et alerter les administrateurs

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)])

Avant que les utilisateurs et les administrateurs d'alerte puissent créer, modifier, supprimer et afficher les alertes de données d'affichage, ils doivent disposer des autorisations SharePoint. La fonctionnalité d’alerte de données de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] ne nécessite aucune autorisation particulière. Utilisez celles qui sont intégrées à SharePoint.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

**Travailleurs de l'information**— Les autorisations doivent inclure les autorisations SharePoint Créer une alerte et Afficher les éléments. Les niveaux d'autorisation prédéfinis SharePoint nommés Concevoir, Contribuer, Lire et Vue uniquement, incluent les autorisations SharePoint Créer une alerte et Afficher les éléments. Vous pouvez également créer un niveau d'autorisation personnalisé avec les autorisations requises pour prendre en charge les utilisateurs qui créent, modifient, exécutent, et affichent les alertes de données.

**Alerte des administrateurs**— Les autorisations doivent inclure l'autorisation SharePoint Gérer les alertes. Par défaut, seul le niveau d'autorisation Contrôle total comprend cette autorisation pour les sites créés avec le modèle de site Team Site. Si vous utilisez d'autres modèles de site, vous verrez des listes différentes de groupes SharePoint par défaut. Vous pouvez ajouter des alertes de gestion à l'un des niveaux d'autorisation prédéfinis ou créer un niveau d'autorisation personnalisé avec l'autorisation requise pour prendre en charge l'alerte des administrateurs qui affichent et suppriment des alertes de données.

Pour en savoir plus sur les autorisations SharePoint, consultez [Autorisations utilisateur et niveaux d’autorisation (SharePoint Server 2010)](http://technet.microsoft.com/library/cc721640.aspx).

## <a name="grant-permissions"></a>Accorder des autorisations
  
1.  Accédez au site SharePoint auquel vous souhaitez accorder des autorisations.  
  
2.  Dans la barre d'outils, cliquez sur **Actions du site** puis cliquez sur **Autorisations du site**.  
  
     Si vous ne voyez pas cette option, vous ne disposez pas de l'autorisation suffisante pour accorder des autorisations à d'autres.  
  
3.  Cliquez sur **Accorder des autorisations**.  
  
4.  Dans **Utilisateurs/groupes**, tapez les noms d’utilisateurs, les noms de groupes ou les adresses e-mail auxquels vous souhaitez accorder l’autorisation.  
  
5.  Sélectionnez l'option **Ajouter des utilisateurs à un groupe SharePoint** ou **Accorder directement les autorisations aux utilisateurs** . Selon que vous avez sélectionné **Ajouter des utilisateurs à un groupe SharePoint** ou **Accorder directement les autorisations aux utilisateurs** , effectuez l'une des opérations suivantes :  
  
    -   Si vous avez sélectionné **Ajouter des utilisateurs à un groupe SharePoint**, sélectionnez un niveau d’autorisation dans la liste déroulante.  
  
    -   Si vous avez sélectionné **Accorder directement les autorisations aux utilisateurs**, sélectionnez un niveau d'autorisation.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  

## <a name="see-also"></a> Voir aussi

[Définir les autorisations sur les éléments de serveur de rapports sur un site SharePoint &#40;Reporting Services en mode intégré SharePoint&#41;](../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md)   
[Alertes de données Reporting Services](../reporting-services/reporting-services-data-alerts.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
