---
title: Planifications | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- schedules [Reporting Services]
- schedules [Reporting Services], about schedules
- published reports [Reporting Services], schedules
- reports [Reporting Services], scheduling
- subscriptions [Reporting Services], scheduling
- automatic report processing
ms.assetid: ecccd16b-eba9-4e95-b55d-f15c621e003f
caps.latest.revision: 51
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: ff16cd9d2794ded1d6b841a14d4ee01f45a9f9d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="schedules"></a>Planifications
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit des **planifications partagées** et des **planifications spécifiques aux rapports** pour vous aider à contrôler le traitement et la distribution des rapports. La différence entre ces deux types de planifications réside dans la façon dont elles sont définies, stockées et gérées. La construction interne des deux types de planifications est la même. Toutes les planifications spécifient un type de périodicité : mensuelle, hebdomadaire ou quotidienne. Dans le type de périodicité, vous définissez les intervalles et la plage pour configurer la fréquence à laquelle un événement se produit. Qu'il s'agisse de créer une planification partagée ou une planification spécifique aux rapports, le type de périodicité et la façon dont elle est spécifiée restent inchangés.
  
  -   Les planifications partagées sont créées en tant qu'éléments distincts. Après les avoir créées, vous les référencez lors de la définition d'un abonnement ou de toute autre opération planifiée.  
  
-   Les planifications spécifiques aux rapports sont créées lorsque vous définissez un abonnement ou configurez des propriétés d'exécution de rapport. L'entrée des informations de planification fait partie de la définition d'un abonnement ou du paramétrage des propriétés. Pour définir une planification spécifique aux rapports, ouvrez le rapport ou l'abonnement qui l'utilise.  
  
 Une planification partagée contient les informations de planification et de périodicité qui peuvent être utilisées par tous les rapports publiés et abonnements qui s'exécutent sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si de nombreux rapports et abonnements s'exécutent en même temps, vous pouvez créer une planification partagée pour ces travaux. Si vous souhaitez modifier ultérieurement la périodicité ou la date de fin, vous pouvez effectuer cette modification à un seul emplacement.  
  
 La maintenance des planifications partagées est plus facile à assurer ; par ailleurs, ces dernières vous apportent une plus grande souplesse de gestion des opérations planifiées. Par exemple, vous pouvez suspendre et reprendre des planifications partagées. En outre, si vous trouvez que trop d'opérations planifiées s'exécutent en même temps, vous pouvez créer plusieurs planifications partagées qui s'exécutent à des heures différentes, puis vous pouvez ajuster les informations de planification de sorte que la charge de traitement soit répartie de manière égale sur le serveur de rapports.  
  
  
##  <a name="bkmk_whatyoucando"></a> Opérations réalisables avec les planifications  
 Vous pouvez utiliser le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] en mode natif et les pages d’administration de site SharePoint en mode SharePoint pour créer et gérer vos planifications. Vous pouvez :  
  
-   Planifier la remise de rapports dans un abonnement standard ou piloté par des données.  
  
-   Planifier l'historique de rapport afin que de nouveaux instantanés soient ajoutés à l'historique de rapport à des fréquences régulières.  
  
-   Planifier le moment auquel les données d'un instantané de rapport sont actualisées.  
  
-   Planifier le moment auquel les données d'un dataset partagé sont actualisées  
  
-   Planifier l'expiration d'un rapport mis en cache ou d'un dataset partagé à une heure prédéfinie pour permettre son actualisation ultérieure.  
  
 Vous pouvez créer une planification partagée si vous voulez utiliser les mêmes informations de planification pour de nombreux rapports ou abonnements. Les planifications partagées sont définies séparément, puis référencées dans des rapports, des datasets partagés et des abonnements qui requièrent des informations de planification.  
  
 Lorsque vous créez une planification, le rapport enregistre les informations de planification dans la base de données du serveur de rapports ou, pour le mode SharePoint, la base de données d'application de service. Le serveur de rapports crée également un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est utilisé pour déclencher la planification. Le traitement des planifications est basé sur l'heure locale du serveur de rapports contenant la planification. Le format horaire se conforme à celui du système d'exploitation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Pour plus d'informations sur la création et la gestion des planifications, consultez [Create, Modify, and Delete Schedules](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md).  
  
> [!NOTE]  
>  Les opérations de planification ne sont pas disponibles dans toutes les édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](http://msdn.microsoft.com/library/22ad82d7-860c-43d3-b77a-77fb9eec5454).  
  
##  <a name="bkmk_compare"></a> Comparaison entre les planifications partagées et les planifications spécifiques aux rapports  
 Les deux types de planification produisent le même résultat.  
  
-   Les**planifications partagées** sont des éléments portables à usage général qui contiennent des informations de planification prêtes à l’emploi. Comme les planifications partagées sont des éléments de niveau système, la création d'une planification partagée exige des autorisations de niveau système. C'est pourquoi c'est un administrateur de serveur de rapports ou un gestionnaire de contenu qui crée généralement les planifications partagées disponibles sur votre serveur de rapports. Les planifications partagées sont stockées et gérées sur le serveur de rapports à l’aide du portail web ou des paramètres de site SharePoint.  
  
     Par opposition aux planifications spécifiques que vous définissez par le biais des propriétés de rapport, de dataset partagé ou d'abonnement, les planifications partagées sont plus faciles à gérer et à maintenir pour les raisons suivantes :  
  
    -   Les planifications partagées peuvent être gérées à partir d'un emplacement central, ce qui facilite la comparaison des propriétés de planification et l'ajustement des modèles de fréquence et de périodicité lorsque les opérations planifiées s'exécutent selon un intervalle trop rapproché, ou lorsqu'elles sont en conflit avec d'autres processus sur votre serveur.  
  
    -   Les planifications partagées vous permettent de vous adapter rapidement aux modifications de l'environnement informatique. Par exemple, supposons qu'un ensemble de rapports sont exécutés à 4h00 du matin après qu'un entrepôt de données est actualisé. Si l'opération d'actualisation des données est replanifiée ou différée, vous pouvez facilement gérer cette modification en mettant à jour les informations de planification dans une planification partagée unique.  
  
    -   Si vous utilisez uniquement des planifications partagées, vous savez précisément à quel moment les opérations planifiées ont lieu. Cela simplifie l'anticipation et la gestion des charges du serveur avant que des problèmes de performances ne se produisent. Par exemple, si vous décidez de planifier des sauvegardes d'ordinateurs à une heure spécifique, vous pouvez ajuster les planifications partagées pour qu'elles s'exécutent à des heures différentes.  
  
-   Les**planifications spécifiques aux rapports** sont définies dans le contexte d’un rapport, d’un abonnement ou d’une opération d’exécution de rapport pour déterminer le délai d’expiration d’un cache ou des mises à jour d’instantanés. Ces planifications sont créées en ligne lorsque vous définissez un abonnement ou précisez des propriétés d'exécution de rapport. Vous pouvez créer une planification spécifique aux rapports si une planification partagée ne fournit pas le modèle de fréquence ou de périodicité dont vous avez besoin. Pour empêcher l'exécution d'un rapport, vous devez modifier manuellement la planification spécifique à ce rapport. Les planifications spécifiques aux rapports peuvent être créées par des utilisateurs.  
  
##  <a name="bkmk_configuredatasources"></a> Configurer les sources de données  
 Avant de pouvoir planifier le traitement de données ou d'abonnements d'un rapport, vous devez configurer la source de données de rapport afin d'utiliser les informations d'identification stockées ou le compte de traitement de rapport sans assistance. Si vous utilisez des informations d'identification stockées, vous ne pouvez stocker qu'un seul jeu d'informations d'identification ; par ailleurs, ces dernières seront employées par tous les utilisateurs qui exécutent le rapport. Les informations d'identification peuvent correspondre à un compte d'utilisateur Windows ou un compte d'utilisateur de base de données.  
  
 Le compte de traitement de rapport sans assistance est un compte spécial configuré sur le serveur de rapports. Il est utilisé par le serveur de rapports pour se connecter aux ordinateurs distants lorsqu'une opération planifiée nécessite la récupération d'un fichier ou traitement externe. Si vous configurez ce compte, vous pouvez l'utiliser pour vous connecter aux sources de données externes qui fournissent des données à un rapport.  
  
 Pour spécifier les informations d'identification stockées ou le compte de traitement de rapport sans assistance, modifiez les propriétés de la source de données du rapport. Si le rapport utilise une source de données partagée, modifiez plutôt cette dernière.  
  
##  <a name="bkmk_credentials"></a> Stocker les informations d'identification et les comptes de traitement  
 La façon dont vous travaillez avec une planification dépend des tâches faisant partie de votre attribution de rôle. Si vous utilisez des rôles prédéfinis, les utilisateurs qui sont des gestionnaires de contenu et des administrateurs système peuvent créer et gérer n'importe quelle planification. Si vous utilisez des attributions de rôle par défaut, l'attribution de rôle doit inclure les tâches prenant en charge les opérations planifiées.  
  
|Pour effectuer cette opération|Incluez cette tâche|Rôles prédéfinis du mode natif|Groupes du mode SharePoint|  
|----------------|-----------------------|----------------------------------|----------------------------|  
|Créer, modifier ou supprimer des planifications partagées|Gérer les planifications partagées|Administrateur système|Propriétaires|  
|Sélectionner les planifications partagées|Afficher les planifications partagées|Utilisateur système|Membres|  
|Créer, modifier ou supprimer des planifications spécifiques aux rapports dans un abonnement défini par l'utilisateur|Gérer les abonnements individuels|Navigateur, Générateur de rapports, Mes rapports, Gestionnaire de contenu|Visiteurs, membres|  
|Créer, modifier ou supprimer des planifications spécifiques aux rapports pour toutes les autres opérations planifiées|Gérer l'historique de rapport, gérer tous les abonnements, gérer les rapports|Gestionnaire de contenu|Propriétaires|  
  
 Pour plus d’informations sur la sécurité dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]en mode natif, consultez [Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md), [Octroi d’autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md) et [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md). Pour le mode SharePoint, consultez [Comparer des rôles et des tâches dans Reporting Services pour des autorisations et des groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)  
  
##  <a name="bkmk_how_scheduling_works"></a> Fonctionnement du traitement des planifications et des livraisons  
 Le processeur de planification et de livraison présente les fonctionnalités suivantes :  
  
-   Maintient une file d'attente d'événements et de notifications dans la base de données du serveur de rapports. Dans un déploiement avec montée en puissance parallèle, la file d'attente est partagée par tous les serveurs de rapports de la structure.  
  
-   Appel au composant processeur de rapports pour exécuter les rapports, traiter les abonnements ou supprimer un rapport mis en cache. Le traitement des rapports qui se produit à la suite d'un événement de planification est entièrement effectué en arrière-plan. Le mode SharePoint utilise les travaux du minuteur.  
  
-   Appelle l'extension de remise qui est spécifiée dans un abonnement de sorte que le rapport puisse être remis.  
  
 Les autres aspects d'une opération de planification et de remise sont gérés par d'autres composants et services qui fonctionnent avec le processeur de planification et de livraison. En particulier, le processeur de planification et de livraison s'exécute dans le service Report Server et utilise l'Agent SQL Server comme minuteur pour générer des événements planifiés. La procédure pas à pas suivante explique le fonctionnement des opérations planifiées dans un déploiement de Reporting Services :  
  
1.  Une opération planifiée est définie lorsqu'un utilisateur crée une planification. La planification définit la date et l'heure qui seront utilisées pour déclencher un abonnement pour la remise de rapport, actualiser un instantané ou terminer un cache.  
  
2.  Le serveur de rapports enregistre les informations de planification dans la base de données du serveur de rapports.  
  
3.  Le serveur de rapports crée un travail correspondant dans l'Agent SQL Server qui inclut les informations de planification fournies. Les travaux sont créés à l'aide d'une procédure stockée, qui utilise la connexion ouverte existante à la base de données du serveur de rapports.  
  
4.  L'Agent SQL Server exécute le travail aux date et heure spécifiées dans la planification. Le travail crée un événement qui est ajouté à une file d'attente gérée par Reporting Services.  
  
5.  L'événement provoque le traitement d'un rapport ou d'un abonnement. Les événements sont traités lorsqu'ils sont détectés dans la file d'attente, et le rapport est traité ou remis en conséquence.  
  
     Avant que les événements ne soient traités, le processeur de planification et de livraison procède à une étape d'authentification pour vérifier que le propriétaire de l'abonnement est autorisé à consulter le rapport.  
  
 Reporting Services gère une file d'attente d'événements pour toutes les opérations planifiées. Il interroge régulièrement la file d'attente pour vérifier si elle contient de nouveaux événements. Par défaut, la file d'attente fait l'objet d'une analyse toutes les 10 secondes. Si vous souhaitez changer cette fréquence, modifiez les paramètres de configuration **PollingInterval**, **IsNotificationService**et **IsEventService** dans le fichier RSReportServer.config. Le mode SharePoint utilise également le fichier RSreporserver.config pour ces paramètres et les valeurs s'appliquent à toutes les applications de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Fichier de configuration RSReportServer](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
##  <a name="bkmk_serverdependencies"></a> Dépendances de serveur  
 Il est impératif que le service Report Server et l'Agent SQL Server s'exécutent pour que le processeur de planification et de livraison fonctionne. La fonctionnalité de traitement des planifications et des livraisons doit être activée par le biais de la propriété **ScheduleEventsAndReportDeliveryEnabled** de la facette **Configuration de la surface d’exposition pour Reporting Services** de la gestion basée sur une stratégie. Enfin, les opérations planifiées ne peuvent se déclencher que si l'Agent SQL Server et le service Report Server sont en cours d'exécution.  
  
> [!NOTE]  
>  Vous pouvez utiliser la facette **Configuration de la surface d'exposition pour Reporting Services** pour interrompre temporairement ou définitivement des opérations planifiées. Bien que vous puissiez créer et déployer des extensions de remise personnalisées, le processeur de planification et de livraison n'est en lui-même pas extensible. Vous ne pouvez pas modifier la manière dont il gère les événements et les notifications. Pour plus d'informations sur la désactivation des fonctionnalités, consultez la section **Événements planifiés et remise** de [Turn Reporting Services Features On or Off](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md).  
  
###  <a name="bkmk_stoppingagent"></a> Conséquences de l'interruption de l'Agent SQL Server  
 Le traitement des rapports planifiés utilise l'Agent SQL Server par défaut. Si vous arrêtez ce service, aucune nouvelle demande de traitement ne sera ajoutée à la file d’attente à moins d’y être intégrée, par programmation, à l’aide de la méthode <xref:ReportService2010.ReportingService2010.FireEvent%2A> . Lorsque vous redémarrez le service, les travaux à l'origine des demandes de traitement des rapports reprennent leur cours. Le serveur de rapports ne tente pas de recréer les travaux de traitement des rapports survenus précédemment tandis que l'Agent SQL Server était en mode hors connexion. Si vous arrêtez l'Agent SQL Server l'espace d'une semaine, toutes les opérations planifiées de cette semaine sont perdues.  
  
> [!NOTE]  
>  La fonction assurée par SQL Server Agent pour Reporting Services peut être remplacée par un code personnalisé qui utilise la méthode <xref:ReportService2010.ReportingService2010.FireEvent%2A> pour ajouter des événements planifiés dans la file d’attente.  
  
###  <a name="bkmk_stoppingservice"></a> Conséquences de l'arrêt du service Report Server  
 Si vous arrêtez le service Report Server, l'Agent SQL Server continue malgré tout d'ajouter des demandes de traitement de rapport à la file d'attente. Les informations d'état de SQL Server Agent indiquent que les travaux ont été correctement effectués. Toutefois, aucun traitement de rapport n'a, en réalité, été réalisé puisque le service Report Server ne fonctionne pas. Les demandes continueront de s'accumuler dans la file d'attente jusqu'à ce que le service Report Server redémarre. Une fois ce service relancé, toutes les demandes de traitement de rapport présentes dans la file d'attente sont traitées dans l'ordre.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer, modifier et supprimer des instantanés dans l'historique de rapport](../../reporting-services/report-server/create-modify-and-delete-snapshots-in-report-history.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Mise en cache de rapports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)  
  
  
