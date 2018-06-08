---
title: Sys.database_recovery_status (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- database_recovery_status_TSQL
- database_recovery_status
- sys.database_recovery_status
- sys.database_recovery_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_recovery_status catalog view
ms.assetid: 46fab234-1542-49be-8edf-aa101e728acf
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2cdefec3d2f5ffd6a8ce326c4d3afd78df47de44
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabaserecoverystatus-transact-sql"></a>sys.database_recovery_status (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne par base de données. Si la base de données n'est pas ouverte, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] tente de la démarrer.  
  
 Pour afficher la ligne pour une base de données autre que **master** ou **tempdb**, une des options suivantes doit s’appliquer :  
  
-   Être le propriétaire de la base de données.  
  
-   Posséder des autorisations au niveau du serveur ALTER ANY DATABASE ou VIEW ANY DATABASE.  
  
-   Avoir l’autorisation CREATE DATABASE dans le **master** base de données.    
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données, unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**database_guid**|**uniqueidentifier**|Utilisé pour associer ensemble tous les fichiers de base de données d'une base de données. Tous les fichiers possèdent ce GUID dans leur page d'en-tête afin que la base de données démarre comme prévu. Une seule base de données doit posséder ce GUID, mais des doublons peuvent être créés en copiant et en joignant des bases de données. RESTORE génère toujours un nouveau GUID lorsque vous restaurez une base de données qui n'existe pas encore.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**family_guid**|**uniqueidentifier**|Identificateur de la « famille de sauvegarde » de la base de données pour détecter les états de restauration correspondants.<br /><br /> NULL = base de données est hors connexion ou de la base de données ne démarrera pas.|  
|**last_log_backup_lsn**|**numeric(25,0)**|Numéro de séquence journal début de la prochaine sauvegarde du journal.<br /><br /> Si NULL, un sauvegarde du journal des transactions jusqu'à ne peut pas être effectué, car il n’existe aucune sauvegarde de base de données ou la base de données est en mode de récupération SIMPLE.|  
|**recovery_fork_guid**|**uniqueidentifier**|Identifie la fourchette de récupération en cours sur laquelle la base de données est actuellement active.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**first_recovery_fork_guid**|**uniqueidentifier**|Identificateur de la fourchette de récupération de début.<br /><br /> NULL= Base de données hors connexion, ou la base de données ne démarrera pas.|  
|**fork_point_lsn**|**numeric(25,0)**|Si **first_recovery_fork_guid** n’est pas égal ( ! =) pour **recovery_fork_guid**, **fork_point_lsn** est le numéro de séquence de journal du point de branchement actuel. Dans le cas contraire, la valeur est NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
