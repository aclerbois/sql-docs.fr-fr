---
title: DROP DATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/15/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
ms.assetid: 477396a9-92dc-43c9-9b97-42c8728ede8e
caps.latest.revision: 83
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 47aa7d947dfeafcad80c74514e1861d44205389c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Supprime un ou plusieurs bases de données utilisateur ou instantanés de base de données à partir d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- SQL Server Syntax  
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]  
```  
  
```  
-- Azure SQL Database, Azure SQL Data Warehouse and Parallel Data Warehouse Syntax   
DROP DATABASE database_name [;]  
```  
  
## <a name="arguments"></a>Arguments  
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Supprime, de manière conditionnelle, la base de données uniquement si elle existe déjà.  
  
 *database_name*  
 Spécifie le nom de la base de données à supprimer. Pour afficher une liste de bases de données, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 *database_snapshot_name*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie le nom d'un instantané de base de données à supprimer.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Une base de données peut être supprimée quel que soit son statut : hors connexion, en lecture seule, suspect, etc. Pour afficher l’état actuel d’une base de données, utilisez la vue de catalogue **sys.databases**.  
  
 Une base de données supprimée ne peut être recréée que par la restauration d'une copie de sauvegarde. Les instantanés de base de données ne peuvent pas être sauvegardés et ne peuvent donc pas être restaurés.  
  
 Quand vous supprimez une base de données, vous devez effectuer une sauvegarde de la [base de données MASTER](../../relational-databases/databases/master-database.md).  
  
 Si vous supprimez une base de données, celle-ci l'est également d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il en est de même pour les fichiers disque physiques utilisés par la base de données. Si la base de données ou l'un de ses fichiers est hors connexion lors de la suppression, les fichiers disque ne sont pas supprimés. Ces fichiers peuvent être supprimés manuellement à l'aide de l'Explorateur Windows. Pour supprimer une base de données du serveur actif sans supprimer les fichiers du système de fichiers, utilisez [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).  
  
> [!WARNING]  
>  Vous pouvez supprimer une base de données qui est associée à des sauvegardes FILE_SNAPSHOT, mais les fichiers de base de données qui sont associés à des instantanés ne sont pas supprimés pour éviter l’invalidation des sauvegardes qui font référence à ces fichiers. Le fichier est tronqué, mais il n’est pas supprimé physiquement afin de conserver les sauvegardes FILE_SNAPSHOT. Pour plus d’informations, consultez [Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658).  
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Si vous supprimez un instantané de base de données, celui-ci l'est également d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il en est de même pour les fichiers partiellement alloués du système de fichiers physiques NTFS utilisés par l'instantané. Pour plus d’informations sur l’utilisation des fichiers partiellement alloués par les instantanés de base de données, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md). La suppression d'un instantané de base de données efface le cache de plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations de maintenance ou de reconfiguration de base de données ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.  
  
## <a name="interoperability"></a>Interopérabilité  
  
### <a name="sql-server"></a>SQL Server  
 Pour supprimer une base de données publiée à des fins de réplication transactionnelle, ou bien une base de données publiée ou abonnée à une réplication de fusion, vous devez d'abord supprimer sa réplication. Si une base de données est endommagée, si vous ne pouvez pas supprimer la réplication dans un premier temps ou si les deux cas de figure se présentent, vous pouvez toujours, dans la plupart des cas, supprimer la base de données à l'aide de l'instruction ALTER DATABASE pour la déconnecter, puis la supprimer.  
  
 Si la base de données intervient dans l'envoi de journaux, supprimez l'envoi de journaux avant de supprimer la base de données. Pour plus d’informations, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Les [bases de données système](../../relational-databases/databases/system-databases.md) ne peuvent pas être supprimées.  
  
 L'instruction DROP DATABASE doit être exécutée en mode autocommit et elle n'est pas autorisée dans une transaction implicite ou explicite. Le mode autocommit est le mode par défaut pour la gestion des transactions.  
  
 Vous ne pouvez pas supprimer une base de données en cours d'utilisation, c'est-à-dire ouverte par un utilisateur pour une opération de lecture ou d'écriture. Une façon de supprimer des utilisateurs de la base de données consiste à utiliser ALTER DATABASE pour affecter à la base de données la valeur SINGLE_USER. 
 
 >[!Warning] 
 > Cette approche n’est pas infaillible, car la première connexion consécutive établie par un thread reçoit le thread SINGLE_USER, ce qui provoque l’échec de votre connexion. SQL Server ne fournit pas de méthode intégrée pour supprimer des bases de données lors du chargement.
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Les instantanés de base de données doivent être supprimés avant que la base de données ne soit supprimée.  
  
 La suppression d’une base de données activée pour Stretch Database ne supprime pas les données distantes. Si vous souhaitez supprimer les données distantes, vous devez le faire manuellement.  
  
### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
 Vous devez être connecté à la base de données master pour supprimer une base de données.
  
 L'instruction DROP DATABASE doit être la seule instruction d'un traitement SQL et vous pouvez supprimer une seule base de données à la fois.
  
### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  
 Vous devez être connecté à la base de données master pour supprimer une base de données.
  
 L'instruction DROP DATABASE doit être la seule instruction d'un traitement SQL et vous pouvez supprimer une seule base de données à la fois.

## <a name="permissions"></a>Autorisations  
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Nécessite l’autorisation **CONTROL** sur la base de données, ou l’autorisation **ALTER ANY DATABASE**, ou l’appartenance au rôle de base de données fixe **db_owner**.  
  
### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
 Seule la connexion principale au niveau du serveur (créée par le processus de provisionnement) ou les membres du rôle de base de données **dbmanager** peuvent supprimer une base de données.  
  
### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nécessite l’autorisation **CONTROL** sur la base de données, ou l’autorisation **ALTER ANY DATABASE**, ou l’appartenance au rôle de base de données fixe **db_owner**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-a-single-database"></a>A. Suppression d'une base de données unique  
 L'exemple suivant supprime la base de données `Sales`.  
  
```  
DROP DATABASE Sales;  
```  
  
### <a name="b-dropping-multiple-databases"></a>B. Suppression de plusieurs bases de données  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 L'exemple suivant supprime chacune des bases de données répertoriées.  
  
```  
DROP DATABASE Sales, NewSales;  
```  
  
### <a name="c-dropping-a-database-snapshot"></a>C. Suppression d'un instantané de base de données  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 L’exemple suivant supprime un instantané de base de données, nommé `sales_snapshot0600`, sans affecter la base de données source.  
  
```  
DROP DATABASE sales_snapshot0600;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
  
