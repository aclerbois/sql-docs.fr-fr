---
title: sp_attach_db (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_attach_db_TSQL
- sp_attach_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_db
ms.assetid: 59bc993e-7913-4091-89cb-d2871cffda95
caps.latest.revision: 69
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1c275128a8e3c2bbce6165486e256fc5b34d402d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spattachdb-transact-sql"></a>sp_attach_db (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Attache une base de données à un serveur.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d’utiliser CREATE DATABASE *nom_base_de_données* pour attacher à la place. Pour plus d’informations, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
> [!NOTE]  
>  Pour reconstruire plusieurs fichiers journaux lorsqu’un ou plusieurs possèdent un nouvel emplacement, utilisez CREATE DATABASE *nom_base_de_données* FOR ATTACH_REBUILD_LOG.  
  
> [!IMPORTANT]  
>  Nous vous recommandons de ne pas attacher ni restaurer de bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_attach_db [ @dbname= ] 'dbname'  
    , [ @filename1= ] 'filename_n' [ ,...16 ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname=** ] **' *** dbnam* **'**  
 Nom de la base de données à attacher au serveur. Le nom doit être unique. *dbname* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@filename1=** ] **'***Nom_de_fichier_n***'**  
 Nom physique, y compris le chemin d'accès, d'un fichier de base de données. *Nom_de_fichier_n* est **nvarchar (260)**, avec NULL comme valeur par défaut. Jusqu'à 16 noms de fichier peuvent être spécifiés. Les noms de paramètre commencent à **@filename1** et **@filename16**. La liste des noms de fichiers doit comprendre au moins le fichier primaire. Le fichier primaire contient les tables système pointant vers d'autres fichiers dans la base de données. Elle doit aussi comprendre tous les fichiers qui ont été déplacés après que la base de données fut détachée.  
  
> [!NOTE]  
>  Cet argument crée un mappage vers le paramètre FILENAME de l'instruction CREATE DATABASE. Pour plus d’informations, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
>   
>  Quand vous attachez une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui contient des fichiers catalogue de texte intégral à une instance de serveur [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , les fichiers catalogue sont attachés à partir de leur emplacement précédent avec les autres fichiers de base de données, les mêmes que dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Le **sp_attach_db** procédure stockée doit être exécutée uniquement sur les bases de données qui ont été préalablement détachées du serveur de base de données à l’aide d’explicite **sp_detach_db** opération ou sur des bases de données copiées. Si vous devez spécifier plus de 16 fichiers, utilisez CREATE DATABASE *nom_base_de_données* FOR ATTACH ou CREATE DATABASE *nom_base_de_données* FOR_ATTACH_REBUILD_LOG. Pour plus d’informations, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
 Tout fichier non spécifié est considéré comme se trouvant à son dernier emplacement identifié. Pour utiliser un fichier à un emplacement différent, vous devez spécifier ce nouvel emplacement.  
  
 Une base de données créée dans une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être attachée à des versions antérieures.  
  
> [!NOTE]  
>  Un instantané de base de données ne peut pas être détaché ni attaché.  
  
 Lorsque vous attachez une base de données répliquée qui a été copiée au lieu d'être détachée, tenez compte des conditions suivantes :  
  
-   Si vous attachez la base de données à la même version et à la même instance de serveur que celles de la base de données d'origine, aucune opération supplémentaire n'est nécessaire.  
  
-   Si vous attachez la base de données à la même instance de serveur alors que sa version a été mise à niveau, vous devez exécuter [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) pour mettre à jour la réplication à la fin de l’opération de rattachement.  
  
-   Si vous attachez la base de données à une instance de serveur différente, sans tenir compte de la version, vous devez exécuter [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) pour supprimer la réplication, une fois l’opération de rattachement effectuée.  
  
 Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés.  
  
## <a name="permissions"></a>Autorisations  
 Pour plus d’informations sur la gestion des autorisations lorsqu’une base de données est attachée, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, des fichiers sont attachés depuis [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] au serveur en cours.  
  
```  
EXEC sp_attach_db @dbname = N'AdventureWorks2012',   
    @filename1 =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf',   
    @filename2 =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
