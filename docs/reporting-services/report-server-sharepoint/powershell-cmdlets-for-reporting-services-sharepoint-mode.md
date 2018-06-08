---
title: Applets de commande PowerShell pour le mode SharePoint de Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2ccff01afbd9e51f0754ceaecf885b36a5b28f9b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="powershell-cmdlets-for-reporting-services-sharepoint-mode"></a>Applets de commande PowerShell pour le mode SharePoint de Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Lorsque vous installez le mode SharePoint de SQL Server 2016 Reporting Services, les applets de commande PowerShell sont installées afin de prendre en charge les serveurs de rapports en mode SharePoint. Les applets de commande couvrent trois catégories de fonctionnalités.  
  
-   Installation du service partagé et du proxy Reporting Services SharePoint.  
  
-   Configuration et gestion des applications de service Reporting Services et des proxys associés.  
  
-   Gestion des fonctionnalités de Reporting Services, comme les extensions et les clés de chiffrement.  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="cmdlet-summary"></a>Résumé des applets de commande

 Pour exécuter les applets de commande, vous devez ouvrir SharePoint Management Shell. Vous pouvez aussi utiliser l’éditeur d’interface utilisateur graphique fourni avec Microsoft Windows : **l’Environnement d’écriture de scripts intégré de Windows PowerShell (ISE)**. Pour plus d’informations, consultez [Démarrage de Windows PowerShell sur Windows Server](http://technet.microsoft.com/library/hh847814.aspx). Dans les résumés suivants d’applets de commande, les références à l’application de service « bases de données » font référence à toutes les bases de données créées et utilisées par une application de service Reporting Services. Cela inclut la configuration, la définition d'alertes et les bases de données temp.  
  
 Si vous voyez un message d'erreur semblable au suivant lorsque vous tapez les exemples PowerShell :  
  
-   Install-SPRSService : Le terme 'Install-SPRSService' n'est pas reconnu comme  
    nom d'applet de commande, fonction, fichier de script ou programme exécutable. Vérifiez l'orthographe du nom, ou si un chemin d'accès existe, vérifiez que le chemin d'accès est correct et réessayez.  
  
 Un des problèmes suivants se produit :  
  
-   Le mode SharePoint de Reporting Services n’étant pas installé, les applets de commande Reporting Services ne sont pas installées.  
  
-   Vous avez exécuté la commande PowerShell dans Windows PowerShell ou Windows PowerShell ISE au lieu de SharePoint Management Shell. Utilisez SharePoint Management Shell ou ajoutez le composant logiciel enfichable SharePoint à la fenêtre Windows PowerShell à l'aide de la commande suivante :  
  
    ```  
    Add-PSSnapin Microsoft.SharePoint.PowerShell  
    ```  
  
 Pour plus d’informations, consultez [Utiliser Windows PowerShell pour administrer SharePoint 2013](http://technet.microsoft.com/library/ee806878.aspx).  
  
### <a name="open-the-sharepoint-management-shell-and-run-cmdlets"></a>Pour ouvrir SharePoint Management Shell et exécuter les applets de commande
  
1.  Cliquez sur le bouton **Démarrer** .  
  
2.  Cliquez sur le groupe **Produits Microsoft SharePoint** .  
  
3.  Cliquez sur **SharePoint Management Shell**.  
  
 Pour consulter l'aide sur la ligne de commande pour une applet de commande utilisez la commande « GET-HELP » de PowerShell à l'invite de commandes de PowerShell. Exemple :  
  
 `Get-Help Get-SPRSServiceApplicationServers`  
  
##  <a name="shared-service-and-proxy-cmdlets"></a>Applets de commande de service partagé et de proxy

 Le tableau suivant contient les applets de commande PowerShell pour le service partagé Reporting Services SharePoint.  
  
|Applet de commande|Description|  
|------------|-----------------|  
|Install-SPRSService|Installe et enregistre, ou désinstalle, le service partagé Reporting Services. Cela peut être effectué uniquement sur l’ordinateur où SQL Server Reporting Services en mode SharePoint est installé. Pour l'installation, deux opérations ont lieu :<br /><br /> - Le service Reporting Services est installé dans la batterie de serveurs.<br /><br /> - L’instance du service Reporting Services est installée sur l’ordinateur actuel.<br /><br /> Pour la désinstallation, deux opérations ont lieu :<br /><br /> - Le service Reporting Services est désinstallé de l’ordinateur actuel.<br /><br /> - Le service Reporting Services est désinstallé de la batterie de serveurs.<br /><br /> <br /><br /> Si le service Reporting Services est installé sur d’autres ordinateurs de la batterie de serveurs ou si des applications de service Reporting Services sont encore en cours d’exécution dans la batterie, un message d’avertissement s’affiche.|  
|Install-SPRSServiceProxy|Installe et enregistre, ou désinstalle, le proxy du service Reporting Services dans la batterie de serveurs SharePoint.|  
|Get-SPRSProxyUrl|Obtient la ou les URL d'accès au service Reporting Services.|  
|Get-SPRSServiceApplicationServers|Obtient tous les serveurs dans la batterie SharePoint locale qui contiennent une installation du service partagé Reporting Services. Cette applet de commande est utile pour les mises à niveau de Reporting Services car elle permet de déterminer quels serveurs exécutent le service partagé et doivent, par conséquent, être mis à niveau.|  
  
## <a name="service-application-and-proxy-cmdlets"></a>Applets de commande d’application de service et de proxy

 Le tableau suivant contient les applets de commande PowerShell pour les applications de service Reporting Services et leurs proxys associés.  
  
|Applet de commande|Description|  
|------------|-----------------|  
|Get-SPRSServiceApplication|Obtient un ou plusieurs objets d’application de service Reporting Services.|  
|New-SPRSServiceApplication|Crée une application de service Reporting Services et des bases de données associées.<br /><br /> Paramètre LogonType : spécifie si le serveur de rapports utilise le compte de pool d'applications SSRS ou un compte de connexion SQL Server pour accéder à la base de données du serveur de rapports. Les valeurs valides sont :<br /><br /> 0 Authentification Windows<br /><br /> 1 SQL Server<br /><br /> 2. Compte du pool d'applications (valeur par défaut)|  
|Remove-SPRSServiceApplication|Supprime l'application de service Reporting Services spécifiée. Cela supprimera également les bases de données associées.|  
|Set-SPRSServiceApplication|Modifie les propriétés d'une base de données d'application de service Reporting Services existante.|  
|New-SPRSServiceApplicationProxy|Crée un nouveau proxy d'application de service Reporting Services.|  
|Get-SPRSServiceApplicationProxy|Obtient un ou plusieurs proxys d'application de service Reporting Services existante.|  
|Dismount-SPRSDatabase|Démonte les bases de données d’une application de service Reporting Services.|  
|Remove-SPRSDatabase|Supprime les bases de données d’une application de service Reporting Services.|  
|Set-SPRSDatabase|Définit les propriétés des bases de données associées à une application de service Reporting Services.|  
|Mount-SPRSDatabase|Monte les bases de données d’une application de service Reporting Services.|  
|New-SPRSDatabase|Crée de nouvelles bases de données pour l’application de service Reporting Services spécifiée.|  
|Get-SPRSDatabaseCreationScript|Affiche à l’écran le script de création de base de données pour une application de service Reporting Services. Vous pouvez ensuite exécuter le script dans SQL Server Management Studio.|  
|Get-SPRSDatabase|Obtient une ou plusieurs bases de données d'application de service Reporting Services. Utilisez la commande pour obtenir l'ID de la base de données d'application de service afin d'utiliser l'applet de commande Set-SPRSDatabase pour modifier les propriétés, par exemple `querytimeout`. Consultez l’exemple de cette rubrique, [Propriétés Get et Set de la base de données d’application Reporting Service](#bkmk_example_db_properties).|  
|Get-SPRSDatabaseRightsScript|Affiche à l’écran le script de droits de base de données pour une application de service Reporting Services. Invite à entrer l'utilisateur et la base de données souhaités, puis retourne un script Transact-SQL que vous pouvez exécuter pour modifier des autorisations. Vous pouvez ensuite exécuter ce script dans SQL Server Management Studio.|  
|Get-SPRSDatabaseUpgradeScript|Génère un script de mise à niveau de base de données à l'écran. Le script met à niveau les bases de données de l’application de service Reporting Services vers la version de base de données de l’installation actuelle de Reporting Services.|  
  
## <a name="reporting-services-custom-functionality-cmdlets"></a>Applets de commande de fonctionnalités personnalisées Reporting Services
  
|Applet de commande|Description|  
|------------|-----------------|  
|Update-SPRSEncryptionKey|Met à jour la clé de chiffrement de l'application de service Reporting Services spécifiée et re-chiffre ses données.|  
|Restore-SPRSEncryptionKey|Restaure une clé de chiffrement précédemment sauvegardée pour une application de service Reporting Services.|  
|Remove-SPRSEncryptedData|Supprime les données chiffrées de l'application de service Reporting Services spécifiée.|  
|Backup-SPRSEncryptionKey|Sauvegarde la clé de chiffrement de l'application de service Reporting Services spécifiée.|  
|New-SPRSExtension|Enregistre une nouvelle extension avec une application de service Reporting Services.|  
|Set-SPRSExtension|Définit les propriétés d'une extension Reporting Services existante.|  
|Remove-SPRSExtension|Supprime une extension d'une application de service Reporting Services.|  
|Get-SPRSExtension|Obtient une ou plusieurs extensions Reporting Services pour une application de service Reporting Services.<br /><br /> Les valeurs valides sont :<br /><br /> <br /><br /> Remise<br /><br /> DeliveryUI<br /><br /> Render<br /><br /> data<br /><br /> Sécurité<br /><br /> Authentification<br /><br /> EventProcessing<br /><br /> ReportItems<br /><br /> Concepteur<br /><br /> ReportItemDesigner<br /><br /> ReportItemConverter<br /><br /> ReportDefinitionCustomization|  
|Get-SPRSSite|Obtient les sites SharePoint en fonction de l'activation de la fonction « ReportingService ». Par défaut, les sites qui activent la fonction « ReportingService » sont retournés.|  
  
## <a name="basic-samples"></a>Exemples de base

 Retournez la liste des applets de commande qui contiennent « SPRS » dans le nom. La liste complète des applets de commande Reporting Services sera retournée.  
  
```  
Get-command –noun *SPRS*  
```  
  
 Ou bien, vous serez redirigés vers un fichier texte nommé commandlist.txt contenant plus de détails.  
  
```  
Get-command -noun *SPRS* | Select name, definition | Format-List | Out-File c:\commandlist.txt  
```  
  
 Installer le service Reporting Services SharePoint et ses proxys.  
  
```  
Install-SPRSService  
```  
  
```  
Install-SPRSServiceProxy  
```  
  
 Démarrer le service Reporting Services  
  
```  
get-spserviceinstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
```  
  
 Tapez la commande suivante à partir de SharePoint Management Shell pour retourner une liste filtrée des lignes du fichier journal. La commande filtre les lignes qui contiennent « ssrscustomactionerror ». Cet exemple consulte le fichier journal créé lorsque le fichier rssharepoint.msi a été installé.  
  
```  
Get-content -path C:\Users\testuser\AppData\Local\Temp\rs_sp_0.log | select-string "ssrscustomactionerror"  
```  
  
## <a name="detailed-samples"></a>Exemples détaillés

 Outre les exemples suivants, consultez la section « Script Windows PowerShell » dans la rubrique [Windows PowerShell script for Steps 1–4](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_full_script).  
  
### <a name="create-a-reporting-services-service-application-and-proxy"></a>Créer une application de service Reporting Services et un proxy

 Cet exemple de script effectue les tâches suivantes :  
  
1.  Création d'une application de service Reporting Services et d'un proxy. Le script suppose que le pool d'applications « My App Pool » existe déjà.  
  
2.  Ajout du proxy au groupe de proxy par défaut  
  
3.  Octroi de l'accès aux applications de service à la base de données de contenus de l'application Web sur le port 80. Le script considère que le site `http://sitename` existe déjà.  
  
```  
# Create service application and service application proxy  
$appPool = Get-SPServiceApplicationPool “My App Pool”  
$serviceApp = New-SPRSServiceApplication “My RS Service App” –ApplicationPool $appPool  
$serviceAppProxy = New-SPRSServiceApplicationProxy –Name “My RS Service App Proxy” –ServiceApplication $serviceApp  
  
# Add service application proxy to default proxy group.  Any web application that uses the default proxy group will now be able to use this service application.  
Get-SPServiceApplicationProxyGroup –default | Add-SPServiceApplicationProxyGroupMember –Member $serviceAppProxy  
  
# Grant application pool account access to the port 80 web application’s content database.  
$webApp = Get-SPWebApplication “http://sitename”  
$appPoolAccountName = $appPool.ProcessAccount.LookupName()  
$webApp.GrantAccessToProcessIdentity($appPoolAccountName)  
  
```  
  
### <a name="review-and-update-a-reporting-services-delivery-extension"></a>Examiner et mettre à jour une extension de remise Reporting Services

 L’exemple de script PowerShell suivant met à jour la configuration de l’extension de remise du courrier électronique par le serveur de rapports pour l’application de service nommée `My RS Service App`. Mettez à jour les valeurs du serveur SMTP (`<email server name>`) et de l’alias de messagerie électronique FROM (`<your FROM email address>`).  
  
```  
$app=get-sprsserviceapplication -Name "My RS Service App"  
$emailCfg = Get-SPRSExtension -identity $app -ExtensionType "Delivery" -name "Report Server Email" | select -ExpandProperty ConfigurationXml   
$emailXml = [xml]$emailCfg   
$emailXml.SelectSingleNode("//SMTPServer").InnerText = “<email server name>”  
$emailXml.SelectSingleNode("//SendUsing").InnerText = "2"  
$emailXml.SelectSingleNode("//SMTPAuthenticate").InnerText = "2"  
$emailXml.SelectSingleNode("//From").InnerText = '<your FROM email address>'  
Set-SPRSExtension -identity $app -ExtensionType "Delivery" -name "Report Server Email" -ExtensionConfiguration $emailXml.OuterXml  
```  
  
 Dans l'exemple ci-dessus, si vous ne connaissiez pas le nom exact de l'application de service, vous pouvez réécrire la première instruction pour obtenir l'application de service en fonction d'une recherche portant sur son nom partiel. Exemple :  
  
```  
$app=get-sprsserviceapplication | where {$_.name -like " ssrs_testapp *"}  
```  
  
 Le script suivant retourne les valeurs de configuration actuelles pour l'extension de remise de courrier électronique du serveur de rapports pour l'application de service nommée « Application Reporting Services ». La première étape définit la valeur de la variable $app pour l'objet de l'application de service dont le nom est « My RS Service App ».  
  
 La deuxième instruction obtiendra l'extension de remise « Report Server Email » pour l'objet d'application de service dans la variable $app, puis sélectionne le configuration XML.  
  
```  
$app=get-sprsserviceapplication –Name "Reporting Services Application"  
Get-SPRSExtension -identity $app -ExtensionType "Delivery" -name "Report Server Email" | select -ExpandProperty ConfigurationXml  
```  
  
 Vous pouvez réécrire les deux instructions ci-dessus dans une seule :  
  
```  
get-sprsserviceapplication –Name "Reporting Services Application" | Get-SPRSExtension -ExtensionType "Delivery" -name "Report Server Email" | select -ExpandProperty ConfigurationXml  
```  
  
### <a name="get-and-set-properties-of-the-reporting-service-application-database"></a>Obtenir et définir les propriétés de la base de données d’application Reporting Services

 L'exemple suivant renvoie d'abord une liste des bases de données et propriétés afin de déterminer le guid (ID) de base de données que vous fournissez à la commande. Pour obtenir une liste complète des propriétés, utilisez `Get-SPRSDatabase | format-list`.  
  
```  
get-SPRSDatabase | select id, querytimeout,connectiontimeout, status, server, ServiceInstance   
```  
  
 Voici un exemple de sortie. Déterminez l'ID de la base de données que vous voulez modifier et utilisez l'ID de l'applet de commande SET.  
  
-   `Id                : 56f8d1bc-cb04-44cf-bd41-a873643c5a14`  
  
     `QueryTimeout      : 120`  
  
     `ConnectionTimeout : 15`  
  
     `Status            : Online`  
  
     `Server            : SPServer Name=uetestb01`  
  
     `ServiceInstance   : SPDatabaseServiceInstance`  
  
```  
Set-SPRSDatabase –identity 56f8d1bc-cb04-44cf-bd41-a873643c5a14 -QueryTimeout 300  
```  
  
 Pour vérifier que la valeur est définie, réexécutez l'applet de commande GET.  
  
```  
Get-SPRSDatabase –identity 56f8d1bc-cb04-44cf-bd41-a873643c5a14 | select id, querytimeout,connectiontimeout, status, server, ServiceInstance  
```  
  
### <a name="list-reporting-services-data-extensions"></a>Répertorier les extensions de données Reporting Services

 L’exemple suivant effectue une itération sur chaque application de service Reporting Services et répertorie pour chacune les extensions de données actuelles.  
  
```  
$apps = Get-SPRSServiceApplication  
foreach ($app in $apps)   
{  
Write-host -ForegroundColor "yellow" Service App Name $app.Name  
Get-SPRSExtension -identity $app -ExtensionType “Data” | select name,extensiontype | Format-Table -AutoSize  
}  
```  
  
 **Exemple de sortie :**  
  
-   `Name           ExtensionType`  
  
     `----           -------------`  
  
     `SQL                     Data`  
  
     `SQLAZURE                Data`  
  
     `SQLPDW                  Data`  
  
     `OLEDB                   Data`  
  
     `OLEDB-MD                Data`  
  
     `ORACLE                  Data`  
  
     `ODBC                    Data`  
  
     `XML                     Data`  
  
     `SHAREPOINTLIST          Data`  
  
### <a name="change-and-list-reporting-services-subscription-owners"></a>Modifier et répertorier les propriétaires d’abonnements Reporting Services

 Consultez [Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md).  
  
## <a name="next-steps"></a>Étapes suivantes

[Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md)  
[Liste de vérification : utiliser PowerShell pour vérifier Power Pivot pour SharePoint](../../analysis-services/instances/install-windows/checklist-use-powershell-to-verify-power-pivot-for-sharepoint.md)   
[Obtenir de l’aide sur SQL Server PowerShell](../../relational-databases/scripting/get-help-sql-server-powershell.md)   

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
