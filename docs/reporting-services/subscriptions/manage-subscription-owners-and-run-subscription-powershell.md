---
title: Gérer les propriétaires d’abonnement et exécuter un abonnement - PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 03/24/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 0fa6cb36-68fc-4fb8-b1dc-ae4f12bf6ff0
caps.latest.revision: 21
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 149c5e9142dc87cc538b0ae6521d0754b5d03d42
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34585521"
---
# <a name="manage-subscription-owners-and-run-subscription---powershell"></a>Gérer les propriétaires d’abonnement et exécuter un abonnement - PowerShell
[!INCLUDE[feedback_stackoverflow_msdn_connect_md](../../includes/feedback-stackoverflow-msdn-connect-md.md)]

  À compter de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous pouvez transférer par programmation la propriété d’un abonnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] d’un utilisateur à un autre. Cette rubrique fournit plusieurs scripts Windows PowerShell que vous pouvez utiliser pour modifier ou simplement dresser la liste des appartenances aux abonnements. Chaque exemple comprend un exemple de syntaxe pour le mode Natif et le mode SharePoint. Une fois que vous avez modifié le propriétaire d'un abonnement, celui-ci s'exécute dans le contexte de sécurité du nouveau propriétaire et le champ User!UserID du rapport affiche la valeur du nouveau propriétaire. Pour plus d’informations sur le modèle objet appelé par les exemples PowerShell, consultez <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A>  
  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell")  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif &#124; [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint|  
  
##  <a name="bkmk_top"></a> Dans cette rubrique :  
  
-   [Pour utiliser les scripts](#bkmk_how_to)  
  
-   [Script : dresser la liste des propriétaires de tous les abonnements](#bkmk_list_ownership_all)  
  
-   [Script : dresser la liste de tous les abonnements détenus par un utilisateur spécifique](#bkmk_list_all_one_user)  
  
-   [Script : modifier la propriété de tous les abonnements détenus par un utilisateur spécifique](#bkmk_change_all)  
  
-   [Script : dresser la liste de tous les abonnements associés à un rapport spécifique](#bkmk_list_for_1_report)  
  
-   [Script : modifier la propriété d'un abonnement spécifique](#bkmk_change_all_1_subscription)  
  
-   [Script : exécuter (déclencher) un seul abonnement](#bkmk_run_1_subscription)  
  
##  <a name="bkmk_how_to"></a> Pour utiliser les scripts  
  
### <a name="permissions"></a>Autorisations  
 Cette section récapitule les niveaux d'autorisation requis pour utiliser chacune des méthodes pour le mode Natif et le mode SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Les scripts de cette rubrique utilisent les méthodes [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] suivantes :  
  
-   [Méthode ReportingService2010.ListSubscriptions](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.listsubscriptions.aspx)  
  
-   [Méthode ReportingService2010.ChangeSubscriptionOwner](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.changesubscriptionowner.aspx)  
  
-   [ReportingService2010.ListChildren](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.listchildren.aspx)  
  
-   La méthode [ReportingService2010.FireEvent](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.fireevent.aspx) est utilisée uniquement dans le dernier script pour déclencher l'exécution d'un abonnement spécifique. Si vous ne prévoyez pas d'utiliser ce script, vous pouvez ignorer les exigences relatives aux autorisations pour la méthode FireEvent.  
  
 **Mode natif :**  
  
-   Dresser la liste des abonnements : [ReportOperation Enumeration](http://msdn.microsoft.com/library/microsoft.reportingservices.interfaces.reportoperation.aspx) sur le rapport ET l’utilisateur est le propriétaire de l’abonnement OU ReadAnySubscription.  
  
-   Modifier les abonnements : l'utilisateur doit être membre du groupe BUILTIN\\Administrateurs  
  
-   Dresser la liste des enfants : ReadProperties sur Item  
  
-   Déclencher un événement : GenerateEvents (Système)  
  
 **Mode SharePoint :**  
  
-   Dresser la liste des abonnements : ManageAlerts OU [CreateAlerts](http://msdn.microsoft.com/library/microsoft.sharepoint.spbasepermissions.aspx) sur le rapport ET l’utilisateur est le propriétaire de l’abonnement et il s’agit d’un abonnement planifié.  
  
-   Modifier des abonnements : ManageWeb  
  
-   Dresser la liste des enfants : ViewListItems  
  
-   Déclencher l'événement : ManageWeb  
  
 Pour plus d'informations, consultez [Comparer des rôles et des tâches dans Reporting Services pour des autorisations et des groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md).  
  
### <a name="script-usage"></a>Utilisation des scripts  
 **Créer les fichiers de scripts (.ps1)**  
  
1.  Créez un dossier nommé **c:\scripts**. Si vous choisissez un autre dossier, modifiez le nom du dossier utilisé dans les exemples d'instructions de syntaxe de ligne de commande.  
  
2.  Créez un fichier texte pour chaque script et enregistrez les fichiers dans le dossier c:\scripts. Lorsque vous créez les fichiers .ps1, utilisez le nom mentionné dans chaque exemple de syntaxe de ligne de commande.  
  
3.  Ouvrez une invite de commandes avec des privilèges d'administrateur.  
  
4.  Exécutez chaque fichier de script, à l'aide de l'exemple de syntaxe de ligne de commande fourni avec chaque exemple.  
  
 **Environnements testés**  
  
 Les scripts de cette rubrique ont été testés sur PowerShell version 3 et avec les versions suivantes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
##  <a name="bkmk_list_ownership_all"></a> Script : dresser la liste des propriétaires de tous les abonnements  
 Ce script dresse la liste de tous les abonnements sur un site. Vous pouvez utiliser ce script pour tester votre connexion ou pour vérifier le chemin d'accès aux rapports et l'ID d'abonnement utilisables dans les autres scripts. Il est également utile pour simplement vérifier quels abonnements existent et qui en est le propriétaire.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/reportserver" "/"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/_vti_bin/reportserver" "http://[server]"  
```  
  
 **Script :**  
  
```  
# Parameters  
#    server   - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
  
Param(  
    [string]$server,  
    [string]$site  
   )  
  
$rs2010 += New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site); # use "/" for default native mode site  
  
Write-Host " "  
Write-Host "----- $server's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted, Status  
```  
  
> [!TIP]  
>  Pour vérifier les URL de site en mode SharePoint, utilisez l’applet de commande SharePoint **Get-SPSite**. Pour plus d’informations, consultez [Get-SPSite](http://msdn.microsoft.com/library/ff607950\(v=office.15\).aspx).  
  
##  <a name="bkmk_list_all_one_user"></a> Script : dresser la liste de tous les abonnements détenus par un utilisateur spécifique  
 Ce script dresse la liste de tous les abonnements détenus par un utilisateur spécifique. Vous pouvez utiliser ce script pour tester votre connexion ou pour vérifier le chemin d'accès aux rapports et l'ID d'abonnement utilisables dans les autres scripts. Il est utile en cas de départ d'un employé de votre organisation, si vous souhaitez vérifier les abonnements qu'il détenait afin d'en modifier le propriétaire ou de supprimer les abonnements.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]" "[server]/reportserver" "/"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]"  "[server]/_vti_bin/reportserver" "http://[server]"  
```  
  
 **Script :**  
  
```  
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
Param(  
    [string]$currentOwner,  
    [string]$server,  
    [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $currentOwner's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.owner -eq $currentOwner}  
```  
  
##  <a name="bkmk_change_all"></a> Script : modifier la propriété de tous les abonnements détenus par un utilisateur spécifique  
 Ce script affecte un nouveau propriétaire à tous les abonnements détenus par un utilisateur spécifique.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\current owner]" "[Domain]\[new owner]" "[server]/reportserver"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\{current owner]" "[Domain]\[new owner]" "[server]/_vti_bin/reportserver"  
```  
  
 **Script :**  
  
```  
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    newOwner      - DOMAIN\USER that will own the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver, myserver/reportserver_db2, myserver/_vti_bin/reportserver)  
  
Param(  
    [string]$currentOwner,  
    [string]$newOwner,  
    [string]$server  
)  
  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$items = $rs2010.ListChildren("/", $true);  
  
$subscriptions = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $curRepSubs = $rs2010.ListSubscriptions($item.Path);  
        ForEach ($curRepSub in $curRepSubs)  
        {  
            if ($curRepSub.Owner -eq $previousOwner)  
            {  
                $subscriptions += $curRepSub;  
            }  
        }  
    }  
}  
  
Write-Host " "  
Write-Host " "  
Write-Host -foregroundcolor "green" "-----  $currentOwner's Subscriptions changing ownership to $newOwner : "  
$subscriptions | select SubscriptionID, Owner, Path, Description,  Status  | format-table -AutoSize  
  
ForEach ($sub in $subscriptions)  
{  
    $rs2010.ChangeSubscriptionOwner($sub.SubscriptionID, $newOwner);  
}  
  
$subs2 = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $subs2 += $rs2010.ListSubscriptions($item.Path);  
    }  
}  
```  
  
##  <a name="bkmk_list_for_1_report"></a> Script : dresser la liste de tous les abonnements associés à un rapport spécifique  
 Ce script dresse la liste de tous les abonnements associés à un rapport spécifique. La syntaxe du chemin d'accès au rapport est différente en mode SharePoint, car elle nécessite une URL complète. Dans les exemples de syntaxe, le nom du rapport utilisé est « title only », qui contient un espace et nécessite par conséquent de placer le nom du rapport entre des guillemets simples.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/reportserver" "'/reports/title only'" "/"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/_vti_bin/reportserver"  "'http://[server]/shared documents/title only.rdl'" "http://[server]"  
```  
  
 **Script :**  
  
```  
# Parameters:  
#    server      - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    reportpath  - path to report in the report server, including report name e.g. /reports/test report >> pass in  "'/reports/title only'"  
#    site        - use "/" for default native mode site  
Param  
(  
      [string]$server,  
      [string]$reportpath,  
      [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $reportpath 's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.path -eq $reportpath}  
```  
  
##  <a name="bkmk_change_all_1_subscription"></a> Script : modifier la propriété d'un abonnement spécifique  
 Ce script modifie la propriété d'un abonnement spécifique. L'abonnement est identifié par le paramètre SubscriptionID, que vous passez dans le script. Vous pouvez utiliser l'un des scripts de liste d'abonnements pour déterminer le SubscriptionID correct.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/reportserver" "/" "ac5637a1-9982-4d89-9d69-a72a9c3b3150"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/_vti_bin/reportserver" "http://[server]" "9660674b-f020-453f-b1e3-d9ba37624519"  
```  
  
 **Script :**  
  
```  
# Parameters:  
#    newOwner       - DOMAIN\USER that will own the subscriptions you wish to change  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
#    subscriptionID - guid for the single subscription to change  
  
Param(  
    [string]$newOwner,  
    [string]$server,  
    [string]$site,  
    [string]$subscriptionid  
   )  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
  
$subscription += $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid};  
  
Write-Host " "  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
  
$rs2010.ChangeSubscriptionOwner($subscription.SubscriptionID, $newOwner)  
  
#refresh the list  
$subscription = $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid}; # use "/" for default native mode site  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
```  
  
##  <a name="bkmk_run_1_subscription"></a> Script : exécuter (déclencher) un seul abonnement  
 Ce script exécute un abonnement spécifique à l'aide de la méthode FireEvent. Le script exécute immédiatement l'abonnement quelle que soit la planification configurée pour lui. L'EventType est comparé à l'ensemble connu d'événements définis dans le fichier de configuration du serveur de rapports **rsreportserver.config** . Le script utilise le type d'événement suivant pour les abonnements standard :  
  
 `<Event>`  
  
 `<Type>TimedSubscription</Type>`  
  
 `</Event>`  
  
 Pour plus d’informations sur le fichier de configuration, consultez [Fichier de configuration RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
 Le script comprend la logique de délai «`Start-Sleep -s 6`». Il y a donc suffisamment de temps pour que l'état mis à jour soit disponible avec la méthode ListSubscription après le déclenchement de l'événement.  
  
 **Syntaxe en mode natif :**  
  
```  
powershell c:\scripts\FireSubscription.ps1 "[server]/reportserver" $null "70366e82-2d3c-4edd-a216-b97e51e26de9"  
```  
  
 **Syntaxe en mode SharePoint :**  
  
```  
powershell c:\scripts\FireSubscription.ps1 "[server]/_vti_bin/reportserver" "http://[server]" "c3425c72-580d-423e-805a-41cf9799fd25"  
```  
  
 **Script :**  
  
```  
  
# Parameters  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site           - use $null for a native mode server  
#    subscriptionid - subscription guid  
  
Param(  
  [string]$server,  
  [string]$site,  
  [string]$subscriptionid  
  )  
  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
#event type is case sensative to what is in the rsreportserver.config  
$rs2010.FireEvent("TimedSubscription",$subscriptionid,$site)  
  
Write-Host " "  
Write-Host "----- Subscription ($subscriptionid) status: "  
#get list of subscriptions and filter to the specific ID to see the Status and LastExecuted  
Start-Sleep -s 6 # slight delay in processing so ListSubscription returns the updated Status and LastExecuted  
$subscriptions = $rs2010.ListSubscriptions($site);   
$subscriptions | select Status, Path, report, Description, Owner, SubscriptionID, EventType, lastexecuted | where {$_.SubscriptionID -eq $subscriptionid}  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 
[Méthode ReportingService2010.ListSubscriptions](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.listsubscriptions.aspx)  

[Méthode ReportingService2010.ChangeSubscriptionOwner](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.changesubscriptionowner.aspx)   

[ReportingService2010.ListChildren](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.listchildren.aspx)  

[ReportingService2010.FireEvent](http://msdn.microsoft.com/library/reportservice2010.reportingservice2010.fireevent.aspx)
  
  
