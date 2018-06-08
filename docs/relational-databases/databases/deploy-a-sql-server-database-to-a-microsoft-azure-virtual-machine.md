---
title: Déployer une base de données SQL Server sur une machine virtuelle Microsoft Azure | Microsoft Docs
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.deploymentwizard.deploymentsettings.f1
- sql13.swb.deploymentwizard.sourcesettings.f1
- sql13.swb.deploymentwizard.summary.f1
- sql13.swb.agdashboard.agp9virtualnw.issues.f1
- sql13.swb.deploymentwizard.f1
- sql13.swb.deploymentwizard.progress.f1
- sql13.swb.usevmdialog.f1
- sql13.swb.newvmdialog.f1
- sql13.swb.sqlvmdialog.f1
- sql13.swb.deploymentwizard.results.f1
- sql13.swb.deploymentwizard.azuresignin.f1
helpviewer_keywords:
- Deploy a database
- Deploy to Azure VM
- Migrate to Azure
- Windows Azure virtual machine
- Migrate to Azure VM
- Migrate to the cloud
- SQL Server Management Studio
- SSMS
- Deploy database wizard
- Deploy a SQL Server database to Azure
- Azure VM
ms.assetid: 5e82e66a-262e-4d4f-aa89-39cb62696d06
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3ad29084eda4f085e090ec9f436a5dd3f170429f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-a-sql-server-database-to-a-microsoft-azure-virtual-machine"></a>Déployer une base de données SQL Server sur une machine virtuelle Microsoft Azure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’Assistant **Déployer une base de données sur un ordinateur virtuel Microsoft Azure** pour déployer une base de données d’une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une machine virtuelle Microsoft Azure. L’Assistant utilise une sauvegarde complète de la base de données ; par conséquent, il copie toujours le schéma complet de la base de données et les données d’une base de données utilisateur SQL Server. L'Assistant effectue également toutes les configurations de machine virtuelle Windows Azure pour vous ; par conséquent, aucune configuration préalable de machine virtuelle n'est requise.  
  
 Vous ne pouvez pas utiliser l’Assistant pour effectuer des sauvegardes différentielles. L’Assistant ne remplacera pas une base de données existante qui porte le même nom de base de données. Pour remplacer une base de données existante sur la machine virtuelle, vous devez d'abord supprimer la base de données existante ou modifier le nom de la base de données. S'il existe un conflit de noms entre le nom de la base de données d'une opération de déploiement en cours et d'une base de données existante sur la machine virtuelle, l'Assistant suggère un nom de base de données avec suffixe pour la base de données en cours pour vous permettre d'effectuer cette opération.  
  
-   [Avant de commencer](#before_you_begin)  
  
-   [Limitations et restrictions](#limitations)  
  
-   [Éléments à prendre en considération pour déployer une base de données FILESTREAM](#filestream)  
  
-   [Éléments à prendre en considération pour la répartition géographique des actifs](#geography)  
  
-   [Assistant Paramètres de configuration](#configuration_settings)  
  
-   [Autorisations requises](#permissions)  
  
-   [Lancement de l'Assistant](#launch_wizard)  
  
-   [Pages de l'assistant](#wizard_pages)  
  
> [!NOTE]  
>  Pour obtenir des instructions détaillées sur l’exécution de cet Assistant, consultez [Migrer une base de données SQL Server vers SQL Server dans une machine virtuelle Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-migrate-onpremises-database/).  
  
##  <a name="before_you_begin"></a> Avant de commencer  
 Pour exécuter cet Assistant, vous devez être en mesure de fournir les informations suivantes et avoir défini ces paramètres de configuration :  
  
-   Informations de compte Microsoft associées à l'abonnement Windows Azure.  
  
-   Votre profil de publication Windows Azure.  
  
    > [!CAUTION]  
    >  SQL Server prend actuellement en charge la version 2.0 du profil de publication. Pour télécharger la version prise en charge du profil de publication, consultez [Télécharger le profil de publication 2.0](http://go.microsoft.com/fwlink/?LinkId=396421).  
  
-   Certificat de gestion téléchargé dans votre abonnement Microsoft Azure.  Créez le certificat de gestion à l’aide de l’applet de commande Powershell [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633(v=wps.630)).  Ensuite, téléchargez le certificat de gestion dans votre abonnement Microsoft Azure.  Pour plus d’informations sur le téléchargement d’un certificat de gestion, consultez [Téléchargement d’un certificat de gestion API dans Azure Management](https://azure.microsoft.com/en-us/documentation/articles/azure-api-management-certs/).  Exemple de syntaxe pour la création d’un certificat de gestion tiré de l’article [Vue d’ensemble des certificats pour Azure Cloud Services](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-certs-create/): 

    ```powershell  
    $cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
    $password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
    Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
    ```    

    > [!NOTE]  
    > L’outil MakeCert peut également être utilisé pour créer un certificat de gestion ; cependant, MakeCert est désormais déconseillé.  Pour plus d’informations, consultez la page [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968).
   
  -   Certificat de gestion enregistré dans le magasin de certificats personnel de l'ordinateur local sur lequel l'Assistant s'exécute.  
  
-   Vous devez disposer d'un emplacement de stockage temporaire disponible sur l'ordinateur sur lequel la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est hébergée. L'emplacement de stockage temporaire doit également être disponible sur l'ordinateur sur lequel s'exécute l'Assistant.  
  
-   Si vous déployez la base de données sur un ordinateur virtuel existant, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être configurée pour écouter sur un port TCP/IP.  
  
-   [L’adaptateur de cloud pour SQL Server](http://msdn.microsoft.com/library/82ed0d0f-952d-4d49-aa36-3855a3ca9877) doit être configuré et en cours d’exécution sur l’image de machine virtuelle Microsoft Azure ou de galerie que vous prévoyez d’utiliser pour créer la machine virtuelle.  
  
-   Vous devez configurer un point de terminaison ouvert pour votre [adaptateur de cloud pour SQL Server](http://msdn.microsoft.com/library/82ed0d0f-952d-4d49-aa36-3855a3ca9877) sur la passerelle Microsoft Azure avec le port privé 11435.  
  
 De plus, si vous projetez de déployer votre base de données dans un ordinateur virtuel Windows Azure existant, vous devez également fournir :  
  
-   Nom DNS du service de cloud computing qui héberge votre ordinateur virtuel.  
  
-   Informations d'identification de l'administrateur pour l'ordinateur virtuel.  
  
-   Informations d'identifications avec privilèges d'opérateur Backup sur la base de données que vous prévoyez de déployer, à partir de l'instance source de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour plus d’informations sur l’exécution de SQL Server sur des machines virtuelles Microsoft Azure, consultez [Approvisionnement d’une machine virtuelle SQL Server dans le portail Azure](https://msdn.microsoft.com/library/dn133141.aspx) et [Migrer une base de données SQL Server vers SQL Server dans une machine virtuelle Azure](http://msdn.microsoft.com/library/dn133142.aspx).  
  
 Sur les ordinateurs qui exécutent des systèmes d'exploitation Windows Server, vous devez utiliser les paramètres de configuration suivants pour exécuter l'Assistant :  
  
-   Désactivez la configuration de sécurité renforcée : utilisez le Gestionnaire de serveur > Serveur local pour définir la configuration de sécurité renforcée (ESC) d’Internet Explorer sur **OFF**.  
  
-   Activez JavaScript : Internet Explorer > Options Internet > Sécurité > Niveau client > Création de scripts > Active Scripting : **Activer**.  
  
###  <a name="limitations"></a> Limitations et restrictions  
Cette fonctionnalité de déploiement ne peut être utilisée qu’avec un compte de stockage Azure créé par le biais du modèle de déploiement de gestion des services (Classic). Pour plus d’informations sur les modèles de déploiement Azure, consultez [Déploiement Azure Resource Manager et déploiement classique](https://azure.microsoft.com/en-us/documentation/articles/resource-manager-deployment-model/).

 La limite de taille de la base de données pour cette opération est 1 To.  
  
 Cette fonctionnalité de déploiement est disponible dans SQL Server Management Studio pour [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Cette fonctionnalité de déploiement sert uniquement avec les bases de données utilisateur ; le déploiement de bases de données système n'est pas pris en charge.  
  
 La fonctionnalité de déploiement ne prend pas en charge les services hébergés qui sont associés à un groupe d'affinités. Par exemple, les comptes de stockage associés à un groupe d'affinités ne peuvent pas être sélectionnés pour être utilisés sur la page **Paramètres de déploiement** de cet Assistant.  
  
 Versions de la base de données SQL Server qui peuvent être déployées sur une machine virtuelle Windows Azure à l'aide de cet Assistant :  
  
-   SQL Server 2008  
  
-   SQL Server 2008 R2  
  
-   SQL Server 2012  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  

- [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Les versions de base de données SQL Server exécutées dans une machine virtuelle Windows Azure peuvent être déployées dans :  
  
-   SQL Server 2012  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  

- [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 S'il existe un conflit de noms entre le nom de la base de données d'une opération de déploiement en cours et d'une base de données existante sur la machine virtuelle, l'Assistant suggère un nom de base de données avec suffixe pour la base de données en cours pour vous permettre d'effectuer cette opération.  
  
###  <a name="filestream"></a> Éléments à prendre en considération pour déployer une base de données FILESTREAM sur des machines virtuelles Windows Azure  
 Tenez compte des instructions et des restrictions suivantes lorsque vous déployez des bases de données possédant des objets blob stockés dans des objets FILESTREAM :  
  
-   La fonctionnalité de déploiement ne peut pas déployer une base de données FILESTREAM dans de nouvelles machines virtuelles. Si FILESTREAM n'est pas activé dans la machine virtuelle avant d'exécuter l'Assistant, l'opération de restauration de base de données échouera et l'Assistant ne pourra pas achever l'opération. Pour déployer correctement une base de données qui utilise FILESTREAM, activez FILESTREAM dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur la machine virtuelle hôte avant de lancer l'Assistant. Pour plus d’informations, consultez [FILESTREAM (SQL Server)](http://msdn.microsoft.com/library/gg471497.aspx).  
  
-   Si votre base de données utilise l'OLTP en mémoire, déployez la base de données sur une machine virtuelle Azure sans aucune modification à la base de données. Pour plus d’informations, consultez [OLTP en mémoire (optimisation en mémoire)](http://msdn.microsoft.com/library/dn133186\(SQL.120\).aspx).  
  
###  <a name="geography"></a> Éléments à prendre en considération pour la répartition géographique des actifs  
 Notez que les actifs suivants doivent se trouver dans la même région géographique :  
  
-   Service de cloud computing  
  
-   Emplacement de la machine virtuelle  
  
-   Service de stockage de disque de données  
  
 Si les actifs répertoriés ci-dessus ne sont pas colocalisés, l'Assistant ne pourra pas terminer l'opération.  
  
###  <a name="configuration_settings"></a> Assistant Paramètres de configuration  
 Utilisez les informations de configuration suivantes pour modifier les paramètres d'un déploiement de base de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une machine virtuelle Azure.  
  
-   **Chemin par défaut du fichier de configuration** - %LOCALAPPDATA%\SQL Server\Deploy to SQL in WA VM\DeploymentSettings.xml  
  
-   **Structure du fichier de configuration**  
  
    -   \<DeploymentSettings>  
  
        -   \<OtherSettings  
  
            -   TraceLevel="Debug" \<!-- Logging level -->  
  
            -   BackupPath="\\\\[server name]\\[volume]\\" \<!-- The last used path for backup. Utilisé comme valeur par défaut dans l'Assistant. -->  
  
            -   CleanupDisabled = False /> \<!-- L’Assistant ne supprimera pas les fichiers intermédiaires et les objets Windows Azure (VM, CS, SA). -->  
  
        -   \<PublishProfile \< ! -- Dernières informations de profil de publication utilisées. -->  
  
            -   Certificate="12A34B567890123ABCD4EF567A8" \<!-- Certificat à utiliser dans l’Assistant. -->  
  
            -   Subscription="1a2b34c5-67d8-90ef-ab12-xxxxxxxxxxxxx" \<!-- Abonnement à utiliser dans l’Assistant. -->  
  
            -   Name="My Subscription" \<!-- Nom de l’abonnement. -->  
  
            -   Publisher="" />  
  
    -   \</DeploymentSettings>  
  
 **Valeurs du fichier de configuration**  
  
###  <a name="permissions"></a> Autorisations  
 La base de données déployée doit avoir un état normal, doit être accessible au compte d'utilisateur qui exécute l'Assistant, et le compte d'utilisateur doit avoir les autorisations requises pour exécuter une opération de sauvegarde.  
  
##  <a name="launch_wizard"></a> Utilisation de l’Assistant Déployer une base de données sur un ordinateur virtuel Microsoft Azure  
 **Pour lancer l'Assistant, suivez les étapes suivantes :**  
  
1.  Utilisez SQL Server Management Studio pour vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec la base de données que vous souhaitez déployer.  
  
2.  Dans l' **Explorateur d'objets**, développez le nom de l'instance, puis développez le nœud **Bases de données** .  
  
3.  Cliquez avec le bouton droit sur la base de données à déployer, sélectionnez **Tâches**, puis sélectionnez **Déployer une base de données sur une machine virtuelle Microsoft Azure…**  
  
##  <a name="wizard_pages"></a> Pages de l'assistant  
 Les sections suivantes fournissent des informations supplémentaires sur les propriétés de déploiement et les détails de configuration de cette opération.  
  
-   [Introduction](#Introduction)  
  
-   [Paramètres de la source](#Source_settings)  
  
-   [Connexion à Windows Azure](#Azure_sign-in)  
  
-   [Paramètres de déploiement](#Deployment_settings)  
  
-   [Résumé](#Summary)  
  
-   [Résultats](#Results)  
  
##  <a name="Introduction"></a> Introduction 
 Cette page décrit l’Assistant **Déployer une base de données sur un ordinateur virtuel Microsoft Azure** .  
  
-   **Ne plus afficher cette page.**  
  Activez cette case à cocher pour ne plus afficher la page Introduction à l’avenir.  
  
-   **Suivant**  
Passer à la page **Paramètres de la source** .  
  
-   **Annuler**  
  Annuler l’opération et fermer l’Assistant.  
  
-   **Aide**  
Ouvrir la rubrique d’aide MSDN de l’Assistant.  
  
##  <a name="Source_settings"></a> Paramètres de la source  
 Utilisez cette page pour vous connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge la base de données que vous souhaitez déployer dans la machine virtuelle Microsoft Azure. Vous spécifierez également un emplacement temporaire pour les fichiers que vous stockez sur l’ordinateur local avant de les transférer vers Microsoft Azure. Il peut s'agir d'un emplacement réseau partagé.  
 
- **SQL Server**    
Cliquez sur **Connecter** et spécifiez les informations de connexion pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge la base de données à déployer.  
  
-   **Sélectionner une base de données**  
Utilisez la liste déroulante pour spécifier la base de données à déployer.  
  
-   **Autres paramètres**  
Dans le champ, spécifiez un dossier partagé qui sera accessible au service de machine virtuelle Microsoft Azure.  
  
##  <a name="Azure_sign-in"></a> Connexion à Microsoft Azure  
 Connectez-vous à Microsoft Azure avec votre compte Microsoft ou votre compte professionnel ou scolaire. Votre compte Microsoft ou compte professionnel ou scolaire se présente sous la forme d’une adresse e-mail, telle que patc@contoso.com. Pour en savoir plus sur les informations d’identification Azure, consultez les rubriques suivantes : [Microsoft Account for Organizations FAQ (FAQ sur les comptes Microsoft pour les organisations)](http://technet.microsoft.com/jj592903) et [Résolution des problèmes](https://technet.microsoft.com/dn197220).  
  
##  <a name="Deployment_settings"></a> Paramètres de déploiement
 Utilisez cette page pour spécifier le serveur de destination et fournir des détails sur votre nouvelle base de données.  
  
 **Machine virtuelle Microsoft Azure**  
  
-   **Nom du service cloud**  
Spécifiez le nom du service qui héberge la machine virtuelle. Pour créer un service de cloud computing, spécifiez un nom.  
  
-   **Nom de l’ordinateur virtuel** Spécifiez le nom de la machine virtuelle qui hébergera la base de données SQL Server. Pour créer une machine virtuelle Microsoft Azure, spécifiez un nom pour cette nouvelle machine virtuelle.  
  
-   **Compte de stockage**  
Sélectionnez le compte de stockage dans la liste déroulante. Pour créer un compte de stockage, spécifiez un nom. Notez que les comptes de stockage associés à un groupe d'affinités ne seront pas disponibles dans la liste déroulante.  

-   **Paramètres**  
Utilisez le bouton Paramètres afin de créer une machine virtuelle pour héberger la base de données SQL Server. Si vous utilisez une machine virtuelle existante, les informations spécifiées seront utilisées pour authentifier vos informations d'identification.  
  
**Base de données cible**
  
-   **Nom de l’instance SQL**  
Informations de connexion du serveur.  
  
-   **Nom de la base de données**  
Spécifiez ou confirmez le nom d’une nouvelle base de données. Si le nom de la base de données existe déjà sur l'instance SQL Server de destination, modifiez le nom.  
  
##  <a name="Summary"></a> Résumé
 Utilisez cette page pour passer en revue les paramètres spécifiés pour l'opération. Pour terminer le déploiement à l'aide des paramètres spécifiés, cliquez sur **Terminer**. Pour annuler le déploiement et quitter l'Assistant, cliquez sur **Annuler**.  Lorsque vous cliquez sur **Terminer** , la page **État d’avancement du déploiement** s’ouvre.  Vous pouvez également afficher l’avancement à partir du fichier journal situé dans `"%LOCALAPPDATA%\SQL Server\Deploy to SQL in WA VM"`.
  
 Des étapes manuelles peuvent être nécessaires pour déployer les détails de la base de données sur la base de données SQL Server dans un ordinateur virtuel Windows Azure. Ces étapes seront décrites de façon détaillée.  
  
##  <a name="Results"></a> Résultats 
 Cette page signale la réussite ou l'échec de l'opération de déploiement, affichant les résultats de chaque action. Toute action pour laquelle une erreur s'est produite comportera une indication dans la colonne **Résultat** . Cliquez sur le lien pour consulter le rapport d'erreur de cette action.  
  
 Cliquez sur **Terminer** pour fermer l'Assistant.  
  
## <a name="see-also"></a> Voir aussi  
 [L’adaptateur de cloud pour SQL Server](http://msdn.microsoft.com/library/82ed0d0f-952d-4d49-aa36-3855a3ca9877)   
 [Gestion du cycle de vie de base de données](../../relational-databases/database-lifecycle-management.md)   
 [Exporter une application de la couche Données](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)   
 [Importer un fichier BACPAC pour créer une nouvelle base de données utilisateur](../../relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)   
 [Sauvegarde et restauration Base de données SQL Azure](https://msdn.microsoft.com/library/azure/jj650016.aspx)   
 [Déploiement de SQL Server dans Windows Azure Virtual Machines](http://msdn.microsoft.com/library/dn133141.aspx)   
 [Préparation à la migration vers SQL Server dans Windows Azure Virtual Machines](http://msdn.microsoft.com/library/dn133142.aspx)  
  
  
