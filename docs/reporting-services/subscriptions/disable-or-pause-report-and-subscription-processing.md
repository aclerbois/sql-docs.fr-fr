---
title: Désactiver ou suspendre le traitement des rapports et des abonnements | Microsoft Docs
ms.custom: ''
ms.date: 09/29/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- pausing schedules
- subscriptions [Reporting Services], pausing
- report processing [Reporting Services], pausing
- shared data sources [Reporting Services]
- pausing subscription processing
- pausing report processing
- temporarily stopping report processing
- disabling shared data sources
- roles [Reporting Services], modifying
- shared schedules [Reporting Services], pausing
ms.assetid: 3cf9a240-24cc-46d4-bec6-976f82d8f830
caps.latest.revision: 47
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: dfddb0368d8c674c7f0148a395f97088d9143228
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="disable-or-pause-report-and-subscription-processing"></a>Désactiver ou suspendre le traitement des rapports et des abonnements
  Il existe plusieurs approches pour désactiver ou suspendre le traitement des rapports et des abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les approches présentées dans cette rubrique couvrent la désactivation d’un abonnement jusqu’à la suspension de la connexion à la source de données. Certaines approches ne sont pas possibles avec les deux modes serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les tableaux suivants récapitulent les méthodes et les modes serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pris en charge :  
  
##  <a name="bkmk_top"></a> Dans cette rubrique  
  
||Mode serveur pris en charge|  
|-|---------------------------|  
|[Activation et désactivation des abonnements](#bkmk_disable_subscription)|en mode natif|  
|[Suspendre une planification partagée](#bkmk_pause_schedule)|Mode natif et SharePoint|  
|[Désactiver une source de données partagée](#bkmk_disable_shared_datasource)|Mode natif et SharePoint|  
|[Modifier les attributions de rôles afin d'empêcher l’accès à un rapport (mode natif)](#bkmk_modify_role_assignment)|en mode natif|  
|[Supprimer les autorisations de gestion des abonnements d’un rôle (mode natif)](#bkmk_remove_manage_subscriptions_permission)|en mode natif|  
|[Désactiver des extensions de remise](#bkmk_disable_extensions)|Mode natif et SharePoint|  
  
##  <a name="bkmk_disable_subscription"></a> Activation et désactivation des abonnements  
  
> [!TIP]  
>  Nouveauté de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]! **Activation et désactivation des abonnements**. De nouvelles options de l'interface utilisateur vous permettent de rapidement désactiver et activer les abonnements. Les abonnements désactivés conservent leurs autres propriétés de configuration comme la planification, et peuvent être facilement activés. Vous pouvez également programmer l’activation et la désactivation des abonnements ou faire un audit des abonnements désactivés.  
  
 ![ruban des abonnements Reporting Services](../../reporting-services/subscriptions/media/ssrs-subscription-ribbon.png "ruban des abonnements Reporting Services")  
  
 Dans le Gestionnaire de rapports en mode natif, accédez à l'abonnement sur la page **Mes abonnements** ou **Gérer** d'un abonnement individuel. Sélectionnez un ou plusieurs abonnements, puis cliquez sur le bouton de désactivation ![désactiver un abonnement Reporting Services](../../reporting-services/subscriptions/media/ssrs-disable-subscription.png "désactiver un abonnement Reporting Services") ou sur le bouton d’activation ![activer un abonnement Reporting Services](../../reporting-services/subscriptions/media/ssrs-enable-subscription.png "activer un abonnement Reporting Services") dans le ruban. Les abonnements désactivés sont signalés par une icône d’avertissement ![avertissement concernant l’état d’un abonnement Reporting Services](../../reporting-services/subscriptions/media/ssrs-subscription-warning.png "avertissement concernant l’état d’un abonnement Reporting Services") et affichent l’état **Désactivé**.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inscrit une ligne dans le journal [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] lorsqu'un abonnement est désactivé, et une autre entrée lorsque l'abonnement est activé. Par exemple, dans le fichier journal du serveur de rapports :  
  
 `C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\LogFiles\ReportServerService__10_16_2014_00_02_18.log`  
  
 des lignes similaires à ce qui suit apparaissent :  
  
 `library!ReportServer_0-1!b08!10/16/2014-16:21:14:: i INFO: Call to DisableSubscriptionAction(SubscriptionID=e843bc2b-023e-45a3-ba23-22f9dc9a0934)`  
  
 `library!ReportServer_0-1!2eec!10/16/2014-16:44:18:: i INFO: Call to EnableSubscriptionAction(SubscriptionID=e843bc2b-023e-45a3-ba23-22f9dc9a0934).`  
  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") **Utiliser Windows PowerShell pour désactiver un abonnement unique :** utilisez le script PowerShell suivant pour désactiver un abonnement spécifique. Mettez à jour le nom du serveur et l’ID d’abonnement.  
  
```  
#disable specific subscription  
$rs2010 = New-WebServiceProxy -Uri "http://SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptionID = "subscription guid”;  
$rs2010.DisableSubscription($subscriptionID);  
  
```  
  
 Vous pouvez utiliser le script suivant pour répertorier tous les abonnements et leurs ID. Mettez à jour le nom du serveur.  
  
```  
#list all subscriptions  
$rs2010 = New-WebServiceProxy -Uri "http://SERVERNAME /ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
$subscriptions | select subscriptionid, report, status, path  
  
```  
  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") **Utiliser Windows PowerShell pour répertorier tous les abonnements désactivés :** utilisez le script PowerShell suivant pour répertorier tous les abonnements désactivés sur le serveur de rapports en mode natif actuel. Mettez à jour le nom du serveur.  
  
```  
#list all disabled subscriptions  
$rs2010 = New-WebServiceProxy -Uri "http://uetestb03/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
Write-Host "--- Disabled Subscriptions ---";  
Write-Host "----------------------------------- ";  
$subscriptions | Where-Object {$_.Active.DisabledByUserSpecified -and $_.Active.DisabledByUser } | select subscriptionid, report, status, lastexecuted,path | format-table -auto  
```  
  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") **Utiliser Windows PowerShell pour activer tous les abonnements désactivés :** utilisez le script PowerShell suivant pour activer tous les abonnements actuellement désactivés. Mettez à jour le nom du serveur.  
  
```  
#enable all subscriptions  
$rs2010 = New-WebServiceProxy -Uri "http://SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/") | Where-Object {$_.status -eq "disabled" } ;  
ForEach ($subscription in $subscriptions)  
{  
    $rs2010.EnableSubscription($subscription.SubscriptionID);  
    $subscription | select subscriptionid, report, path  
}  
  
```  
  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") **Utiliser Windows PowerShell pour DÉSACTIVER tous les abonnements :** utilisez le script PowerShell suivant pour répertorier **TOUS** les abonnements.  
  
```  
#DISABLE all subscriptions  
$rs2010 = New-WebServiceProxy -Uri "http://SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/") ;  
ForEach ($subscription in $subscriptions)  
{  
    $rs2010.DisableSubscription($subscription.SubscriptionID);  
    $subscription | select subscriptionid, report, path  
}  
```  
  
##  <a name="bkmk_pause_schedule"></a> Suspendre une planification partagée  
 Si un rapport ou un abonnement s'exécute à partir d'une planification partagée, vous pouvez suspendre la planification pour empêcher le traitement. Tous les traitements de rapports et d'abonnements pilotés par la planification sont reportés jusqu'à la reprise de la planification.  
  
-   **Mode SharePoint :** ![Paramètres SharePoint](../../analysis-services/media/as-sharepoint2013-settings-gear.gif "Paramètres SharePoint") Dans **Paramètres du site**, sélectionnez **Gérer les planifications partagées**. Sélectionnez la planification, puis cliquez sur **Suspendre les planifications sélectionnées**.  
  
-   **Mode natif :** dans le gestionnaire de rapports, cliquez sur **Paramètres du site**. Sélectionnez la planification puis cliquez sur **Suspendre**.  
  
##  <a name="bkmk_disable_shared_datasource"></a> Désactiver une source de données partagée  
 L'un des avantages de l'utilisation de sources de données partagées est que vous pouvez désactiver celles-ci pour empêcher l'exécution d'un rapport ou d'un abonnement piloté par les données. La désactivation d'une source de données partagée déconnecte le rapport de sa source externe. Lorsqu'elle est désactivée, la source de données n'est plus disponible pour les rapports et les abonnements qui l'utilisent.  
  
 Notez que le rapport continue à se charger même si la source de données n'est pas disponible. Le rapport ne contient pas de données, mais les utilisateurs dotés des autorisations appropriées peuvent accéder aux pages des propriétés, aux paramètres de sécurité, à l'historique de rapport et aux informations d'abonnement associés à ce rapport.  
  
-   **Mode SharePoint :** pour désactiver une source de données partagée sur un serveur de rapports en mode SharePoint, accédez à la bibliothèque de documents qui contient la source de données. ![Icône de source de données partagée](../../reporting-services/report-data/media/hlp-16datasource.png "Icône de source de données partagée") Cliquez sur la source de données, puis décochez la case **Activer cette source de données**.  
  
-   **Mode natif :** pour désactiver une source de données partagée sur un serveur de rapports en mode natif, ouvrez-la dans le Gestionnaire de rapports et désactivez la case à cocher **Activer cette source de données** .  
  
##  <a name="bkmk_modify_role_assignment"></a> Modifier les attributions de rôles afin d'empêcher l’accès à un rapport (mode natif)  
 Un moyen de rendre un rapport indisponible consiste à supprimer temporairement l'attribution de rôle qui permet d'accéder au rapport. Cette méthode peut s'appliquer à tous les rapports, quelle que soit la façon dont est établie la connexion à la source de données. Elle a une incidence uniquement sur le rapport et n'affecte pas la mise en œuvre des autres rapports ou éléments.  
  
 Pour supprimer une attribution de rôle, ouvrez la page Propriétés de sécurité du rapport dans le Gestionnaire de rapports. Si le rapport hérite de la sécurité d’un parent, cliquez sur **Modifier la sécurité de l’élément** pour créer une stratégie de sécurité restrictive qui supprime les attributions de rôles offrant un accès étendu. Par exemple, vous pouvez supprimer une attribution de rôle qui fournit un accès à Tout le monde et conserver l’attribution de rôle offrant un accès à un petit groupe d’utilisateurs, comme le groupe Administrateurs.  
  
##  <a name="bkmk_remove_manage_subscriptions_permission"></a> Supprimer les autorisations de gestion des abonnements d’un rôle (mode natif)  
 Désactivez la tâche **Gérer les abonnements individuels** du rôle pour retirer aux utilisateurs la possibilité de créer des abonnements. Lorsque vous supprimez cette tâche, les pages Abonnements ne sont plus disponibles. Dans le Gestionnaire de rapports, la page Mes abonnements semble vide (il est impossible de la supprimer) même si elle contenait auparavant des abonnements. La suppression de tâches liées à des abonnements empêche les utilisateurs de créer et de modifier des abonnements, mais elle ne supprime pas les abonnements existants. Ces abonnements continuent de s'exécuter tant qu'ils ne sont pas supprimés. Pour supprimer l'autorisation :  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et con  
  
2.  Connectez-vous au serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
3.  Développez le nœud **Sécurité** .  
  
4.  Sélectionnez le rôle et désactivez la tâche **Gérer les abonnements individuels** .  
  
##  <a name="bkmk_disable_extensions"></a> Désactiver des extensions de remise  
 Toutes les extensions de remise installées sur un serveur de rapports sont disponibles pour tout utilisateur autorisé à créer un abonnement à un rapport donné. Les extensions de remise suivantes sont disponibles et configurées automatiquement :  
  
-   Partage de fichiers Windows  
  
-   Bibliothèque SharePoint (disponible uniquement à partir d'un site SharePoint intégré à un serveur de rapports en mode intégré SharePoint)  
  
 La remise par messagerie électronique doit être configurée avant de pouvoir être utilisée. Si vous ne la configurez pas, elle n'est pas disponible. Pour en savoir plus, consultez [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83).  
  
 Si vous souhaitez désactiver des extensions spécifiques, vous pouvez supprimer les entrées d'extension appropriées dans le fichier **RSReportServer.config** . Pour plus d’informations, consultez [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md) et [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83).  
  
 Lorsqu'une extension de remise est supprimée, elle n'est plus disponible dans le Gestionnaire de rapports, ni dans un site SharePoint. La suppression d'une extension de remise peut engendrer des abonnements inactifs. Avant de supprimer une extension, prenez soin de supprimer ces abonnements ou configurez-les pour qu'ils utilisent une autre extension de remise.  
  
## <a name="see-also"></a> Voir aussi  
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Configurer le Gestionnaire de rapports &#40;mode natif&#41;](../../reporting-services/report-server/configure-report-manager-native-mode.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Propriétés de sécurité, éléments de la page &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/351b8503-354f-4b1b-a7ac-f1245d978da0)  
  
  
