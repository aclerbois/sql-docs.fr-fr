---
title: Sys.fn_virtualfilestats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_virtualfilestats_TSQL
- fn_virtualfilestats
dev_langs:
- TSQL
helpviewer_keywords:
- I/O [SQL Server], statistics
- fn_virtualfilestats function
- sys.fn_virtualfilestats function
- statistical information [SQL Server], I/O
ms.assetid: 96b28abb-b059-48db-be2b-d60fe127f6aa
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 396eee771ece7036906d1ef8e09cc69c1ab2c1da
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnvirtualfilestats-transact-sql"></a>sys.fn_virtualfilestats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des statistiques sur les entrées/sorties (E/S) des fichiers de base de données, notamment sur les fichiers journaux. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ces informations sont également disponibles à partir de la [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) vue de gestion dynamique.  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_virtualfilestats ( { database_id | NULL } , { file_id | NULL } )  
```  
  
## <a name="arguments"></a>Arguments  
 *database_id* | VALEUR NULL  
 Est l’ID de la base de données. *database_id* est de type **int**, sans valeur par défaut. Spécifiez NULL pour retourner des informations concernant toutes les bases de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *FILE_ID* | VALEUR NULL  
 Identificateur du fichier. *FILE_ID* est **int**, sans valeur par défaut. Spécifiez NULL pour retourner les informations de tous les fichiers de la base de données.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**DbId**|**smallint**|ID de la base de données.|  
|**FileId**|**smallint**|ID de fichier.|  
|**TimeStamp**|**bigint**|Horodateur de prélèvement des données de base de données **int** dans les versions antérieures [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)]. |  
|**NumberReads**|**bigint**|Nombre de lectures effectuées sur le fichier.|  
|**BytesRead**|**bigint**|Nombre d'octets lus sur le fichier|  
|**IoStallReadMS**|**bigint**|Durée totale (en millisecondes) d'exécution des E/S de lecture sur le fichier|  
|**NumberWrites**|**bigint**|Nombre d'écritures effectuées sur le fichier|  
|**BytesWritten**|**bigint**|Nombre d'octets écrits sur le fichier|  
|**IoStallWriteMS**|**bigint**|Durée totale (en millisecondes) d'exécution des E/S d'écriture sur le fichier|  
|**IoStallMS**|**bigint**|Somme de **IoStallReadMS** et **IoStallWriteMS**.|  
|**FileHandle**|**bigint**|Valeur du handle de fichier.|  
|**BytesOnDisk**|**bigint**|Taille physique du fichier sur le disque (en octets).<br /><br /> Pour les fichiers de base de données, il est la même valeur que **taille** dans **sys.database_files**, mais est exprimée en octets plutôt que des pages.<br /><br /> Dans le cas des fichiers partiellement alloués d'instantané de base de données, il s'agit de l'espace qu'utilise le système d'exploitation pour ceux-ci.|  
  
## <a name="remarks"></a>Notes  
 **fn_virtualfilestats** est un système de fonction table qui fournit des informations statistiques, telles que le nombre total d’e/s effectuée dans un fichier. Cette fonction vous permet d'enregistrer et de suivre la durée d'attente de l'utilisateur avant de pouvoir lire ou écrire dans un fichier. Cette fonction permet également d'identifier les fichiers dont l'activité est intense au niveau des entrées/sorties (E/S).  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-statistical-information-for-a-database"></a>A. Affichage des informations statistiques d'une base de données  
 L'exemple suivant affiche les informations statistiques de l'ID de fichier 1 de la base de données dont l'ID est `1`.  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(1, 1);  
GO  
```  
  
### <a name="b-displaying-statistical-information-for-a-named-database-and-file"></a>B. Affichage des informations statistiques d'une base de données et d'un fichier nommés  
 L'exemple suivant affiche les informations statistiques du fichier journal de l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La fonction système `DB_ID` est utilisé pour spécifier le *database_id* paramètre.  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="c-displaying-statistical-information-for-all-databases-and-files"></a>C. Affichage des informations statistiques de la totalité des bases de données et des fichiers  
 L'exemple suivant affiche les informations statistiques de tous les fichiers de toutes les bases de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(NULL,NULL);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)   
 [FILE_IDEX &#40;Transact-SQL&#41;](../../t-sql/functions/file-idex-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
