---
title: Installer et configurer la recherche sémantique | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], installing
- semantic search [SQL Server], configuring
ms.assetid: 2cdd0568-7799-474b-82fb-65d79df3057c
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6816d53fb9b9c8bc79bec05623640c91f9edd3d8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="install-and-configure-semantic-search"></a>Installer et configurer la recherche sémantique
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit les conditions préalables à une recherche sémantique statistique, ainsi que la procédure d'installation ou de vérification de ces conditions.  
  
## <a name="install-semantic-search"></a>Installer la recherche sémantique  
  
###  <a name="HowToCheckInstalled"></a> Vérifier si la recherche sémantique est installée  
 Interrogez la propriété **IsFullTextInstalled** de la fonction de métadonnées [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md).  
  
 Une valeur de retour de 1 indique que la recherche en texte intégral et la recherche sémantique sont installées ; une valeur de retour de 0 indique qu'elles ne le sont pas.  
  
```sql  
SELECT SERVERPROPERTY('IsFullTextInstalled');  
GO  
```  
  
###  <a name="BasicsSemanticSearch"></a> Installer la recherche sémantique  
 Pour installer la recherche sémantique, sélectionnez **Extraction en texte intégral et extraction sémantique de recherche** dans la page **Fonctionnalités à installer** pendant l’installation de SQL Server.  
  
 La recherche sémantique statistique dépend de la recherche en texte intégral. Ces deux fonctionnalités en option de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées ensemble.  
  
## <a name="install-the-semantic-language-statistics-database"></a>Installer la base de données des statistiques linguistiques de sémantique  
 La recherche sémantique a une dépendance externe supplémentaire qui est appelée base de données des statistiques linguistiques de sémantique. Cette base de données contient les modèles linguistiques statistiques requis par la recherche sémantique. Une base de données unique des statistiques linguistiques de sémantique contient les modèles linguistiques de toutes les langues prises en charge pour l'indexation sémantique.  
  
###  <a name="HowToCheckDatabase"></a> Vérifier si la base de données des statistiques linguistiques de sémantique est installée  
 Interrogez l’affichage catalogue [sys.fulltext_semantic_language_statistics_database &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md).  
  
 Si la base de données des statistiques linguistiques de sémantique est installée et inscrite pour l'instance, les résultats de la requête contiennent une seule ligne d'informations sur la base de données.  
  
```sql  
SELECT * FROM sys.fulltext_semantic_language_statistics_database;  
GO  
```  
  
###  <a name="HowToInstallModel"></a> Installer, attacher et inscrire la base de données des statistiques linguistiques de sémantique  
 La base de données des statistiques linguistiques de sémantique n'est pas installée par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour installer la base de données des statistiques linguistiques de sémantique comme une condition préalable à l’indexation sémantique, procédez comme suit :  
  
 **1. Installez la base de données des statistiques linguistiques de sémantique.**  
 
 1.  Localisez la base de données des statistiques linguistiques de sémantique sur le support d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou téléchargez-la sur le Web.  
  
        1.  Localisez le package Windows Installer nommé **SemanticLanguageDatabase.msi** sur le support d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        2.  Téléchargez le package d’installation à partir de la page [Microsoft® SQL Server® 2016 Semantic Language Statistics](https://www.microsoft.com/en-us/download/details.aspx?id=52681) du centre de téléchargement [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
2.  Exécutez le package Windows Installer **SemanticLanguageDatabase.msi** pour extraire la base de données et le fichier journal.  
  
     Vous pouvez éventuellement modifier le répertoire de destination. Par défaut, le programme d’installation extrait les fichiers dans un sous-dossier nommé **Microsoft Semantic Language Database** du dossier Program Files. Le fichier MSI contient un fichier de base de données et un fichier journal compressés.  
  
3.  Déplacez le fichier de base de données et le fichier journal extraits vers l'emplacement approprié dans le système de fichiers.  
  
     Si vous laissez les fichiers dans leur emplacement par défaut, il n'est pas possible d'extraire une autre copie de la base de données pour une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    > [!IMPORTANT]  
    >  Lorsque la base de données des statistiques linguistiques de sémantique est extraite, des autorisations limitées sont assignées au fichier de base de données et au fichier journal à l'emplacement par défaut dans le système de fichiers. Par conséquent, vous pouvez ne pas avoir l'autorisation d'attacher la base de données si vous la laissez dans l'emplacement par défaut. Si une erreur se produit lorsque vous essayez d'attacher la base de données, déplacez les fichiers, ou vérifiez et corrigez les autorisations du système de fichiers s'il y a lieu.  
  
 **2. Attachez la base de données des statistiques linguistiques de sémantique.**
   
 Attachez la base de données à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou en appelant [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md) avec la syntaxe **FOR ATTACH**. Pour plus d’informations, consultez [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
 Par défaut, le nom de la base de données est **semanticsdb**. Vous pouvez éventuellement donner un nom différent à la base de données lorsque vous l'attachez. Vous devez spécifier ce nom lorsque vous inscrivez la base de données à l'étape suivante.  
  
```sql  
CREATE DATABASE semanticsdb  
            ON ( FILENAME = 'C:\Microsoft Semantic Language Database\semanticsdb.mdf' )  
            LOG ON ( FILENAME = 'C:\Microsoft Semantic Language Database\semanticsdb_log.ldf' )  
            FOR ATTACH;  
GO  
```  
  
 Cet exemple de code suppose que vous avez déplacé la base de données de son emplacement par défaut vers un nouvel emplacement.  
  
 **3. Inscrivez la base de données des statistiques linguistiques de sémantique.** 
  
 Appelez la procédure stockée [sp_fulltext_semantic_register_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md) et fournissez le nom attribué à la base de données lors de son attachement.  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = N'semanticsdb';  
GO  
```  

##  <a name="reqinstall"></a> Exigences et restrictions pour la base de données des statistiques linguistiques de sémantique  
  
-   Vous ne pouvez attacher et inscrire une base de données des statistiques linguistiques de sémantique que sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un seul ordinateur requiert une copie physique distincte de la base de données des statistiques linguistiques de sémantique. Attachez une copie à chaque instance.  
  
-   Vous ne pouvez pas détacher une base de données des statistiques linguistiques de sémantique valide et inscrite et la remplacer par une base de données arbitraire du même nom. Vous risqueriez sinon de provoquer l'échec du remplissage de l'index actif ou des futurs index.  
  
-   La base de données des statistiques linguistiques de sémantique est en lecture seule. Vous ne pouvez pas personnaliser cette base de données. Si vous modifiez le contenu de la base de données de quelque manière que ce soit, les résultats d'une future indexation sémantique sont non déterministes. Pour restaurer l'état d'origine de ces données, vous pouvez supprimer la base de données modifiée et télécharger et attacher une copie nouvelle et inchangée de la base de données.  
  
-   Il est possible de détacher ou de supprimer la base de données des statistiques linguistiques de sémantique. S'il existe des opérations d'indexation actives qui comportent des verrous de lecture sur la base de données, l'opération de détachement ou de suppression échoue ou expire. Ce comportement est cohérent avec celui existant. Une fois la base de données supprimée, les opérations d'indexation sémantique échouent.  
 
##  <a name="HowToUnregister"></a> Supprimer la base de données des statistiques linguistiques de sémantique  

###  <a name="unregister-detach-and-remove-the-semantic-language-statistics-database"></a>Annuler l’inscription, détacher et supprimer la base de données des statistiques linguistiques de sémantique 

 **1. Annulez l’inscription de la base de données des statistiques linguistiques de sémantique.**
   
 Appelez la procédure stockée [sp_fulltext_semantic_unregister_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md). Vous ne devez pas fournir le nom de la base de données étant donné qu'une instance ne peut avoir qu'une seule base de données des statistiques linguistiques de sémantique.  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
 **2. Détachez la base de données des statistiques linguistiques de sémantique.**  
 
 Appelez la procédure stockée [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) et fournissez le nom de la base de données.  
  
```sql  
USE master;  
GO  
  
EXEC sp_detach_db @dbname = N'semanticsdb';  
GO  
```  
  
 **3. Supprimez la base de données des statistiques linguistiques de sémantique.**  
 
 Après avoir annulé l'inscription de la base de données et l'avoir détachée, vous pouvez simplement supprimer le fichier de base de données. Il n'existe aucun programme de désinstallation ni aucune entrée dans **Programmes et fonctionnalités** dans le Panneau de configuration.  
  
## <a name="install-optional-support-for-newer-document-types"></a>Installer la prise en charge facultative de nouveaux types de documents  
  
###  <a name="office"></a> Installer les derniers filtres pour Microsoft Office et d’autres types de documents Microsoft  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les analyseurs lexicaux et les générateurs de formes dérivées [!INCLUDE[msCoName](../../includes/msconame-md.md)] les plus récents, mais n'installe pas les filtres les plus récents pour les documents [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office et d'autres types de documents [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Ces filtres sont nécessaires pour l'indexation des documents créés avec les versions récentes de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office et d'autres applications [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Pour télécharger les filtres les plus récents, consultez [Microsoft Office 2010 Filter Packs](http://go.microsoft.com/fwlink/?LinkId=218293). (Il n’existe apparemment pas de version Filter Pack pour Office 2013 et Office 2016.)
  
  
