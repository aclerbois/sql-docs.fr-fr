---
title: Gérer des sources de données de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], data
- published reports [Reporting Services], data source connections
- shared data sources [Reporting Services]
- data sources [Reporting Services], managing
ms.assetid: 0475aded-c8fe-4337-a2b5-4df0ec4c46af
caps.latest.revision: 52
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 80ae6fcf181e3fe48a4be6c9d29b3637e8e70bd3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-report-data-sources"></a>Gérer des sources de données de rapports
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], les rapports, les modèles de rapports et les abonnements pilotés par les données récupèrent les données qui proviennent de sources de données externes. Pour se connecter à une source de données externe, un serveur de rapports utilise les informations de connexion de la source de données qui sont définies dans le rapport, le modèle ou l'abonnement, ou qui sont référencées à partir de ceux-ci. Les propriétés de connexion à la source de données sont toujours définies avec le rapport ou le modèle que vous créez, mais vous pouvez les gérer de manière indépendante après avoir publié le rapport ou le modèle sur un serveur de rapports.  
  
 Pour gérer les sources de données de rapport, vous pouvez utiliser le Gestionnaire de rapports pour un serveur de rapports en mode natif ou des pages d'application sur un site SharePoint si vous avez déployé le serveur de rapports en mode intégré SharePoint.  
  
 La gestion des connexions à la source de données est caractérisée par les tâches suivantes, décrites dans cette rubrique :  
  
-   modifier les chaînes de connexion ;  
  
-   modifier les informations d'identification ;  
  
-   créer et utiliser des sources de données partagées sur un serveur de rapports, notamment passer d'une source de données incorporée à une source de données partagée ;  
  
-   contrôler l'accès aux propriétés de la source de données en définissant des autorisations sur le rapport, le modèle, ou une source de données partagée que vous utilisez.  
  
 Remarquez que la modification des requêtes ne fait pas partie de la gestion de la connexion à la source de données. Pour modifier une requête pour un rapport ou un modèle, vous devez utiliser un outil de création et apporter vos modifications dans la définition du rapport ou du modèle.  
  
## <a name="managed-properties-data-source-type-connection-strings-and-credentials"></a>Propriétés gérées : type de source de données, chaînes de connexion et informations d'identification  
 Les propriétés de la source de données que vous pouvez gérer sur un serveur de rapports sont les suivantes :  
  
|Propriété|Description|Comment gérer|  
|--------------|-----------------|----------------------|  
|Type de source de données|Détermine quelle extension utiliser sur les données externes pour le traitement de données sur le serveur de rapports. Les exemples de processeurs de données incluent SQL Server, Analysis Services et Oracle.|Le type de source de données est une propriété managée parce qu'il est configurable. Toutefois, vous devez configurer uniquement un type de source de données si vous créez une nouvelle source de données partagée.<br /><br /> Ne modifiez pas le type de source de données dans les pages de propriétés d'un rapport ou modèle publié, car vous risquez certainement d'invalider la connexion. Il est peu probable que les structures de données requises par un rapport ou modèle soient identiques sur une plateforme de données différente.|  
|Chaîne de connexion|Établit la connexion initiale à une source de données externe. Un rapport peut utiliser des chaînes de connexion statiques ou dynamiques.<br /><br /> Une *chaîne de connexion statique* est un ensemble de valeurs que le rapport utilise toujours pour se connecter à la même source de données à chaque exécution du rapport.<br /><br /> Une *chaîne de connexion dynamique* est une expression que vous intégrez au rapport et qui permet à l’utilisateur de choisir quelle source de données utiliser au moment de l’exécution. Vous devez inclure la liste de la sélection de source de données et l'expression dans le rapport que vous créez dans le Concepteur de rapports.|La modification d'une chaîne de connexion est utile si vous déplacez une source de données vers un autre ordinateur, ou si vous avez créé des rapports à l'aide de données de test mais que vous souhaitez déployer les rapports avec une base de données de production.<br /><br /> Vous pouvez gérer une chaîne de connexion statique en remplaçant la chaîne d'origine par une autre chaîne.<br /><br /> Pour gérer une chaîne de connexion dynamique dans le Gestionnaire de rapports ou sur un site SharePoint, vous devez vous contenter de la remplacer par une chaîne statique. Vous ne pouvez pas modifier l'expression elle-même, ni modifier la liste de la sélection de source de données. Pour modifier l'expression ou la liste des valeurs valides, vous devez modifier la définition du rapport et la republier sur le serveur de rapports. Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).|  
|Informations d'identification|Fournit le nom et le mot de passe d'un utilisateur autorisé à lire des données dans la source de données.<br /><br /> Si une source de données ne prend pas en charge l'authentification (par exemple, si la source de données est un fichier XML dans le système de fichiers), vous pouvez configurer le compte d'exécution sans assistance pour permettre au serveur de rapports de se connecter à la source de données externe sans passer d'informations d'identification.|Vous pouvez gérer des informations d'identification en mettant à jour le compte d'utilisateur ou un mot de passe s'il a expiré.<br /><br /> Vous pouvez également modifier la façon dont les informations d'identification sont obtenues (par exemple, en invitant les utilisateurs à entrer des informations d'identification au moment de l'exécution).<br /><br /> Si vous souhaitez que les utilisateurs puissent s'abonner à un rapport, vous devez configurer le rapport pour utiliser des informations d'identification stockées.|  
  
## <a name="creating-and-using-shared-data-sources"></a>Création et utilisation de sources de données partagées  
 Si vous publiez un rapport avec des propriétés de source de données incorporées dans le rapport, vous pouvez envisager de passer à des propriétés de source de données partagée. Les sources de données partagées sont plus faciles à gérer parce que vous pouvez mettre à jour les informations d'identification et les chaînes de connexion sur une page. Tous les rapports, modèles et abonnements pilotés par des données qui font appel à cette source de données peuvent intégrer immédiatement les modifications. Vous pouvez également mettre une source de données partagée hors connexion et suspendre le rapport ou l'abonnement pour empêcher son exécution pendant que vous dépannez ou analysez un problème étant survenu.  
  
## <a name="controlling-access-data-source-properties"></a>Contrôle de l'accès aux propriétés de la source de données  
 Par défaut, un utilisateur autorisé à gérer des rapports peut définir n'importe quelle propriété sur le rapport, notamment les propriétés qui déterminent le type de la source de données, la chaîne de connexion, les informations d'identification, et si le rapport reçoit des informations de connexion d'une source de données partagée ou incorporée. Pour plus d’informations sur les tâches et autorisations qui contrôlent l’accès aux propriétés de la source des données sur un serveur de rapports en mode natif, consultez [Sécuriser les éléments de source de données partagée](../../reporting-services/security/secure-shared-data-source-items.md) et [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).  
  
 Les autorisations d'afficher et de modifier les propriétés des éléments dans une bibliothèque SharePoint sont déterminées par l'administrateur de site. Pour plus d’informations sur les autorisations qui contrôlent l’accès aux propriétés de connexion à la source des données, consultez [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
## <a name="how-to-work-with-data-source-properties-on-a-report-server"></a>Comment utiliser les propriétés de la source de données sur un serveur de rapports  
 Vous pouvez utiliser divers outils pour créer et modifier des propriétés de la source de données. Le tableau suivant résume les approches et les outils, et fournit un lien vers des instructions supplémentaires.  
  
|Tâche|Outil|Lien|  
|----------|----------|----------|  
|Afficher des exemples de chaînes de connexion.||[Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|  
|Choisir une stratégie pour obtenir des informations d'identification pour se connecter à une source de données.||[Spécifier des informations d'identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)|  
|Ajouter des propriétés de connexion à la source de données à un fichier de définition de rapport (.rdl).|Concepteur de rapports|[Créer une source de données incorporée ou partagée &#40;SSRS&#41;](http://msdn.microsoft.com/library/b111a8d0-a60d-4c8b-b00a-51644b19c34b)|  
|Ajouter et créer un lien vers un fichier de source de données partagée (.rds) dans un projet de rapport.|Concepteur de rapports|[Créer, modifier et supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)|  
|Créer une liste prédéfinie des sources de données que les utilisateurs peuvent sélectionner au moment de l'exécution. Lorsqu'un utilisateur demande un rapport, celui-ci fournit une liste des sources de données. L'utilisateur doit sélectionner quelle source de données utiliser avant d'exécuter le rapport. Pour ajouter une liste de sélection de la source de données à un rapport, utilisez une expression.<br /><br /> Il s'agit d'une connexion dynamique à la source de données.|Concepteur de rapports|[Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|  
|Créer un élément de source de données partagée sur un serveur de rapports.|Gestionnaire de rapports|[Créer, supprimer ou modifier une source de données partagée &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/cd7bace3-f8ec-4ee3-8a9f-2f217cdca9f2)|  
|Stocker des informations d'identification comme condition préalable à la création des abonnements ou des instantanés de rapport.|Gestionnaire de rapports|[Store Credentials in a Reporting Services Data Source](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)|  
|Modifier les propriétés de connexion à la source de données sur un rapport publié.|Gestionnaire de rapports|[Configurer les propriétés de la source de données d’un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)|  
|Créer un élément de source de données partagée sur un serveur de rapports.|Site SharePoint|[Créer et gérer des sources de données partagées &#40;Reporting Services en mode intégré SharePoint&#41;](http://msdn.microsoft.com/library/2d3428e4-a810-4e66-a287-ff18e57fad76)|  
|Utiliser des informations de connexion .odc existantes avec un rapport.|Site SharePoint|[Utiliser une connexion de données Office &#40;.odc&#41; avec les rapports &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-data/use-an-office-data-connection-odc-with-reports.md)|  
  
> [!NOTE]  
>  La gestion des connexions aux sources de données d'un rapport diffère de la gestion de la connexion d'un serveur de rapports à sa base de données. Pour plus d’informations sur la connexion d’un serveur de rapports à sa banque de données interne, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Lier un rapport ou un modèle à une source de données partagée &#40;SSRS&#41;](../../reporting-services/report-data/bind-a-report-or-model-to-a-shared-data-source-ssrs.md)   
 [Créer, supprimer ou modifier une source de données partagée &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/cd7bace3-f8ec-4ee3-8a9f-2f217cdca9f2)   
 [Stocker des informations d’identification dans une source de données Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)  
  
  
