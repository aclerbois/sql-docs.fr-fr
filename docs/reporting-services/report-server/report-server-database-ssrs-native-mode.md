---
title: Base de données du serveur de rapports (SSRS en mode natif) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [Reporting Services]
- report servers [Reporting Services], databases
- temporary databases [Reporting Services]
- report server database
- reportservertempdb
- reportserver database
ms.assetid: 0fc5c033-3fe1-4cea-86c7-66ea5e424d65
caps.latest.revision: 48
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d1f1eeeeaf960fbfe8659abd5a7299dd6b924212
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-database-ssrs-native-mode"></a>Base de données du serveur de rapports (SSRS en mode natif)
  Un serveur de rapports est un serveur sans état qui utilise le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour stocker les métadonnées et les définitions d'objets. Une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif utilise deux bases de données pour distinguer le stockage de données persistantes des obligations de stockage temporaire. Les bases de données sont créées ensemble et liées par le nom. Les noms par défaut de ces bases de données sont respectivement **reportserver** et **reportservertempdb**.  
  
 Une installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint crée également une base de données pour la fonctionnalité d'alertes de données. Les trois bases de données en mode SharePoint sont associées aux applications de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  
  
 Les bases de données peuvent s'exécuter sur une instance locale ou distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Le choix d'une instance locale est utile si vous possédez suffisamment de ressources système ou si vous voulez économiser des licences logicielles, mais l'exécution des bases de données sur un ordinateur distant permet d'améliorer les performances.  
  
 Vous pouvez déplacer ou réutiliser une base de données de serveur de rapports existante provenant d'une installation précédente ou d'une instance différente avec une autre instance de serveur de rapports. Le schéma de la base de données du serveur de rapports doit être compatible avec l'instance du serveur de rapports. Si la base de données est dans un format plus ancien, vous serez invité à le mettre à niveau au format actuel. Les versions plus récentes ne peuvent pas être réajustées vers une version antérieure. Si vous possédez une base de données de serveur de rapports récente, vous ne pouvez pas l'utiliser avec une version antérieure d'une instance de serveur de rapports. Pour plus d’informations sur la façon dont les bases de données de serveur de rapports sont mises à niveau vers des formats plus récents, consultez [Mettre à niveau une base de données du serveur de rapports](../../reporting-services/install-windows/upgrade-a-report-server-database.md).  
  
> [!IMPORTANT]  
>  La structure de table des bases de données est optimisée pour les opérations serveur et ne doit pas être modifiée ou ajustée. [!INCLUDE[msCoName](../../includes/msconame-md.md)] peut éventuellement modifier la structure de table d'une version à une autre. Si vous modifiez ou agrandissez la base de données, vous pouvez limiter voire supprimer la possibilité d'effectuer des mises à jour ou d'appliquer des Service Packs. Vous risquez également de créer des perturbations sur le serveur de rapports par les modifications que vous effectuez. Par exemple si vous activez READ_COMMITTED_SNAPSHOT sur la base de données ReportServer, vous arrêterez des fonctionnalités interactives de tri.  
  
 Tous les accès à une base de données du serveur de rapports doivent être gérés par le biais du serveur de rapports. Pour avoir accès au contenu d’une base de données de serveur de rapports, utilisez les outils d’administration dédiés aux serveurs de rapports (tels que le Gestionnaire de rapports et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]) ou des interfaces par programmation telles que l’accès par URL, le service Web Report Server ou le fournisseur WMI (Windows Management Instrumentation).  
  
 La connexion à la base de données du serveur de rapports est généralement définie par l'intermédiaire du Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Toutefois, cette connexion peut être configurée au cours de l'installation si vous choisissez d'installer la configuration par défaut. Pour plus d’informations sur la connexion du serveur de rapports à la base de données, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="report-server-database"></a>base de données du serveur de rapports  
 La base de données d'un serveur de rapports est une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui stocke le contenu suivant :  
  
-   éléments gérés par un serveur de rapports (rapports et rapports liés, sources de données partagées, modèles de rapport, dossiers, ressources) ainsi que toutes les propriétés et les paramètres de sécurité qui sont associés à ces éléments ;  
  
-   définitions des abonnements et des planifications ;  
  
-   instantanés de rapport (notamment les résultats de requête) et historique de rapport ;  
  
-   propriétés système et paramètres de sécurité au niveau système ;  
  
-   données du journal d'exécution des rapports ;  
  
-   clés symétriques, ainsi que connexions et informations d'identification chiffrées pour les sources de données des rapports.  
  
 Étant donné que la base de données du serveur de rapports stocke l'état de l'application ainsi que des données permanentes, vous devez créer une planification de sauvegarde pour cette base de données afin d'éviter toute perte de données. Pour obtenir des recommandations et des instructions sur la sauvegarde de la base de données, consultez [Déplacement des bases de données du serveur de rapports vers un autre ordinateur &#40;en mode natif SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
## <a name="report-server-temporary-database"></a>Base de données temporaire du serveur de rapports  
 Chaque base de données de serveur de rapports utilise une base de données temporaire associée pour stocker les données d'exécution et de session, les rapports mis en cache et les tables de travail qui sont générées par le serveur de rapports. Les processus serveur d'arrière-plan supprimeront périodiquement des éléments plus anciens et inutilisés des tables dans la base de données temporaire.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne recrée pas la base de données temporaire si elle est manquante, pas plus qu’il ne répare les tables manquantes ou modifiées. Bien qu'une base de données temporaire ne contienne pas de données permanentes, il est conseillé malgré tout d'en sauvegarder un exemplaire pour éviter d'être obligé de la recréer lors d'une opération de récupération suite à une défaillance majeure.  
  
 Si vous sauvegardez la base de données temporaire et la restaurez ensuite, il est recommandé d'en supprimer le contenu. En règle générale, il n'est pas risqué de supprimer le contenu de la base de données temporaire à quelque moment que ce soit. Toutefois, vous devez redémarrer le service Windows Report Server après avoir supprimé le contenu.  
  
## <a name="see-also"></a> Voir aussi  
 [Héberger une base de données du serveur de rapports dans un cluster de basculement SQL Server](../../reporting-services/install-windows/host-a-report-server-database-in-a-sql-server-failover-cluster.md)   
 [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)   
 [Serveur de rapports Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)   
 [Administrer une base de données du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)   
 [Créer une base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)   
 [Opérations de sauvegarde et de restauration pour Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md)  
  
  
