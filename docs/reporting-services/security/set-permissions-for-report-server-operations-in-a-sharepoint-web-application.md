---
title: Définir des autorisations pour des opérations de serveurs de rapports dans une application web SharePoint | Microsoft Docs
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
- SharePoint integration [Report Builder]
- security [Reporting Services], SharePoint integrated mode
- Report Builder 1.0, SharePoint integration
- model item security [Reporting Services]
ms.assetid: 9ea71f1a-ee9e-4337-95ff-d7cef79946e7
caps.latest.revision: 17
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2fc28110b4d927e95eb3791f9d3408f046c683d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-permissions-for-report-server-operations-in-a-sharepoint-web-application"></a>Définir des autorisations pour les opérations de serveur de rapports dans une application web SharePoint
  Pour un serveur de rapports qui s'exécute en mode intégré SharePoint, les paramètres de sécurité définis sur le site SharePoint déterminent le mode d'affichage et de gestion des rapports, des modèles de rapport et des sources de données partagées. Si vous utilisez les groupes SharePoint par défaut, les niveaux d'autorisation et les attributions d'autorisations, vous pouvez utiliser les rapports et d'autres documents à l'aide des paramètres de sécurité actuels.  
  
 Si les paramètres de sécurité par défaut n'offrent pas le niveau d'accès souhaité, vous pouvez utiliser les informations fournies dans les sections suivantes pour connaître les autorisations nécessaires à des opérations particulières :  
  
-   [Autorisations pour afficher et gérer des rapports](#permissionReports)  
  
-   [Autorisations pour la création de rapports et l'utilisation du Générateur de rapports](#permissionReportBuilder)  
  
-   [Autorisations pour créer et gérer des planifications partagées](#permissionSharedSchedules)  
  
-   [Autorisations pour créer et gérer des abonnements](#permissionSubscriptions)  
  
-   [Autorisations pour créer et gérer des sources de données partagées et des modèles de rapport](#permissionDataSources)  
  
 Quelques autorisations clés sont requises pour effectuer presque toutes les opérations d'un site SharePoint. Ces autorisations ne sont pas répertoriées dans les tableaux de tâches et d'autorisations ci-dessous, mais vous devez les inclure si vous créez des niveaux d'autorisation personnalisés :  
  
-   Parcourir les informations utilisateur  
  
-   Utiliser les interfaces distantes  
  
-   Ouvrir  
  
-   Afficher les pages des applications  
  
 Si vous utilisez des niveaux d'autorisation prédéfinis, aucune action n'est nécessaire, car les autorisations ci-dessus sont déjà incluses dans Contrôle total, Création, Collaboration, Lecture et Accès limité. Cependant, si vous utilisez des niveaux d'autorisation personnalisés ou si vous modifiez les autorisations attribuées à un utilisateur ou un groupe spécifique, vous devez ajouter cette autorisation manuellement.  
  
 L'autorisation « Parcourir les informations utilisateur » permet au serveur de rapports de retourner des informations sur l'auteur de l'élément et sur l'utilisateur qui l'a modifié en dernier. Sans cette autorisation, le serveur de rapports retourne les erreurs ci-après. Pour les opérations de navigation, l'erreur est : « Report Server a rencontré une erreur SharePoint. ---> System.UnauthorizedAccessException : accès refusé. » Pour les opérations de publication, l’erreur est : « Les autorisations accordées à l’utilisateur '\<domaine>\\<utilisateur\>' sont insuffisantes pour effectuer cette opération ».  
  
##  <a name="permissionReports"></a> Autorisations pour afficher et gérer des rapports  
 Les autorisations de définition de rapport sont définies par l'intermédiaire des autorisations pour les listes sur la bibliothèque contenant le rapport ; vous pouvez toutefois définir des autorisations sur des rapports individuels si vous souhaitez en restreindre l'accès. Le tableau suivant présente une liste de tâches et les autorisations nécessaires pour chacune d'entre elles.  
  
|Tâche|Autorisation|  
|----------|----------------|  
|Afficher un rapport.|**Afficher les éléments** sur la bibliothèque contenant les fichiers ou sur le rapport individuel.|  
|Afficher un rapport consultable à l'aide de clics qui utilise un modèle de rapport comme source de données.|**Afficher les éléments** sur la bibliothèque contenant le rapport ou le modèle de rapport, ou sur le rapport individuel. Si vous ne disposez pas d'autorisations d'affichage sur le modèle, vous pouvez cependant ouvrir le rapport, mais l'exploration ad hoc des données n'est pas possible.<br /><br /> Si le modèle de rapport utilise la sécurité des éléments d'un modèle, l'utilisateur doit aussi disposer d'une autorisation **Énumérer les autorisations** sur le modèle de rapport.|  
|Afficher des instantanés de l'historique de rapport.|**Modifier les éléments** dans la bibliothèque contenant les fichiers ou dans le rapport individuel. Pour un rapport spécifique, vous pouvez afficher l'intégralité de l'historique de rapport ou ne rien afficher. Il n'est pas possible de définir des autorisations sur des instantanés individuels de l'historique de rapport.|  
|Télécharger ou publier un rapport dans une bibliothèque.|**Ajouter des éléments** sur la bibliothèque qui contiendra le rapport.|  
|Définir les propriétés sur un rapport, notamment les informations de connexion aux sources de données, les options de traitement et les propriétés des paramètres.|**Modifier des éléments** sur la bibliothèque contenant le rapport ou sur le rapport individuel. Vous devez également disposer d'autorisations d'affichage sur une source de données partagée (.rsds) pour pouvoir la sélectionner et l'utiliser avec le rapport.|  
|Planifier le traitement d'un rapport.|Pour sélectionner une planification partagée, vous devez disposer de l'autorisation **Ouvrir** sur le site contenant la bibliothèque qui contient le rapport. Pour planifier le traitement des données ou l'expiration du cache, vous devez disposer de l'autorisation **Modifier des éléments** sur la bibliothèque contenant le rapport ou sur le rapport individuel.|  
|Supprimer un rapport.|**Supprimer des éléments** sur la bibliothèque contenant le rapport ou sur le rapport individuel.|  
|Remplacer la définition d'un rapport (sans incidence sur ses propriétés, ses autorisations, son historique ou ses abonnements) par une version plus récente.|**Modifier des éléments** sur la bibliothèque contenant le rapport ou sur le rapport individuel.|  
|Créer des instantanés de l'historique de rapport.|**Ajouter des éléments** sur la bibliothèque contenant le rapport pour lequel vous créez l'historique de rapport.|  
|Créer des instantanés de l'historique de rapport.|**Ajouter des éléments** sur la bibliothèque contenant le rapport pour lequel vous créez l'historique de rapport.|  
|Supprimer les instantanés de l'historique de rapport et supprimer des versions spécifiques de définitions de rapport extraites et modifiées de temps à autre.|**Supprimer les versions** sur la bibliothèque contenant le rapport pour lequel vous supprimez l'historique de rapport.|  
|Afficher les instantanés de l'historique de rapport et afficher des versions spécifiques de définitions de rapport extraites et modifiées de temps à autre.|**Afficher les versions** sur la bibliothèque qui contient le rapport.|  
  
##  <a name="permissionReportBuilder"></a> Autorisations pour la création de rapports et l'utilisation du Générateur de rapports  
 Le Générateur de rapports est un outil de création de rapports permettant de créer des rapports ad hoc. Le Générateur de rapports utilise les modèles de rapport comme source de données pour prendre en charge l'exploration ad hoc des données. Vous pouvez charger un modèle dans le Générateur de rapports pour créer un rapport, puis l'exécuter, cliquer sur des données du modèle et éventuellement enregistrer le rapport dans une bibliothèque. Les utilisateurs bénéficiant des autorisations suffisantes peuvent ensuite ouvrir le même rapport et effectuer également une exploration ad hoc des données.  
  
> [!NOTE]  
>  L'accès au Générateur de rapports peut être déterminé par d'autres facteurs que les autorisations. Un administrateur de site peut désactiver les rapports ad hoc en définissant des propriétés sur le serveur ; il peut également limiter la disponibilité du Générateur de rapports en n'ajoutant pas le type de contenu Rapport du Générateur de rapports, ce qui empêche les utilisateurs de créer de nouveaux rapports à partir du menu **Nouveau** d'une bibliothèque. En outre, un administrateur de serveur de rapports peut rendre le Générateur de rapports non disponible en définissant des propriétés sur le serveur de rapports. Si l'une de ces conditions est vraie pour votre serveur, vous ne pouvez pas utiliser le Générateur de rapports même si vous disposez des autorisations nécessaires.  
  
 Le tableau suivant présente une liste de tâches permettant de créer des rapports et d'utiliser le Générateur de rapports, ainsi que les autorisations nécessaires pour chacune d'entre elles :  
  
|Tâche|Autorisation|  
|----------|----------------|  
|Démarrer le Générateur de rapports.|Aucune autorisation ne sert explicitement à contrôler l'accès à l'utilisation du Générateur de rapports. Le Générateur de rapports est disponible si l'intégration du serveur de rapports est configurée et si vous disposez de l'autorisation d'ajouter des éléments à une bibliothèque. Pour démarrer le Générateur de rapports à partir du menu **Nouveau** de la bibliothèque, vous devez enregistrer le type de contenu Générateur de rapports. Pour plus d’informations, consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).|  
|Télécharger un modèle ou une source de données partagée.|**Ajouter des éléments** sur la bibliothèque qui contiendra les fichiers.|  
|Afficher un modèle ou une source de données partagée dépendante.|**Afficher les éléments** sur la bibliothèque qui contient les fichiers.<br /><br /> Si le modèle contient des paramètres de la sécurité des éléments d'un modèle, l'utilisateur doit aussi disposer d'une autorisation **Énumérer les autorisations** sur le modèle.|  
|Créer un modèle à partir d'une source de données partagée.|**Ajouter des éléments** sur la bibliothèque contenant le fichier de source de données partagée (.rsds) à partir duquel vous créez le modèle.|  
|Définir des autorisations dans un modèle sur des éléments particuliers du modèle.|**Gérer les autorisations** sur le site contenant la bibliothèque et le fichier de modèle de rapport (.smdl).|  
|Charger un modèle dans le Générateur de rapports.|**Modifier des éléments** sur le fichier de modèle de rapport (.smdl).|  
|Créer une définition de rapport dans le Générateur de rapports et enregistrer un rapport dans une bibliothèque.|**Ajouter des éléments** pour enregistrer le fichier dans une bibliothèque.|  
|Modifier un rapport dans le Générateur de rapports.|**Modifier des éléments** sur le fichier de définition de rapport.|  
  
 Les autorisations pour créer et utiliser les abonnements, l'historique de rapport et pour définir les options de traitement des rapports ou des données sur un rapport du Générateur de rapports sont identiques à celles servant à ces mêmes actions sur les fichiers de définition de rapport standard.  
  
##  <a name="permissionSharedSchedules"></a> Autorisations pour créer et gérer des planifications partagées  
 Les planifications partagées ne sont pas des documents stockés dans une bibliothèque. Pour cette raison, la création et la gestion de ces planifications exigent des autorisations sur le site. Vous ne pouvez pas restreindre l'accès à des planifications partagées spécifiques. Toutes les planifications partagées créées seront disponibles à tous les utilisateurs qui disposent d'une autorisation Ouvrir sur l'ensemble du site.  
  
 Le tableau suivant présente une liste des tâches et des autorisations permettant de créer, gérer et utiliser les planifications partagées :  
  
|Tâche|Autorisation|  
|----------|----------------|  
|Créer, modifier ou supprimer une planification partagée.|**Gérer le site Web** sur le site.|  
|Sélectionner une planification partagée pour le traitement des abonnements ou la récupération de données.|**Ouvrir** sur le site contenant la bibliothèque.|  
  
##  <a name="permissionSubscriptions"></a> Autorisations pour créer et gérer des abonnements  
 SharePoint applique une dépendance entre l'abonnement et les autorisations d'affichage. Vous ne pouvez pas vous abonner à un rapport que vous n'êtes pas autorisé à afficher. Si vous accordez des autorisations d'abonnement à un rapport, les autorisations d'affichage sont automatiquement accordées.  
  
 Le tableau suivant présente une liste des tâches et des autorisations permettant de créer, gérer et utiliser les abonnements :  
  
|Tâche|Autorisation|  
|----------|----------------|  
|Créer, modifier ou supprimer un abonnement à un rapport spécifique, qui a pour propriétaire un utilisateur.|**Modifier des éléments** sur la bibliothèque contenant le rapport ou sur le rapport lui-même. Afficher les éléments est une autorisation dépendante qui sera automatiquement incluse dans le niveau d'autorisation. Les utilisateurs en mesure de créer un abonnement peuvent également créer des planifications personnalisées pour exécuter cet abonnement.|  
|Sélectionner une planification partagée à utiliser avec l'abonnement.|**Ouvrir** sur le site contenant la bibliothèque.|  
|Créer, modifier ou supprimer un abonnement quelconque d'un site.|**Gérer les alertes** sur le site.|  
  
##  <a name="permissionDataSources"></a> Autorisations pour créer et gérer des sources de données partagées et des modèles de rapport  
 Un fichier de source de données partagée (.rsds) contient des informations de connexion à la source de données utilisables par plusieurs rapports et modèles. Pour les rapports standard, l'utilisation d'un fichier .rsds pour spécifier les informations de connexion à la source de données est facultative. Pour les rapports pilotés par un modèle, l'utilisation d'un fichier .rsds est nécessaire. Un modèle de rapport utilise toujours un fichier .rsds pour se connecter à des sources de données externes.  
  
 Vous pouvez définir des propriétés sur les sources de données partagées qui déterminent si des utilisateurs individuels peuvent afficher ou gérer ces sources de données partagées. Les autorisations pour afficher ou gérer une source de données partagée sont différentes des autorisations pour afficher un rapport ; vous pouvez afficher un rapport utilisant un fichier .rsds sans disposer d'autorisation d'affichage sur le fichier .rsds lui-même.  
  
|Tâches|Autorisation|  
|-----------|----------------|  
|Créer une source de données partagée.|**Ajouter des éléments** sur la bibliothèque contenant la source de données partagée. Vous pouvez créer de nouvelles sources de données partagées à partir du menu Nouveau d'une bibliothèque. Pour ce faire, vous devez enregistrer le type de contenu Source de données du rapport avec la bibliothèque. Pour plus d’informations, consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).|  
|Modifier une source de données partagée.|**Modifier des éléments** sur la bibliothèque contenant la source de données partagée ou sur la source de données partagée elle-même.|  
|Supprimer une source de données partagée.|**Supprimer des éléments** sur la bibliothèque contenant la source de données partagée ou sur la source de données partagée elle-même.|  
|Utiliser une source de données partagée (.rsds) avec un rapport.|**Modifier des éléments** sur le rapport ou sur la bibliothèque contenant le rapport. La sélection d'une source de données partagée fait partie de la définition des propriétés de la source de données sur un rapport.|  
|Créer un modèle de rapport à partir d'une source de données partagée.|**Ajouter des éléments** sur la bibliothèque qui contiendra le modèle de rapport.|  
|Supprimer un modèle de rapport.|**Supprimer des éléments** sur la bibliothèque contenant le modèle de rapport ou sur le modèle de rapport lui-même.|  
|Définir des autorisations dans un modèle sur des éléments particuliers du modèle.|**Gérer les autorisations** sur le site contenant la bibliothèque et le fichier de modèle de rapport (.smdl).|  
  
> [!NOTE]  
>  Il n'existe pas d'autorisation pour modifier des modèles de rapport. Même si vous pouvez créer ou supprimer des modèles de rapport, vous ne pouvez pas les modifier depuis un site SharePoint. La modification de modèles de rapport exige le Générateur de modèles, un outil de création client sur lequel les autorisations définies dans SharePoint sont sans effet.  
  
## <a name="see-also"></a> Voir aussi  
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)   
 [Comparer des rôles et des tâches dans Reporting Services avec les autorisations et les groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)   
 [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md)  
  
  
