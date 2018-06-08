---
title: sp_detach_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2015
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_detach_db
- sp_detach_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_db
- detaching databases [SQL Server]
ms.assetid: abcb1407-ff78-4c76-b02e-509c86574462
caps.latest.revision: 86
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c0f17581782cea310bcfad9ec6d7ce4823d1d38c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdetachdb-transact-sql"></a>sp_detach_db (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Détache d'une instance de serveur une base de données qui n'est pas en cours d'utilisation, et exécute accessoirement UPDATE STATISTICS sur toutes les tables avant de détacher la base de données.  
  
> [!IMPORTANT]  
>  Pour détacher une base de données répliquée, celle-ci ne doit pas être publiée. Pour plus d’informations, consultez la section « Remarques » plus loin dans cette rubrique.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_detach_db [ @dbname= ] 'database_name'   
    [ , [ @skipchecks= ] 'skipchecks' ]   
    [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname =** ] **'***nom_base_de_données***'**  
 Nom de la base de données à détacher. *database_name* est un **sysname** valeur, avec NULL comme valeur par défaut.  
  
 [  **@skipchecks =** ] **'***skipchecks***'**  
 Indique si UPDATE STATISTIC doit être ignoré ou exécuté. *option skipchecks* est un **nvarchar (10)** valeur, avec NULL comme valeur par défaut. Pour ignorer UPDATE STATISTICS, spécifiez **true**. Pour exécuter explicitement UPDATE STATISTICS, spécifiez **false**.  
  
 Par défaut, UPDATE STATISTICS est exécuté pour mettre à jour les informations relatives aux données des tables et des index. L'exécution de UPDATE STATISTICS est utile pour les bases de données qui doivent être placées sur des supports en lecture seule.  
  
 [  **@keepfulltextindexfile=** ] **'***KeepFulltextIndexFile***'**  
 Spécifie que le fichier d'index de texte intégral associé à la base de données à détacher ne sera pas supprimé pendant l'opération de détachement de la base de données. *KeepFulltextIndexFile* est un **nvarchar (10)** valeur par défaut de **true**. Si *KeepFulltextIndexFile* est **false**, tous les fichiers d’index de texte intégral associé à la base de données et les métadonnées de l’index de recherche en texte intégral sont supprimés, sauf si la base de données est en lecture seule. Si NULL ou **true**, liée au texte intégral les métadonnées sont conservées.  
  
> [!IMPORTANT]  
>  Le**@keepfulltextindexfile** paramètre sera supprimé dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ce paramètre dans de nouveaux travaux de développement, et modifiez dès que possible les applications qui utilisent actuellement ce paramètre.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Lorsqu'une base de données est détachée, toutes ses métadonnées sont supprimées. Si la base de données a été la base de données par défaut de tous les comptes de connexion **master** devient leur base de données par défaut.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’affichage de la base de données par défaut de tous les comptes de connexion, consultez [sp_helplogins &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md). Si vous avez les autorisations requises, vous pouvez utiliser [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) pour attribuer une nouvelle base de données par défaut à une connexion.  
  
## <a name="restrictions"></a>Restrictions  
 Une base de données ne peut pas être détachée si une des conditions suivantes est vraie :  
  
-   La base de données est en cours d'utilisation. Pour plus d'informations, consultez « Obtention d'un accès exclusif », plus loin dans cette rubrique.  
  
-   Si la base de données est répliquée, elle est publiée.  
  
     Avant de pouvoir détacher la base de données, vous devez désactiver la publication en exécutant [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    >  Si vous ne pouvez pas utiliser **sp_replicationdboption**, vous pouvez supprimer la réplication en exécutant [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Un instantané existe sur la base de données.  
  
     Avant de pouvoir détacher la base de données, vous devez supprimer tous ses instantanés. Pour plus d’informations, consultez [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
    > [!NOTE]  
    >  Un instantané de base de données ne peut pas être détaché ni attaché.  
  
-   La base de données fait l'objet d'une mise en miroir.  
  
     La base de données ne peut pas être détachée tant que la session de mise en miroir de la base de données n'a pas été interrompue. Pour plus d’informations, consultez [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   La base de données est suspecte.  
  
     Vous devez placer une base de données suspecte en mode urgence avant de pouvoir la détacher. Pour plus d’informations sur la manière de mettre une base de données en mode urgence, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   La base de données est une base de données système.  
  
## <a name="obtaining-exclusive-access"></a>Obtention d'un accès exclusif  
 Un accès exclusif à la base de données est nécessaire pour procéder au détachement de la base de données. Si la base de données que vous voulez détacher est en cours d'utilisation, avant de procéder à son détachement, passez-la en mode SINGLE_USER pour obtenir un accès exclusif.  
  
 Par exemple, `ALTER DATABASE` instruction Obtient un accès exclusif à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] une fois que tous les utilisateurs actuels vous déconnecter de la base de données de base de données.  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER;  
GO  
```  
  
> [!NOTE]  
>  Pour forcer actuelle aux utilisateurs en dehors de la base de données immédiatement ou après un nombre spécifié de secondes, également utilisent l’option ROLLBACK : ALTER DATABASE *nom_base_de_données* SET SINGLE_USER WITH ROLLBACK *rollback_option*. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="reattaching-a-database"></a>Rattachement d'une base de données  
 Les fichiers détachés restent et peuvent être rattachés à l'aide de CREATE DATABASE (avec l'option FOR ATTACH ou FOR ATTACH_REBUILD_LOG). Vous pouvez les déplacer sur un autre serveur et les y attacher.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **sysadmin** rôle serveur fixe ou l’appartenance à la **db_owner** rôle de la base de données.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant détache la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] avec la base de données *skipchecks* défini sur true.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
 L'exemple suivant détache la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et conserve les fichiers d'index de texte intégral et les métadonnées de l'index de texte intégral. Cette commande exécute UPDATE STATISTICS, ce qui correspond au comportement par défaut.  
  
```  
exec sp_detach_db @dbname='AdventureWorks2012'  
    , @keepfulltextindexfile='true';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [Détacher une base de données](../../relational-databases/databases/detach-a-database.md)  
  
  
