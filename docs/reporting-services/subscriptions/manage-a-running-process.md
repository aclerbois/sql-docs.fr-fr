---
title: Gérer un processus en cours d’exécution | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report processing [Reporting Services], status information
- jobs [Reporting Services]
- viewing jobs
- canceling jobs
- user jobs [Reporting Services]
- system jobs [Reporting Services]
- report processing [Reporting Services], managing running processes
- processes [Reporting Services]
- scanning processes [Reporting Services]
- status information [Reporting Services]
- report processing [Reporting Services]
- canceling subscriptions
- report servers [Reporting Services], jobs
- data-driven subscriptions
- displaying jobs
- subscriptions [Reporting Services], running processes
ms.assetid: 473e574e-f1ff-4ef9-bda6-7028b357ac42
caps.latest.revision: 53
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 33dc3d4c22d3eb8ab898e680e33595ea1b83b89d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-a-running-process"></a>Gérer un processus en cours d'exécution
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] analyse l'état des travaux qui s'exécutent sur le serveur de rapports. À intervalles réguliers, le serveur de rapports procède à une analyse des travaux en cours et transmet des informations d'état à la base de données du serveur de rapports ou aux bases de données d'application de service pour le mode SharePoint. Un travail est en cours si l'un des processus suivants est en cours : exécution de la requête sur un serveur de base de données distant ou local, traitement des rapports et rendu de rapport.  
  
 Vous pouvez gérer à la fois les *travaux utilisateur* et les *travaux système*.  
  
-   Les travaux utilisateur sont lancés par un utilisateur individuel ou par un abonnement. Ils comprennent l'exécution d'un rapport à la demande, la demande d'instantané d'un historique de rapport, la création manuelle d'un instantané de rapport et le traitement d'un abonnement standard.  
  
-   Les travaux système sont lancés par le serveur de rapports. Ils comprennent des instantanés d'exécution de rapport planifiés, des instantanés d'historique de rapport planifiés et des abonnements pilotés par les données.  
  
 La durée et l'utilisation des ressources allouées au traitement d'un rapport varient considérablement en fonction du rapport, de la complexité de la requête, de la quantité de données et du format de rendu spécifié pour le rapport. L'exécution des rapports, dont les requêtes à une source de données locale sont simples, n'est qu'une question de millisecondes et ne nécessite ni gestion ni réglage particulier. En revanche, un rapport volumineux dont le rendu est effectué au format PDF ou Excel requiert une durée de traitement plus ou moins importante selon les ressources matérielles, les options de remise et éventuellement l'exécution concomitante de divers autres processus. Sur un serveur de rapports, la plupart des processus caractérisés par une exécution longue sont des opérations de rendu de rapport et des processus en attente d'une fin de traitement de requête. Si nécessaire, vous pouvez de temps à autre annuler un processus de rapport pour mettre l'ordinateur en mode hors connexion ou suspendre un travail dont l'exécution est en cours et tarde à s'achever.  
  
 Les processus suivants peuvent être annulés :  
  
-   Traitement des rapports à la demande.  
  
-   Traitement des rapports planifié.  
  
-   Abonnements standard détenus par des utilisateurs individuels.  
  
 L'annulation d'un travail supprime uniquement les processus qui s'exécutent sur le serveur de rapports. Le serveur de rapports ne gère pas le traitement des données sur d'autres ordinateurs ; par conséquent, vous devez annuler manuellement les processus de requête qui se retrouvent par la suite orphelins sur d'autres systèmes. Envisagez la possibilité d'affecter des valeurs au délai d'expiration des requêtes afin de clore automatiquement les requêtes dont l'exécution est trop longue. Pour plus d’informations, consultez [Définition des valeurs de délai d’attente pour le traitement d’un rapport et d’un dataset partagé &#40;SSRS&#41;](../../reporting-services/report-server/setting-time-out-values-for-report-and-shared-dataset-processing-ssrs.md). Pour plus d’informations sur l’interruption momentanée d’un rapport, consultez [Désactiver ou suspendre le traitement des rapports et des abonnements](../../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).  
  
> [!NOTE]  
>  Dans de rares cas, vous serez peut-être amené à redémarrer le serveur pour annuler un processus. Pour le mode SharePoint, vous devrez peut-être redémarrer le pool d'applications hébergeant l'application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).  
  
 Dans cette rubrique :  
  
-   [Afficher et annuler les travaux (mode natif)](#bkmk_native)  
  
-   [Afficher et annuler les travaux (mode SharePoint)](#bkmk_sharepoint)  
  
-   [Gestion des travaux par programmation](#bkmk_programmatically)  
  
##  <a name="bkmk_native"></a> Afficher et annuler les travaux (mode natif)  
 Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour afficher ou annuler un travail qui est en cours d'exécution sur le serveur de rapports. Vous devez actualiser la page afin de récupérer la liste des travaux en cours d'exécution ou d'obtenir l'état mis à jour des travaux à partir de la base de données du serveur de rapports. Lorsque vous vous connectez à un serveur de rapports dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous pouvez ouvrir un dossier Jobs pour consulter la liste des rapports en cours de traitement sur le serveur de rapports. Les informations d'état de chaque travail sont affichées dans la page Propriétés du travail. Vous pouvez afficher les informations d'état de tous les travaux en ouvrant la boîte de dialogue Annuler les travaux du serveur de rapports.  
  
 Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour afficher ou annuler un travail qui est en cours d'exécution sur le serveur de rapports. Vous devez actualiser la page afin de récupérer la liste des travaux en cours d'exécution ou d'obtenir l'état mis à jour des travaux à partir de la base de données du serveur de rapports. Lorsque vous vous connectez à un serveur de rapports dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous pouvez ouvrir un dossier Jobs pour consulter la liste des rapports en cours de traitement sur le serveur de rapports. Les informations d'état de chaque travail sont affichées dans la page Propriétés du travail. Vous pouvez afficher les informations d'état de tous les travaux en ouvrant la boîte de dialogue Annuler les travaux du serveur de rapports.  
  
 Vous ne pouvez pas utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour répertorier ou annuler la génération de modèle, le traitement de modèle ou les abonnements pilotés par les données. Reporting Services n'offre aucun moyen d'annuler le traitement ou la génération de modèle. Toutefois, vous pouvez annuler les abonnements pilotés par les données à l'aide des instructions fournies dans cette rubrique.  
  
### <a name="how-to-cancel-report-processing-or-subscription"></a>Procédure d'annulation du traitement d'un rapport ou d'un abonnement  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], connectez-vous au serveur de rapports. Pour obtenir des instructions, consultez [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md).  
  
2.  Ouvrez le dossier **Jobs** .  
  
3.  Cliquez avec le bouton droit sur le rapport, puis cliquez sur **Annuler les travaux**.  
  
### <a name="how-to-cancel-a-data-driven-subscription"></a>Procédure d'annulation d'un abonnement piloté par les données  
  
1.  Ouvrez le fichier RSReportServer.config dans un éditeur de texte.  
  
2.  Recherchez **IsNotificationService**.  
  
3.  Affectez-lui la valeur **False**.  
  
4.  Enregistrez le fichier.  
  
5.  Dans le Gestionnaire de rapports, supprimez l’abonnement piloté par les données sous l’onglet Abonnements du rapport ou dans **Mes abonnements**.  
  
6.  Après avoir supprimé l’abonnement, dans le fichier RSReportServer.config, recherchez **IsNotificationService** et affectez-lui la valeur **True**.  
  
7.  Enregistrez le fichier.  
  
### <a name="configuring-frequency-settings-for-retrieving-job-status"></a>Configuration des paramètres de fréquence pour la récupération de l'état des travaux  
 Un travail en cours d'exécution est stocké dans la base de données temporaire du serveur de rapports. Vous pouvez modifier les paramètres de configuration dans le fichier RSReportServer.config pour contrôler la fréquence d'analyse du serveur de rapports sur les travaux en cours et le laps de temps à la suite duquel l'état d'un travail passe de « nouveau » à « en cours d'exécution ». Le paramètre **RunningRequestsDbCycle** spécifie la fréquence à laquelle le serveur de rapports procède à l’analyse des processus en cours d’exécution. Par défaut, les informations d'état sont enregistrées toutes les 60 secondes. Le paramètre **RunningRequestsAge** précise la durée suite à laquelle l’état d’un nouveau travail évolue vers l’état d’exécution en cours.  
  
##  <a name="bkmk_sharepoint"></a> Afficher et annuler les travaux (mode SharePoint)  
 La gestion des travaux d'un déploiement en mode SharePoint s'effectue via l'Administration centrale de SharePoint, pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
#### <a name="to-manage-jobs-in-sharepoint-mode"></a>Pour gérer des travaux en mode SharePoint  
  
1.  Dans l'Administration centrale de SharePoint, cliquez sur **Gérer les applications de service**.  
  
2.  Recherchez le nom de votre application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis cliquez dessus pour ouvrir la page de gestion des applications.  
  
3.  Cliquez sur **Gérer les travaux**  
  
4.  Cliquez sur **ID de travail** pour afficher les détails du travail.  
  
5.  Ou cliquez sur la zone correspondant à votre travail, puis cliquez sur **Supprimer** pour annuler le travail. La suppression du travail n'entraîne pas de suppression de l'abonnement.  
  
##  <a name="bkmk_programmatically"></a> Gestion des travaux par programmation  
 Vous pouvez gérer des travaux par programmation ou au moyen d'un script. Pour plus d’informations, consultez <xref:ReportService2010.ReportingService2010.ListJobs%2A>, <xref:ReportService2010.ReportingService2010.CancelJob%2A>.  
  
## <a name="see-also"></a> Voir aussi  
 [Annuler les travaux du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/cancel-report-server-jobs-management-studio.md)   
 [Propriétés du travail &#40;Management Studio&#41;](../../reporting-services/tools/job-properties-management-studio.md)   
 [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Contrôle des performances d'un serveur de rapports](../../reporting-services/report-server/monitoring-report-server-performance.md)  
  
  
