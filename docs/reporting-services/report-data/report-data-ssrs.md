---
title: Données des rapports (SSRS) | Microsoft Docs
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
ms.assetid: e22b7c24-edab-42d6-82f6-95068e1c6043
caps.latest.revision: 16
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 94028c673119c7a9af635c7bc9ccb37626eae86e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-data-ssrs"></a>Données des rapports (SSRS)
  Les données de rapport peuvent provenir de plusieurs sources de données de votre organisation. Votre première étape lors de la conception d'un rapport consiste à créer des sources de données et des datasets qui représentent les données de rapport sous-jacentes. Chaque source de données inclut des informations de connexion de données. Chaque dataset inclut une commande de requête qui définit le jeu de champs à utiliser comme données d'une source de données. Pour visualiser des données de chaque dataset, ajoutez une région de données, telle qu'une table, une matrice, un graphique ou une carte. Lorsque le rapport est traité, les requêtes s'exécutent sur la source de données, et chaque région de données s'étend autant que nécessaire pour afficher les résultats de la requête pour le dataset.  
  
##  <a name="BkMk_ReportDataTerms"></a> Termes  
  
-   **Connexion de données** Également appelée *Source de données*. Une connexion de données inclut un nom et des propriétés de connexion qui dépendent du type de connexion. Par défaut, une connexion de données n'inclut pas d'informations d'identification. Une connexion de données ne spécifie pas les données à récupérer à partir de la source de données externe. Pour ce faire, vous devez spécifier une requête lorsque vous créez un dataset.  
  
-   **Définition de source de données.** Un fichier qui contient la représentation XML d'une source de données de rapport. Lorsqu'un rapport est publié, ses sources de données sont enregistrées sur le serveur de rapports ou le site SharePoint en tant que définitions de source de données, indépendamment de la définition de rapport. Par exemple, un administrateur de serveur de rapports peut mettre à jour la chaîne de connexion ou les informations d'identification. Sur un serveur de rapports natif, le type de fichier est .rds. Sur un site SharePoint, le type de fichier est .rsds.  
  
-   **Chaîne de connexion.** Une chaîne de connexion est une version de chaîne des propriétés de connexion nécessaires à la connexion à une source de données. Les propriétés de connexion diffèrent selon le type de connexion de données. Pour obtenir des exemples, consultez [Data Connections, Data Sources, and Connection Strings in Report Builder](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34).  
  
-   **Source de données partagée.** Source de données disponible sur un serveur de rapports ou un site SharePoint, et qui est utilisable par plusieurs rapports.  
  
-   **Source de données incorporée.** Également appelée *source de données spécifique aux rapports*. Il s'agit d'une source de données définie dans un rapport et utilisée uniquement par ce dernier.  
  
-   **Informations d'identification.** Les informations d'identification sont des informations d'authentification qui doivent être fournies pour vous permettre d'accéder à des données externes.  
  
##  <a name="BkMk_ReportDataTips"></a> Conseils pour spécifier les données de rapport  
 Utilisez les informations suivantes pour concevoir votre stratégie de données de rapport.  
  
-   **Sources de données** Les sources de données peuvent être publiées et gérées indépendamment des rapports sur un serveur de rapports ou un site SharePoint. Pour chaque source de données, vous ou le propriétaire de la base de données pouvez gérer les informations de connexion dans un seul emplacement. Les informations d'identification de la source de données sont stockées de manière sécurisée sur le serveur de rapports ; n'incluez pas de mots de passe dans la chaîne de connexion. Vous pouvez rediriger une source de données d'un serveur de test vers un serveur de production. Vous pouvez désactiver une source de données pour interrompre tous les rapports qui l'utilisent.  
  
-   **Datasets** Les datasets peuvent être publiés et gérés indépendamment des rapports ou des sources de données partagées desquels ils dépendent. Vous ou le propriétaire de la base de données pouvez fournir des requêtes optimisées pour les auteurs de rapport à utiliser. Lorsque vous modifiez la requête, tous les rapports qui se servent des datasets partagés utilisent la requête mise à jour. Vous pouvez autoriser la mise en cache du dataset pour améliorer les performances. Vous pouvez planifier la mise en cache de requête à un moment donné ou utiliser une planification partagée.  
  
-   **Données utilisées par les parties de rapports** Les parties de rapport peuvent incluent les données dont elles dépendent. Pour plus d’informations sur les parties de rapport, consultez [Parties de rapport dans le Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
-   **Filtrer les données** Les données de rapport peuvent être filtrées dans la requête ou dans le rapport. Vous pouvez utiliser des datasets et interroger des variables pour créer des paramètres en cascade et fournir à l'utilisateur la possibilité de limiter les choix parmi des milliers de sélections à un nombre plus gérable. Vous pouvez filtrer les données dans une table ou un graphique en fonction des valeurs des paramètres ou d'autres valeurs que vous spécifiez.  
  
-   **Paramètres** Les commandes de requête de datasets qui incluent des variables de requêtes créent automatiquement les paramètres de rapport correspondants. Vous pouvez également créer des paramètres manuellement. Lorsque vous affichez un rapport, la barre d'outils Rapport affiche les paramètres. Les utilisateurs peuvent sélectionner des valeurs pour contrôler l'apparence des données de rapport ou du rapport. Pour personnaliser les données du rapport pour un public donné, vous pouvez créer des ensembles de paramètres de rapport avec différentes valeurs par défaut liées à la même définition de rapport ou utiliser le champ prédéfini **UserID** . Pour plus d’informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md) et [Collections intégrées dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
-   **Alertes de données** Après la publication d'un rapport, vous pouvez créer des alertes sur des données de rapport et recevoir des messages électroniques lorsqu'elle satisfait aux règles que vous spécifiez.  
  
-   **Groupe et données agrégées** Les données de rapport peuvent être regroupées et agrégées dans la requête ou dans le rapport. Si vous agrégez des valeurs dans la requête, vous pouvez continuer à combiner des valeurs dans le rapport dans des limites de ce qui est explicite.  Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) et [Fonction d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-function.md).  
  
-   **Trier des données** Les données de rapport peuvent être triées dans la requête ou dans le rapport. Dans les tables, vous pouvez également ajouter un bouton de tri interactif pour permettre à l'utilisateur de contrôler l'ordre de tri.  
  
-   **Données basées sur des expressions** Comme la plupart des propriétés de rapport peuvent être basées sur des expressions, et que les expressions peuvent inclure des références à des champs de dataset et à des paramètres de rapport, vous pouvez écrire des expressions puissantes pour contrôler les données et l’apparence du rapport. Vous pouvez fournir à l'utilisateur la possibilité de contrôler les données qu'il consulte en définissant des paramètres.  
  
-   **Afficher les données d'un dataset** Les données d'un dataset sont généralement affichées sur une ou plusieurs régions de données, par exemple, une table et un graphique.  
  
-   **Afficher les données de plusieurs datasets**  Vous pouvez écrire des expressions dans une région de données basée sur un dataset qui recherche des valeurs ou des agrégats dans d'autres datasets. Vous pouvez inclure des sous-rapports dans une table selon un dataset pour afficher les données d'une source de données différente.  
  
 Utilisez la liste suivante pour définir les sources de données d'un rapport.  
  
-   Envisagez d'utiliser, ou non, les sources de données et les datasets incorporés ou partagés. Collaborez avec les propriétaires des sources de données pour implémenter et utiliser l'authentification et la technologie d'autorisation appropriée pour votre organisation.  
  
-   Comprenez l'architecture de la couche de données logicielle de votre organisation et les problèmes potentiels résultant des types de données. Comprenez comment les extensions de données et les extensions pour le traitement des données peuvent affecter les résultats de la requête. Les types de données diffèrent entre la source de données, les fournisseurs de données et les types de données stockés dans le fichier de définition de rapport (.rdl).  
  
-   Comprenez les architectures et les outils client/serveur de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Par exemple, dans le Concepteur de rapports, créez des rapports sur un ordinateur client qui utilise les types de sources de données intégrés. Lorsque vous publiez un rapport, les types de source de données doivent être pris en charge sur le serveur de rapports ou sur le site SharePoint.  Pour plus d’informations, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
-   Les sources de données et les datasets sont créés dans un rapport et publiés sur un serveur de rapports ou un site SharePoint à partir d'un outil de création client. Les sources de données peuvent être créées directement sur le serveur de rapports. Une fois qu'ils sont publiés, vous pouvez configurer les informations d'identification et d'autres propriétés sur le serveur de rapports. Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) et [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md).  
  
-   Les sources de données que vous pouvez utiliser dépendent des extensions de données de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui sont installées. La prise en charge des sources de données peut être différente selon l'outil de création client, la version du serveur de rapports et la plateforme de serveur de rapports. Pour plus d’informations, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
-   Les informations d'identification de la source de données varient selon le type de source de données et selon que les rapports sont affichés, ou non, sur votre serveur client, sur votre serveur de rapports ou sur un site SharePoint. Pour plus d’informations, consultez [Définir les autorisations sur les éléments de serveur de rapports sur un site SharePoint &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md), [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) et les informations d’identification propres à chaque outil dans [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 Les tâches liées à la création des connexions de données, l'ajout de données provenant de sources externes, de datasets et de requêtes.  
  
|||  
|-|-|  
|**Tâches courantes**|**Liens**|  
|Créer des connexions de données|[Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|  
|Créer des datasets et des requêtes|[Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)|  
|Gérer les sources de données après leur publication|[Gérer des sources de données de rapports](../../reporting-services/report-data/manage-report-data-sources.md)|  
|Gérer des datasets partagés après leur publication|[Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md)|  
|Créer et gérer des alertes de données|[Alertes de données Reporting Services](../../reporting-services/reporting-services-data-alerts.md)|  
|Mettre en cache un dataset partagé|[Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)|  
|Planifier un dataset partagé pour précharger le cache|[Planifications](../../reporting-services/subscriptions/schedules.md)|  
|Ajouter une extension de données|[Implémentation d'une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
