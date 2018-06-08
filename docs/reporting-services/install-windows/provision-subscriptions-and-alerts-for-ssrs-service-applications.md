---
title: Mettre en service les abonnements et les alertes pour les applications de service SSRS | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Shared Service
- SharePoint Mode [Reporting Services]
- SharePoint Mode
- Reporting Services Service Application
- SSRS service application
ms.assetid: d0de3f1f-4887-47fb-bacf-46aaad74c4be
caps.latest.revision: 20
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: a4de22aefed2d4602e5ca331355ae7588394672e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="provision-subscriptions-and-alerts-for-ssrs-service-applications"></a>Configurer les abonnements et les alertes pour les applications de service de SSRS
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Les abonnements et les alertes de données nécessitent SQL Server Agent et peuvent exiger la configuration des autorisations de SQL Server Agent. Si des messages d'erreur apparaissent indiquant que SQL Server Agent est obligatoire et que vous avez vérifié le fonctionnement de SQL Server Agent, alors vous devez mettre à jour ou vérifier les autorisations. Cette rubrique traite de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint et décrit trois méthodes pour mettre à jour les autorisations de SQL Server Agent avec les abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les informations d'identification que vous utilisez pour les étapes de cette rubrique doivent disposer d'autorisations suffisantes pour accorder des autorisations EXECUTE au rôle RSExecRole pour les objets dans les bases de données de l'application de service, msdb et master.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** SharePoint 2016 &#124 SharePoint 2013|  
  
 ![Autorisations d’accès de l’Agent SQL aux bases de données d’application de service](../../reporting-services/install-windows/media/rs-provisionsqlagent.gif "Autorisations d’accès de l’Agent SQL aux bases de données d’application de services")  
  
||Description|  
|------|-----------------|  
|**1**|Instance du moteur de base de données SQL Server qui héberge les bases de données d'application de service Reporting Services.|  
|**2**|L'instance d'agent SQL Server pour l'instance du moteur de base de données SQL.|  
|**3**|Les bases de données d'application de service Reporting Services. Les noms sont formés sur la base des informations utilisées lors de la création de l'application de service. Vous trouverez ci-dessous des exemples de noms de bases de données :<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0_Alerting<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0TempDB|  
|**4**|Base de données master et MSDB de l'instance du moteur de base de données SQL.|  
  
 Recourez à l'une des trois méthodes suivantes pour mettre à jour les autorisations :  
  
1.  Dans la page **Configurer les abonnements et les alertes** , entrez les informations d’identification, puis cliquez sur **OK**.  
  
2.  Dans la page Configurer les abonnements et les alertes, cliquez sur le bouton **Télécharger le script** pour télécharger un script Transact-SQL qui peut être utilisé pour configurer les autorisations.  
  
3.  Exécutez une applet de commande PowerShell pour générer un script Transact-SQL qui peut être utilisé pour configurer les autorisations.  
  
### <a name="to-update-permissions-using-the-provision-page"></a>Pour mettre à jour les autorisation dans la page de configuration  
  
1.  Depuis l’Administration centrale de SharePoint, dans le groupe **Gestion des applications** , cliquez sur **Gérer les applications de service**.  
  
2.  Repérez votre application de service dans la liste et cliquez sur son nom ou cliquez sur la colonne **Type** pour sélectionner l’application de services et cliquez sur le bouton **Gérer** dans le ruban SharePoint.  
  
3.  Dans la page **Gérer l’application Reporting Services** , cliquez sur **Configurer les abonnements et les alertes**.  
  
4.  Si l'administrateur SharePoint possède assez de privilèges sur la base de données MASTER et les bases de données d'application de service, entrez ces informations de connexion.  
  
5.  Cliquez sur le bouton **OK** .  
  
##  <a name="bkmk_download"></a> Pour télécharger le script Transact-SQL  
  
1.  Depuis l’Administration centrale de SharePoint, dans le groupe **Gestion des applications** , cliquez sur **Gérer les applications de service**.  
  
2.  Repérez votre application de service dans la liste et cliquez sur son nom ou cliquez sur la colonne **Type** pour sélectionner l’application de services et cliquez sur le bouton **Gérer** dans le ruban SharePoint.  
  
3.  Dans la page **Gérer l’application Reporting Services** , cliquez sur **Configurer les abonnements et les alertes**.  
  
4.  Dans la zone **Afficher l’état** , vérifiez que l’Agent SQL Server s’exécute.  
  
5.  Cliquez sur **Télécharger le script** pour télécharger un script Transact-SQL que vous pouvez exécuter dans SQL Server Management Studio pour accorder des autorisations. Le nom du fichier de script créé contient le nom de l’application de service Reporting Services, par exemple **[nom de l’application de service]-GrantRights.sql**.  
  
### <a name="to-generate-the-transact-sql-statement-with-powershell"></a>Pour générer l'instruction Transact-SQL avec PowerShell  
  
1.  Vous pouvez également utiliser une applet de commande Windows PowerShell dans SharePoint 2016 ou SharePoint 2013 Management Shell pour créer le script Transact-SQL.  
  
2.  Dans le menu **Démarrer** , cliquez sur **Tous les programmes**.  
  
3.  Développez **Produits Microsoft SharePoint 2016** et cliquez sur **SharePoint 2016 Management Shell**.
  
4.  Mettez à jour l'applet de commande PowerShell suivant en remplaçant le nom de la base de données du serveur de rapports, le compte du pool d'applications, et le chemin d'accès de l'instruction.  
  
     **Syntaxe d’applets de commande :** `Get-SPRSDatabaseRightsScript –DatabaseName <ReportingServices database name> -UserName <app pool account> -IsWindowsUser | Out-File <path of statement>`  
  
     **Exemple d'une applet de commande :** `Get-SPRSDatabaseRightsScript –DatabaseName ReportingService_46fd00359f894b828907b254e3f6257c –UserName “NT AUTHORITY\NETWORK SERVICE” –IsWindowsUser | Out-File c:\SQLServerAgentrights.sql`  
  
## <a name="using-the-transact-sql-script"></a>Utilisation du script Transact-SQL  
 Les procédures suivantes peuvent être utilisées avec des scripts téléchargés depuis les pages de configuration ou des scripts créés à l'aide de PowerShell.  
  
#### <a name="to-load-the-transact-sql-script-in-sql-server-management-studio"></a>Pour charger le script Transact-SQL dans SQL Server Management Studio  
  
1.  Pour ouvrir SQL Server Management Studio, dans le menu **Démarrer** , cliquez sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)] , puis sur **SQL Server Management Studio**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , définissez les options suivantes :  
  
    -   Dans la liste **Type de serveur** , sélectionnez **Moteur de base de données**.  
  
    -   Dans **Nom du serveur**, tapez le nom de l’instance de SQL Server sur laquelle vous souhaitez configurer l’Agent SQL Server.  
  
    -   Sélectionnez un mode d'authentification.  
  
    -   Si vous utilisez l'authentification SQL Server pour vous connecter, vous devez fournir un ID de conenxion et un mot de passe.  
  
3.  Cliquez sur **Se connecter**.  
  
#### <a name="to-run-the-transact-sql-statement"></a>Pour exécuter l'instruction Transact-SQL  
  
1.  Dans la barre d’outils de SQL Server Management Studio, cliquez sur **Nouvelle requête**.  
  
2.  Dans le menu **Fichier** , cliquez sur **Ouvrir**, puis sur **Fichier**.  
  
3.  Accédez au dossier où vous avez enregistré l’instruction Transact-SQL que vous avez générée dans SharePoint 2016 ou SharePoint 2013 Management Shell.  
  
4.  Cliquez sur le fichier, puis sur **Ouvrir**.  
  
     L'instruction est ajoutée dans la fenêtre de requête.  
  
5.  Cliquez sur **Exécuter**.  
  
  
