---
title: Déployer une base de données à l’aide d’une DAC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: data-tier-applications
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-data-tier-apps
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbdeployment.settings.f1
- sql13.swb.dbdeployment.progress.f1
- sql13.swb.dbdeployment.summary.f1
- sql13.swb.dbdeployment.results.f1
- sql13.swb.dbdeployment.welcome.f1
helpviewer_keywords:
- deploy database wizard
- database deploy [SQL Server]
ms.assetid: 08c506e8-4ba0-4a19-a066-6e6a5c420539
caps.latest.revision: 12
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d87f60bc73ff969aa2f3f6ef42264ea8cb93b23c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-a-database-by-using-a-dac"></a>Déployer une base de données à l'aide d'une DAC
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’Assistant **Déploiement de base de données dans SQL Azure** pour déployer une base de données entre une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et un serveur [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] , ou entre deux serveurs [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
##  <a name="BeforeBegin"></a> Avant de commencer  
 L'Assistant utilise un fichier d'archive d'application de la couche Données (DAC) BACPAC pour déployer les données et les définitions des objets de base de données. Il effectue une opération d'exportation DAC de la base de données source, et une importation DAC vers la destination.  
  
###  <a name="DBOptSettings"></a> Options et paramètres de bases de données  
 Par défaut, la base de données créée pendant le déploiement aura tous les paramètres par défaut de l'instruction CREATE DATABASE. Le classement de base de données et le niveau de compatibilité sont définis aux valeurs de la base de données source et font exception.  
  
 Les options de base de données, telles que TRUSTWORTHY, DB_CHAINING et HONOR_BROKER_PRIORITY, ne peuvent pas être ajustées dans le cadre du processus de déploiement. Des propriétés physiques, telles que le nombre de groupes de fichiers ou le nombre et la taille des fichiers, ne peuvent pas être modifiées dans le cadre du processus de déploiement. Une fois le déploiement terminé, vous pouvez utiliser l'instruction ALTER DATABASE, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell pour personnaliser la base de données.  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 L'Assistant **Déploiement de base de données** prend en charge le déploiement d'une base de données :  
  
-   D'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
-   D'une instance de [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] vers une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Entre deux serveurs [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] .  
  
 L'Assistant ne prend pas en charge le déploiement de bases de données entre deux instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit exécuter [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) ou version ultérieure pour fonctionner avec l'Assistant. Si une base de données sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] contient des objets qui ne sont pas pris en charge sur [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)], vous ne pouvez pas utiliser l'Assistant pour déployer la base de données dans [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Si une base de données sur [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] contient des objets qui ne sont pas pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous ne pouvez pas utiliser l'Assistant pour déployer la base de données sur des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="Security"></a> Sécurité  
 Pour améliorer la sécurité, les connexions d'authentification SQL Server sont stockées dans un fichier DAC BACPAC sans mot de passe. Lorsque le fichier BACPAC est importé, la connexion est créée en tant que connexion désactivée avec un mot de passe généré. Pour activer les connexions, connectez-vous à l'aide d'une connexion qui possède l'autorisation ALTER ANY LOGIN et utilisez ALTER LOGIN pour activer la connexion et affecter un nouveau mot de passe pouvant être communiqué à l'utilisateur. Cela n'est pas nécessaire pour les connexions d'authentification Windows car leurs mots de passe ne sont pas gérés par SQL Server.  
  
#### <a name="permissions"></a>Autorisations  
 L'Assistant a besoin d'autorisations d'exportation DAC dans la base de données source. La connexion nécessite au minimum des autorisations ALTER ANY LOGIN et VIEW DEFINITION de la portée de la base de données, ainsi que des autorisations SELECT sur **sys.sql_expression_dependencies**. L'exportation d'une DAC peut être réalisée par les membres du rôle serveur fixe securityadmin également membres du rôle de base de données fixe database_owner dans la base de données à partir de laquelle est extraite la DAC. Les membres du rôle serveur fixe sysadmin ou le compte d’administrateur système intégré de SQL Server nommé **sa** peuvent également exporter une DAC.  
  
 L'Assistant a besoin d'autorisations d'exportation DAC sur l'instance ou le serveur de destination. La connexion doit être membre des rôles serveur fixes **sysadmin** ou **serveradmin** , ou du rôle serveur fixe **dbcreator** et disposer d'autorisations ALTER ANY LOGIN. Le compte d’administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré nommé **sa** peut également importer une DAC. L'importation d'une DAC avec des connexions à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] requiert l'appartenance aux rôles loginmanager ou serveradmin. L'importation d'une DAC sans connexions à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] requiert l'appartenance aux rôles dbmanager ou serveradmin.  
  
##  <a name="UsingDeployDACWizard"></a> Utilisation de l'Assistant Déploiement de base de données  
 **Pour migrer une base de données à l'aide de l'Assistant de déploiement de base de données**  
  
1.  Connectez-vous à l'emplacement de la base de données à déployer. Vous pouvez spécifier une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou un serveur [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] .  
  
2.  Dans l' **Explorateur d'objets**, développez le nœud pour l'instance qui contient la base de données.  
  
3.  Développez le nœud **Bases de données** .  
  
4.  Cliquez avec le bouton droit sur la base de données que vous voulez déployer, sélectionnez **Tâches**, puis sélectionnez **Déployer la base de données dans SQL Azure…**  
  
5.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    -   [Page Introduction](#Introduction)  
  
    -   [Paramètres de déploiement](#Deployment_settings)  
  
    -   [Page Résumé](#Summary)  
  
    -   [Progression](#Progress)  
    
    -   [Résultats](#Results)  
  
##  <a name="Introduction"></a> Page Introduction  
 Cette page décrit les étapes de l'Assistant **Déploiement de base de données**  
  
 **Options**  
  
-   **Ne plus afficher cette page.** - Activez la case à cocher pour ne plus afficher la page Introduction à l'avenir.  
  
-   **Suivant** : passe à la page **Paramètres de déploiement** .  
  
-   **Annuler** : annule l'opération et ferme l'Assistant.  
  
##  <a name="Deployment_settings"></a> Page Paramètres de déploiement  
 Utilisez cette page pour spécifier le serveur de destination et fournir des détails sur votre nouvelle base de données.  
  
 **Hôte local :**  
  
-   **Connexion serveur** – Spécifiez les détails de connexion au serveur, puis cliquez sur **Se connecter** pour vérifier la connexion.  
  
-   **Nouveau nom de la base de données** – Spécifiez un nom pour la nouvelle base de données.  
  
 **[!INCLUDE[ssSDS](../../includes/sssds-md.md)] :**  
  
-   **Édition [!INCLUDE[ssSDS](../../includes/sssds-md.md)]** : sélectionnez l’édition de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] dans le menu déroulant.  
  
-   **Taille maximale de base de données** : sélectionnez la taille maximale de la base de données dans le menu déroulant.  
  
 **Autres paramètres :**  
  
-   Spécifiez un répertoire local pour le fichier temporaire, qui est le fichier d'archive BACPAC. Notez que le fichier est créé à l'emplacement spécifié et qu'il y reste une fois l'opération terminée.  
  
##  <a name="Summary"></a> Page Résumé  
 Utilisez cette page pour passer en revue la source spécifiée et les paramètres cibles de l'opération. Pour terminer le déploiement à l'aide des paramètres spécifiés, cliquez sur **Terminer**. Pour annuler le déploiement et quitter l'Assistant, cliquez sur **Annuler**.  
  
##  <a name="Progress"></a> Page Progression  
 Cette page affiche une barre de progression indiquant l'état de l'opération. Pour afficher l'état détaillé, cliquez sur l'option **Afficher les détails** .  
  
##  <a name="Results"></a> Page Résultats  
 Cette page signale la réussite ou l'échec de l'opération de déploiement, affichant les résultats de chaque action. Toute action pour laquelle une erreur s'est produite aura un lien dans la colonne **Résultat** . Cliquez sur le lien pour consulter le rapport d'erreur de cette action.  
  
 Cliquez sur **Terminer** pour fermer l'Assistant.  
  
## <a name="using-a-net-framework-application"></a>Utilisation d'une application .Net Framework  
 **Pour déployer une base de données à l’aide des méthodes DacStore, Export() et Import() dans une application .NET Framework.**  
  
 Pour afficher un exemple de code, téléchargez l'exemple d'application DAC sur [Codeplex](http://go.microsoft.com/fwlink/?LinkId=219575)  
  
1.  Créez un objet serveur SMO et définissez-le sur l'instance ou le serveur qui contient la base de données à déployer.  
  
2.  Ouvrez un objet **ServerConnection** et connectez-vous à la même instance.  
  
3.  Utilisez la méthode **Export** de type **Microsoft.SqlServer.Management.Dac.DacStore** pour exporter la base de données dans un fichier BACPAC. Spécifiez le nom de la base de données à exporter, ainsi que le chemin d'accès au dossier où le fichier BACPAC doit être placé.  
  
4.  Créez un objet serveur SMO et définissez-le sur l'instance ou le serveur de destination.  
  
5.  Ouvrez un objet **ServerConnection** et connectez-vous à la même instance.  
  
6.  Utilisez la méthode **Import** de type **Microsoft.SqlServer.Management.Dac.DacStore** pour importer le fichier BACPAC. Spécifiez le fichier BACPAC créé par l'exportation.  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Exporter une application de la couche Données](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)   
 [Importer un fichier BACPAC pour créer une nouvelle base de données utilisateur](../../relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)  
  
  
