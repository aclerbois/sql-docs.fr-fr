---
title: Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- permissions [Reporting Services], SharePoint integrated mode
- SharePoint integration [Reporting Services], permissions
- security [Reporting Services], SharePoint integrated mode
ms.assetid: 9577e88d-c22b-4934-936f-e0f1400cedf5
caps.latest.revision: 14
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5155c5689a4c7a51f2d392e8560a2c87dbf44fdd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-built-in-security-in-windows-sharepoint-services-for-report-server-items"></a>Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports
  SharePoint fournit des fonctionnalités de sécurité intégrée qui vous permettent d'accéder aux éléments d'un serveur de rapports à partir de sites et de bibliothèques SharePoint. Si vous avez déjà affecté des autorisations de site et de listes aux utilisateurs, ces derniers auront accès aux éléments et opérations de serveur de rapports dès que vous aurez configuré les paramètres d'intégration entre SharePoint et un serveur de rapports.  
  
## <a name="securable-items"></a>Éléments sécurisables  
 Les autorisations définies sur le site ou la bibliothèque permettent d'accorder l'accès aux éléments d'un serveur de rapports. Toutefois, si vous souhaitez sécuriser des éléments individuels, vous pouvez définir des autorisations sur les types de contenu suivants :  
  
|Type de fichier|Description|  
|---------------|-----------------|  
|.rdl|Fichier de définition de rapport qui définit la mise en page d'un rapport et les commandes de récupération des données. Une définition de rapport utilise les informations de connexion de la source de données pour récupérer des données lors du traitement du rapport. Si la définition de rapport correspond à un rapport ad hoc créé dans le Générateur de rapports, ce rapport est accompagné d'un fichier de modèle de rapport (.smdl) qui définit l'étendue de l'exploration des données dans le rapport rendu.|  
|.smdl|Fichier de modèle de rapport qui décrit les structures de données et leurs relations. Il sert à créer et exécuter les rapports du Générateur de rapports.|  
|.rsds|Fichier de source de données partagée qui spécifie les informations de connexion à une source de données externe. Il est utilisé par les fichiers de définition de rapport (.rdl) et les fichiers de modèle de rapport (.smdl). Les modèles de rapports utilisent toujours des fichiers .rsds pour obtenir les informations de connexion d'une source de données sous-jacente. Les définitions de rapports peuvent utiliser des fichiers .rsds ou des informations de connexion définies dans les propriétés de la source de données du rapport.|  
|.rsc|Fichier de partie de rapport qui définit la disposition et la structure d'un élément de rapport ou d'une région de données. Il permet de publier la partie de rapport sur un serveur afin que l'élément puisse être réutilisé par d'autres auteurs de rapports de la Bibliothèque de parties de rapports.|  
|.rsd|Fichier de dataset partagé qui définit la syntaxe et les propriétés de requête pour un dataset. Les datasets partagés peuvent être partagés, stockés, traités et mis en cache à l'extérieur d'un rapport.|  
  
 Les planifications, les abonnements et l'historique de rapport ne sont pas des éléments sécurisables. Vous pouvez définir des autorisations sur le site ou la bibliothèque afin de déterminer si un utilisateur peut créer ou utiliser les planifications, les abonnements et l'historique de rapport ; toutefois, vous ne pouvez pas sécuriser ces éléments directement.  
  
 Pour sécuriser des éléments individuels, sélectionnez l’élément dans la bibliothèque, cliquez sur la flèche orientée vers le bas, puis sélectionnez **Gérer les autorisations**. Dans le menu **Actions** , sélectionnez **Modifier les autorisations**.  
  
## <a name="using-built-in-groups-and-permission-levels-to-access-report-server-items"></a>Utilisation de groupes et de niveaux d'autorisation prédéfinis pour accéder aux éléments d'un serveur de rapports  
 Lorsque vous utilisez l'héritage des autorisations et les groupes SharePoint standard, vous pouvez accéder à la plupart des opérations de serveur de rapports immédiatement après avoir configuré les paramètres d'intégration du serveur de rapports et des instances SharePoint.  
  
 SharePoint fournit des groupes standard associés à des niveaux d'autorisation prédéfinis qui déterminent votre mode d'accès aux documents et pages d'un site SharePoint. Si vous utilisez des groupes standard et les niveaux d’autorisation par défaut, et si vos sites sont configurés pour l’héritage des autorisations, vous pouvez utiliser les fonctionnalités de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comme suit :  
  
|**Groupes SharePoint**|**Niveau d'autorisation**|**Résumé**|**Accès Report Server**|  
|---------------------------|--------------------------|-----------------|------------------------------|  
|**Propriétaires**|Contrôle total|Les propriétaires disposent de toutes les autorisations nécessaires pour créer, gérer et sécuriser les éléments et opérations d'un serveur de rapports.|Définissez des autorisations de contrôle d'accès à l'ensemble des éléments de serveur de rapports stockés dans toutes les bibliothèques du site. Définissez des autorisations dans un modèle de rapport (également appelé sécurité de l'élément de modèle). Personnalisez un composant WebPart Visionneuse de rapports. Ajoutez des rapports et autres éléments à des bibliothèques. Modifiez les propriétés d'éléments des rapports et autres documents. Supprimez les rapports et autres éléments. Affichez les rapports, y compris ceux qui reposent sur des modèles de rapports pour l'exploration de données. Définissez les paramètres des rapports. Définissez les options de traitement d'un rapport. Générez des modèles de rapports. Créez des rapports dans le Générateur de rapports. Créez et gérez des sources de données partagées. Créez, modifiez et supprimez les abonnements détenus par un utilisateur. Créez et gérez les planifications partagées utilisées dans l'ensemble du site. Créez et gérez les versions d'un document, y compris l'historique de rapport. Téléchargez le fichier source d'une définition de rapport ou d'un modèle de rapport. Remplacez une définition de rapport, un modèle de rapport, une source de données partagée ou une ressource (en conservant les autorisations et les propriétés d'éléments).|  
|**Membres**|Collaboration|Les membres peuvent créer des éléments, publier les rapports et les modèles correspondants à partir d'outils de conception dans une bibliothèque SharePoint.|Ajoutez des rapports et autres éléments à des bibliothèques. Modifiez les propriétés d'éléments des rapports et autres documents. Supprimez les rapports et autres éléments. Affichez les rapports, y compris ceux qui reposent sur des modèles de rapports pour l'exploration de données. Affichez les versions antérieures d'un document, y compris les instantanés d'historique de rapport (l'utilisateur doit disposer également de l'autorisation nécessaire pour ouvrir le rapport dont l'historique de rapport a été créé). Définissez les paramètres des rapports. Définissez les options de traitement d'un rapport. Générez des modèles de rapports. Créez des rapports dans le Générateur de rapports. Créez et gérez des sources de données partagées. Créez, modifiez et supprimez les abonnements détenus par l'utilisateur. Utilisez des planifications partagées avec un abonnement. Créez et gérez les versions d'un document, y compris l'historique de rapport. Téléchargez le fichier source d'une définition de rapport ou d'un modèle de rapport. Remplacez une définition de rapport, un modèle de rapport, une source de données partagée ou une ressource (en conservant les autorisations et les propriétés d'éléments).|  
|**Visiteurs** et **visionneuses**|Lire|Les visiteurs peuvent afficher les rapports|Affichez les rapports, y compris ceux qui reposent sur des modèles de rapports pour l'exploration de données.|  
  
 Si vous n'utilisez pas les groupes et les niveaux d'autorisation prédéfinis, vous devez inclure des autorisations spécifiques pour l'accès aux fonctionnalités de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Définir des autorisations pour les opérations de serveur de rapports dans une application web SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)   
 [Comparer des rôles et des tâches dans Reporting Services avec les autorisations et les groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)   
 [Définir des autorisations pour des opérations de serveurs de rapports dans une application web SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)  
  
  
