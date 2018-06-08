---
title: Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backup compression [SQL Server], Resource Governor
- backup compression [SQL Server], CPU usage
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- Resource Governor, backup compression
ms.assetid: 01796551-578d-4425-9b9e-d87210f7ba72
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 9d539b5cc863a4371f29c7e2b34c3cb83560f2bd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql"></a>Utiliser le gouverneur de ressources pour limiter l'utilisation de l'UC par compression de sauvegarde (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Par défaut, sauvegarder en utilisant la compression augmente considérablement l'utilisation de l'UC et l'UC supplémentaire consommée par le processus de compression peut nuire aux opérations simultanées. Ainsi, il peut être préférable, dans une session où l’utilisation du processeur est limitée, de créer une sauvegarde compressée de priorité basse à l’aide de[Resource Governor](../../relational-databases/resource-governor/resource-governor.md) en cas de contention du processeur. Cette rubrique présente un scénario qui classifie les sessions d'un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] particulier en les mappant à un groupe de charge de travail de Resource Governor qui limite l'utilisation de l'UC dans de tels cas.  
  
> [!IMPORTANT]  
>  Dans un scénario donné de Resource Governor, la classification des sessions peut être basée sur un nom d'utilisateur, un nom d'application ou tout autre élément permettant d'identifier une connexion. Pour plus d'informations, consultez [Fonction classifieur de Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md) et [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md).  
  
##  <a name="Top"></a> Cette rubrique propose les scénarios suivants, présentés dans l'ordre :  
  
1.  [Configuration d'une connexion et d'un utilisateur pour les opérations de priorité basse](#setup_login_and_user)  
  
2.  [Configuration de Resource Governor pour limiter l'utilisation de l'UC](#configure_RG)  
  
3.  [Vérification de la classification de la session active (Transact-SQL)](#verifying)  
  
4.  [Compression de sauvegardes dans une session à utilisation maximale de l'UC limitée](#creating_compressed_backup)  
  
##  <a name="setup_login_and_user"></a> Configuration d'une connexion et d'un utilisateur pour les opérations de priorité basse  
 Le scénario de cette rubrique requiert une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de priorité basse et un utilisateur. Le nom d'utilisateur sera utilisé pour classifier des sessions exécutées dans la connexion et pour les router vers un groupe de charge de travail de Resource Governor qui limite l'utilisation de l'UC.  
  
 La procédure ci-dessous décrit les étapes nécessaires à la configuration d'une connexion et d'un utilisateur à cette fin. Elle est suivie d'un exemple [!INCLUDE[tsql](../../includes/tsql-md.md)] , « Exemple A : configuration d'une connexion et d'un utilisateur (Transact-SQL) ».  
  
### <a name="to-set-up-a-login-and-database-user-for-classifying-sessions"></a>Pour configurer une connexion et un utilisateur de base de données afin de classifier des sessions  
  
1.  Créez une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour créer des sauvegardes compressées de priorité basse.  
  
     **Pour créer une connexion**  
  
    -   [Créer un compte de connexion](../../relational-databases/security/authentication-access/create-a-login.md)  
  
    -   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
2.  Accordez éventuellement à cette connexion l'autorisation VIEW SERVER STATE.  
  
    -   [GRANT – octroi d’autorisations d’objet système &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)  
  
     Pour plus d’informations, consultez [GRANT – octroi d’autorisations de principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md).  
  
3.  Créez un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour cette connexion.  
  
     **Pour créer un utilisateur**  
  
    -   [Créer un utilisateur de base de données](../../relational-databases/security/authentication-access/create-a-database-user.md)  
  
    -   [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)  
  
4.  Pour permettre aux sessions de cette connexion et cet utilisateur de sauvegarder une base de données spécifique, ajoutez l'utilisateur au rôle de base de données db_backupoperator de cette base de données. Répétez l'opération pour chaque base de données que cet utilisateur doit sauvegarder. Ajoutez éventuellement l'utilisateur à d'autres rôles de base de données fixes.  
  
     **Pour ajouter un utilisateur de base de données à un rôle de base de données fixe**  
  
    -   [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
  
     Pour plus d’informations, consultez [GRANT – octroi d’autorisations de principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md).  
  
### <a name="example-a-setting-up-a-login-and-user-transact-sql"></a>Exemple A : configuration d'une connexion et d'un utilisateur (Transact-SQL)  
 L'exemple ci-dessous est pertinent uniquement si vous choisissez de créer une connexion et un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour des sauvegardes de priorité basse. Vous avez également la possibilité d'utiliser une connexion et un utilisateur existants, le cas échéant.  
  
> [!IMPORTANT]  
>  L’exemple ci-dessous utilise un exemple de connexion et de nom d’utilisateur, *domaine_nom*`\MAX_CPU`. Remplacez-le par les noms de la connexion et de l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous projetez d'utiliser lors de la création de vos sauvegardes compressées de priorité basse.  
  
 Cet exemple crée une connexion pour le compte Windows *domaine_nom*`\MAX_CPU` , puis accorde l’autorisation VIEW SERVER STATE à la connexion. Cette autorisation vous permet de vérifier la classification de Resource Governor pour les sessions de la connexion. L’exemple crée ensuite un utilisateur pour *domaine_nom*`\MAX_CPU` et l’ajoute au rôle de base de données fixe db_backupoperator pour l’exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] . Ce nom d'utilisateur sera utilisé par la fonction classifieur de Resource Governor.  
  
```sql  
-- Create a SQL Server login for low-priority operations  
USE master;  
CREATE LOGIN [domain_name\MAX_CPU] FROM WINDOWS;  
GRANT VIEW SERVER STATE TO [domain_name\MAX_CPU];  
GO  
-- Create a SQL Server user in AdventureWorks2012 for this login  
USE AdventureWorks2012;  
CREATE USER [domain_name\MAX_CPU] FOR LOGIN [domain_name\MAX_CPU];  
EXEC sp_addrolemember 'db_backupoperator', 'domain_name\MAX_CPU';  
GO  
  
```  
  
 [&#91;Haut&#93;](#Top)  
  
##  <a name="configure_RG"></a> Configuration de Resource Governor pour limiter l'utilisation de l'UC  
  
> [!NOTE]  
>  Vérifiez que Resource Governor est activé. Pour plus d’informations, consultez [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md).  
  
 Dans ce scénario de Resource Governor, la configuration comprend les étapes de base indiquées ci-dessous.  
  
1.  Création et configuration d'un pool de ressources de Resource Governor limitant la bande passante processeur moyenne maximale qui sera allouée aux demandes dans le pool de ressources en cas de contention du processeur.  
  
2.  Création et configuration d'un groupe de charge de travail de Resource Governor qui utilise ce pool.  
  
3.  Création d’une *fonction classifieur*, qui est une fonction définie par l’utilisateur et dont les valeurs de retour sont utilisées par Resource Governor pour classifier des sessions pour qu’elles soient routées vers le groupe de charge de travail approprié.  
  
4.  Inscription de la fonction classifieur auprès de Resource Governor.  
  
5.  Application des modifications à la configuration en mémoire de Resource Governor.  
  
> [!NOTE]  
>  Pour plus d’informations sur les pools de ressources, les groupes de charge de travail et la classification de Resource Governor, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour ces étapes sont décrites dans la procédure « Pour configurer Resource Governor afin de limiter l'utilisation de l'UC » qui est suivie d'un exemple [!INCLUDE[tsql](../../includes/tsql-md.md)] de la procédure.  
  
 **Pour configurer Resource Governor (SQL Server Management Studio)**  
  
-   [Configurer Resource Governor à l'aide d'un modèle](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)  
  
-   [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)  
  
-   [Créer un groupe de charge de travail](../../relational-databases/resource-governor/create-a-workload-group.md)  
  
### <a name="to-configure-resource-governor-for-limiting-cpu-usage-transact-sql"></a>Pour configurer Resource Governor afin de limiter l'utilisation de l'UC (Transact-SQL)  
  
1.  Émettez une instruction [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md) pour créer un pool de ressources. L'exemple de cette procédure utilise la syntaxe suivante :  
  
     *CREATE RESOURCE POOL nom_du_pool* WITH ( MAX_CPU_PERCENT = *valeur* );  
  
     *Value* est un entier de 1 à 100 qui indique le pourcentage de bande passante processeur moyenne maximale. La valeur appropriée dépend de votre environnement. À des fins d'illustration, l'exemple de cette rubrique utilise 20 % (MAX_CPU_PERCENT = 20.)  
  
2.  Émettez une instruction [CREATE WORKLOAD GROUP](../../t-sql/statements/create-workload-group-transact-sql.md) pour créer un groupe de charge de travail pour les opérations de priorité basse dont vous souhaitez régir l’utilisation de l’UC. L'exemple de cette procédure utilise la syntaxe suivante :  
  
     CREATE WORKLOAD GROUP *nom_du_groupe* USING *nom_du_pool*;  
  
3.  Émettez une instruction [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) pour créer une fonction classifieur qui mappe le groupe de charge de travail créé à l’étape précédente à l’utilisateur de la connexion de priorité basse. L'exemple de cette procédure utilise la syntaxe suivante :  
  
     CREATE FUNCTION [*nom_de_schéma*.]*nom_de_fonction*() RETURNS sysname  
  
     WITH SCHEMABINDING  
  
     AS  
  
     BEGIN  
  
     DECLARE @workload_group_name AS *sysname*  
  
     IF (SUSER_NAME() = '*utilisateur_de_connexion_faible_priorité*')  
  
     SET @workload_group_name = '*nom_groupe_charge_de_travail*'  
  
     RETURN @workload_group_name  
  
     END  
  
     Pour plus d'informations sur les composants de cette instruction CREATE FUNCTION, consultez :  
  
    -   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
  
    -   [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md)  
  
        > [!IMPORTANT]  
        >  SUSER_NAME est simplement l'une des fonctions système qui peuvent être utilisées dans une fonction classifieur. Pour plus d’informations, consultez [Créer et tester une fonction classifieur définie par l’utilisateur](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md).  
  
    -   [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
4.  Émettez une instruction [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) pour inscrire la fonction classifieur auprès de Resource Governor. L'exemple de cette procédure utilise la syntaxe suivante :  
  
     ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = *nom_de_schéma*.*nom_de_fonction*);  
  
5.  Émettez une deuxième instruction ALTER RESOURCE GOVERNOR pour appliquer les modifications à la configuration en mémoire de Resource Governor, comme suit :  
  
    ```  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    ```  
  
### <a name="example-b-configuring-resource-governor-transact-sql"></a>Exemple B : configuration de Resource Governor (Transact-SQL)  
 L'exemple ci-dessous effectue les étapes qui suivent dans une transaction unique.  
  
1.  Il crée le pool de ressources `pMAX_CPU_PERCENT_20` .  
  
2.  Il crée le groupe de charge de travail `gMAX_CPU_PERCENT_20` .  
  
3.  Il crée la fonction classifieur `rgclassifier_MAX_CPU()` , qui utilise le nom d'utilisateur créé à l'exemple précédent.  
  
4.  Il inscrit la fonction classifieur auprès de Resource Governor.  
  
 Après avoir validé la transaction, l'exemple applique les modifications de configuration demandées dans les instructions ALTER WORKLOAD GROUP ou ALTER RESOURCE POOL.  
  
> [!IMPORTANT]  
>  L’exemple suivant utilise le nom d’utilisateur de l’exemple d’utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créé dans « Exemple A : configuration d’une connexion et d’un utilisateur (Transact-SQL) », *domaine_nom*`\MAX_CPU`. Remplacez-le par le nom de l'utilisateur de la connexion que vous projetez d'utiliser pour créer les sauvegardes compressées de priorité basse.  
  
```sql  
-- Configure Resource Governor.  
BEGIN TRAN  
USE master;  
-- Create a resource pool that sets the MAX_CPU_PERCENT to 20%.   
CREATE RESOURCE POOL pMAX_CPU_PERCENT_20  
   WITH  
      (MAX_CPU_PERCENT = 20);  
GO  
-- Create a workload group to use this pool.   
CREATE WORKLOAD GROUP gMAX_CPU_PERCENT_20  
USING pMAX_CPU_PERCENT_20;  
GO  
-- Create a classification function.  
-- Note that any request that does not get classified goes into   
-- the 'Default' group.  
CREATE FUNCTION dbo.rgclassifier_MAX_CPU() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
    DECLARE @workload_group_name AS sysname  
      IF (SUSER_NAME() = 'domain_name\MAX_CPU')  
          SET @workload_group_name = 'gMAX_CPU_PERCENT_20'  
    RETURN @workload_group_name  
END;  
GO  
  
-- Register the classifier function with Resource Governor.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION= dbo.rgclassifier_MAX_CPU);  
COMMIT TRAN;  
GO  
-- Start Resource Governor  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
  
```  
  
 [&#91;Haut&#93;](#Top)  
  
##  <a name="verifying"></a> Vérification de la classification de la session active (Transact-SQL)  
 Éventuellement, connectez-vous en tant que l’utilisateur spécifié dans votre fonction classifieur et vérifiez la classification des sessions au moyen de l’instruction [SELECT](../../t-sql/queries/select-transact-sql.md) suivante dans l’Explorateur d’objets :  
  
```sql  
USE master;  
SELECT sess.session_id, sess.login_name, sess.group_id, grps.name   
FROM sys.dm_exec_sessions AS sess   
JOIN sys.dm_resource_governor_workload_groups AS grps   
    ON sess.group_id = grps.group_id  
WHERE session_id > 50;  
GO  
```  
  
 Dans le volet de résultats, la colonne **nom** doit répertorier une ou plusieurs sessions pour le nom de groupe de charge de travail que vous avez spécifié dans votre fonction classifieur.  
  
> [!NOTE]  
>  Pour plus d’informations sur les vues de gestion dynamique appelées par cette instruction SELECT, consultez [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) et [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 [&#91;Haut&#93;](#Top)  
  
##  <a name="creating_compressed_backup"></a> Compression de sauvegardes dans une session à utilisation maximale de l'UC limitée  
 Pour créer une sauvegarde compressée dans une session à utilisation maximale de l'UC limitée, connectez-vous en tant que l'utilisateur spécifié dans votre fonction classifieur. Dans votre commande de sauvegarde, spécifiez WITH COMPRESSION ([!INCLUDE[tsql](../../includes/tsql-md.md)]) ou sélectionnez **Compresser la sauvegarde** ([!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]). Pour créer une sauvegarde de base de données compressée, consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
### <a name="example-c-creating-a-compressed-backup-transact-sql"></a>Exemple C : création d'une sauvegarde compressée (Transact-SQL)  
 L’exemple [BACKUP](../../t-sql/statements/backup-transact-sql.md) suivant crée une sauvegarde complète compressée de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] dans un fichier de sauvegarde récemment formaté, `Z:\SQLServerBackups\AdvWorksData.bak`.  
  
```sql  
--Run backup statement in the gBackup session.  
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'   
WITH   
   FORMAT,   
   MEDIADESCRIPTION='AdventureWorks2012 Compressed Data Backups'  
   DESCRIPTION='First database backup on AdventureWorks2012 Compressed Data Backups media set'  
   COMPRESSION;  
GO  
```  
  
 [&#91;Haut&#93;](#Top)  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et tester une fonction classifieur définie par l’utilisateur](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)  
  
  
