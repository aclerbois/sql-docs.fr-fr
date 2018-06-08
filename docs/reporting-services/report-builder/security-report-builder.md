---
title: Sécurité (Générateur de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ed38291a-6afe-449f-9f32-3ae04502bd6f
caps.latest.revision: 11
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 2ee5186a812aac4cec049c7624bbb514d6d42914
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="security-report-builder"></a>Sécurité (Générateur de rapports)
  Le Générateur de rapports est une application cliente de création de rapports conçue pour être utilisée conjointement avec un serveur de rapports [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Le serveur de rapports peut être configuré pour être utilisé en mode natif en tant que serveur autonome ou en mode intégré SharePoint afin de prendre en charge les rapports sur un site SharePoint.  
  
 Dans le Générateur de rapports, vous pouvez créer des rapports, des datasets partagés et des parties de rapports réutilisables. À partir d'un serveur de rapports ou d'un site SharePoint, vous pouvez modifier des rapports et ajouter des sources de données partagées, des datasets partagés et des parties de rapports partagées.  
  
 Pour créer, publier et utiliser des rapports et des éléments liés aux rapports, vous devez comprendre l'importance des fonctionnalités de sécurité en ce qui concerne les aspects suivants :  
  
-   **Serveur de rapports ou site SharePoint de publication des rapports** Ces fonctionnalités sont gérées par l'administrateur du serveur de rapports ou l'administrateur de site SharePoint.  
  
-   **Rapports publiés et éléments liés aux rapports** Les éléments liés aux rapports comprennent les sources de données incorporées et partagées, ainsi que les informations d’identification, les datasets partagés, les paramètres, les parties de rapports et les modèles de rapport. Les fonctionnalités de sécurité qui s'appliquent à ces éléments sont gérées par l'auteur du rapport. L'auteur du rapport doit se voir accorder des autorisations suffisantes par l'administrateur du serveur de rapports ou l'administrateur de site SharePoint afin de pouvoir publier et partager les éléments.  
  
-   **Sources de données externes utilisées par un rapport** Ces fonctionnalités sont gérées par le propriétaire de la source de données externe.  
  
-   **Modèles de rapport basés sur des sources de données externes** Ces fonctionnalités sont gérées par le Générateur de modèles.  
  
-   **Fonctionnalités de rapport interactives telles que les paramètres** Ces fonctionnalités sont gérées par l'auteur du rapport.  
  
 Passez en revue les informations de cette rubrique afin de mieux comprendre comment utiliser les fonctionnalités de sécurité pour gérer et sécuriser les rapports et les éléments liés aux rapports.  
  
##  <a name="ReportServers"></a> Fonctionnement de la sécurité des serveurs de rapports  
 La publication de rapports et l'affichage de rapports sont des opérations qui requièrent des privilèges. Un administrateur de serveur de rapports accorde des autorisations pour s'assurer que seuls les utilisateurs autorisés peuvent publier et afficher des rapports sur l'un des types de serveurs de rapports suivants :  
  
-   Serveur de rapports configuré en mode natif  
  
     Pour vous connecter ou accéder à un serveur de rapports, vous devez utiliser une URL valide et disposer d'autorisations d'accès suffisantes.  
  
     Pour permettre l'affichage et la publication des éléments sur un serveur de rapports, les ensembles d'autorisations qui s'appliquent aux éléments et opérations liés aux rapports sont organisés en rôles. Un administrateur de serveur de rapports vous attribue un ou plusieurs rôles. Par exemple, le rôle prédéfini Navigateur vous permet d'afficher les rapports, dossiers, modèles et ressources.  
  
     Si vous ne pouvez pas vous connecter ou accéder à un serveur de rapports, contactez l'administrateur du serveur de rapports. Pour plus d’informations, consultez [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](http://go.microsoft.com/fwlink/?linkid=121312).  
  
-   Serveur de rapports configuré en mode intégré SharePoint  
  
     Pour vous connecter à un site SharePoint intégré à un serveur de rapports, vous devez utiliser une URL valide pointant vers le site ou sous-site SharePoint, et disposer d'autorisations d'accès suffisantes.  
  
     L'autorisation d'accès à des éléments et des opérations liés aux rapports est accordée via des stratégies de sécurité SharePoint qui mappent un compte d'utilisateur ou de groupe à un niveau d'autorisation pour un élément.  
  
     Si vous ne pouvez pas vous connecter ou accéder à un site ou sous-site SharePoint, contactez l'administrateur du site SharePoint.  
  
  
##  <a name="Reports"></a> Fonctionnement de la sécurité des rapports publiés et des éléments liés aux rapports  
 La sécurité des rapports et des éléments liés aux rapports est gérée par l'administrateur du serveur de rapports. Les éléments liés aux rapports comprennent les sources de données incorporées et partagées, ainsi que les informations d'identification, les datasets partagés, les paramètres, les parties de rapports et les modèles.  
  
 Sur un serveur de rapports ou un site SharePoint, les rapports ainsi que les éléments et opérations liés aux rapports sont sécurisables de manière indépendante. L'autorisation d'accès aux éléments et opérations est accordée via des stratégies de sécurité qui mappent un compte d'utilisateur ou de groupe à un niveau d'autorisation pour un élément. Pour réduire la complexité et la charge de traitement de la maintenance d'un grand nombre de stratégies, les autorisations d'un conteneur, tel qu'un dossier, sont héritées par les éléments présents dans le conteneur. Par exemple, si un utilisateur dispose de l'autorisation spécifique Afficher les rapports pour un dossier, il en dispose également pour les éléments présents dans le dossier.  
  
 Les autorisations peuvent être substituées pour les éléments ou les dossiers à l'aide de la sécurité au niveau de l'élément. Lorsque la sécurité au niveau de l'élément est appliquée, l'héritage d'autorisations du conteneur parent ne s'applique plus à l'élément. Si la sécurité au niveau de l'élément est appliquée à un dossier, les dossiers imbriqués héritent des mêmes autorisations.  
  
 Si vous ne parvenez pas à rechercher et trouver des éléments qu'une autre personne a publiés à votre intention, vous avez peut-être un problème d'autorisation pour l'élément ou le dossier concerné.  
  
 Pour permettre aux autres utilisateurs de rechercher et de trouver des éléments que vous avez publiés pour les partager, vous devez collaborer avec l'administrateur du serveur de rapports afin d'organiser les dossiers de manière à les rendre accessibles à vos utilisateurs. L'accès est nécessaire pour la création de rapports et l'exécution des rapports publiés.  
  
 Pour plus d'informations, consultez les rubriques suivantes dans la documentation relative à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] au sein de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312):  
  
-   [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md)  
  
-   [Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md)  
  
### <a name="update-notifications-for-report-parts"></a>Notifications de mise à jour pour les parties de rapports  
 Les parties de rapports sont publiées sur un serveur de rapports afin d'être partagées avec d'autres utilisateurs. Par défaut, vous spécifiez l'emplacement de publication des parties de rapports.  
  
 Les utilisateurs qui incluent des parties de rapports dans leurs rapports peuvent activer la fonctionnalité de mise à jour. Si cette fonctionnalité est activée, les utilisateurs reçoivent des notifications lors de la modification des parties de rapports sur le serveur de rapports.  
  
 Si des parties de rapports ne se trouvent plus à leur emplacement d'origine, la notification de mise à jour inclut à la fois l'emplacement actuel et l'emplacement précédent de la partie de rapport concernée. Acceptez uniquement les mises à jour d'emplacements approuvés.  
  
 Pour plus d’informations, consultez [Publication de parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
  
##  <a name="Data"></a> Fonctionnement de la sécurité des données de rapport et des sources de données externes  
 Pour accéder aux données de chaque source de données externe dans un rapport, vous devez créer une source de données incorporée ou ajouter une référence à une source de données partagée ou un dataset partagé dans votre rapport.  
  
 Pour chaque source de données externe, vous devez fournir des informations d'identification suffisantes pour accéder à la source et aux données sous-jacentes. Le propriétaire de la source de données spécifie le type d'informations d'identification qui permettent cet accès.  
  
 Les informations d'identification ne sont pas enregistrées dans la définition de rapport. Elles sont gérées indépendamment du rapport sur le serveur de rapports ou le site SharePoint, ainsi que sur le client de création de rapports.  
  
 Au moment de la conception du rapport, les informations d'identification sont utilisées pour exécuter les requêtes de dataset et afficher l'aperçu du rapport. Au moment de l'exécution, les informations d'identification sont utilisées pour exécuter le rapport et mettre en cache les résultats des requêtes. Vous pouvez également mettre en cache les résultats des requêtes de dataset partagé de manière indépendante. Les informations d'identification peuvent différer au moment de la conception et au moment de l'exécution. Pour plus d’informations, consultez [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
 Pour plus d'informations sur la sécurisation des données, consultez la rubrique suivante dans la documentation relative à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312):  
  
-   [Centre de sécurité pour le moteur de base de données SQL Server et la base de données SQL Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
 Pour plus d’informations sur les sources de données, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34).  
  
  
##  <a name="Models"></a> Fonctionnement des modèles et des filtres de sécurité  
 Lorsque des données sont récupérées à partir d'un modèle de rapport basé sur des données externes, vous pouvez appliquer des filtres de sécurité dans le modèle. Il s'agit d'un bon moyen pour sécuriser les données afin que chaque utilisateur qui exécute un rapport puisse consulter uniquement les données auxquelles il a l'autorisation d'accéder.  
  
 Les paramètres de rapport ne sont pas utilisés pour la sécurité au niveau des lignes ; ils n'empêchent pas des utilisateurs ou des groupes d'utilisateurs de visualiser des lignes de données spécifiques. Pour appliquer la sécurité aux données affichées dans un rapport, vous devez utiliser des filtres de sécurité ou la sécurité des éléments de modèle.  
  
  
##  <a name="Interactive"></a> Fonctionnement de la sécurité en matière de création de rapports pour les fonctionnalités interactives  
 Les rapports comportent fréquemment des paramètres pour permettre à l'utilisateur de personnaliser de manière interactive l'affichage d'un rapport. Utilisez les conseils suivants pour concevoir des rapports basés sur les meilleures pratiques :  
  
-   N'utilisez pas de paramètres basés sur des paramètres de requête et qui sont de type **Texte** , à moins que vous ne fournissiez des valeurs valides. Une liste de valeurs disponibles aide l'utilisateur à choisir uniquement des valeurs valides. Sans une liste de valeurs disponibles, vous ne pouvez pas restreindre les valeurs qu'un utilisateur peut entrer.  
  
-   N'utilisez pas la valeur globale [&UserID] pour sécuriser des données privées. En tant que paramètre de rapport, cette valeur peut être spécifiée dans une URL de rapport à l'aide de la syntaxe d'accès URL. L'utilisation de cette valeur dans une expression d'un dataset partagé empêche le dataset d'être mis en cache. Pour plus d’informations, consultez [Référence de paramètre d’accès URL](../../reporting-services/url-access-parameter-reference.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](http://go.microsoft.com/fwlink/?linkid=121312).  
  
 Une fois les éléments publiés sur un serveur de rapports, l'administrateur du serveur de rapports peut contribuer à les sécuriser à l'aide d'une sécurité basée sur les rôles ou d'une sécurité au niveau des éléments et des dossiers. Pour plus d’informations, consultez [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md) dans la documentation relative à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la [documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](http://go.microsoft.com/fwlink/?linkid=121312).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Installer et désinstaller le Générateur de rapports](http://msdn.microsoft.com/library/2c9a5814-17bf-4947-8fb3-6269e7caa416)   
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)  
  
  
