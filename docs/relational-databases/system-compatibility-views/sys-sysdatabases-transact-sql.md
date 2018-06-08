---
title: Sys.sysdatabases (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysdatabases_TSQL
- sys.sysdatabases_TSQL
- sys.sysdatabases
- sysdatabases
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysdatabases compatibility view
- sysdatabases system table
ms.assetid: 60a93880-62f1-4eda-a886-f046706ba90c
caps.latest.revision: 35
author: rothja
ms.author: jroth
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9a19ff47d576a7a2ffe5a72f609a27d5c1214f70
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssysdatabases-transact-sql"></a>sys.sysdatabases (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Contient une ligne pour chaque base de données dans une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est tout d’abord installé, **sysdatabases** contient des entrées pour le **master**, **modèle**, **msdb**, et **tempdb** bases de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom de la base de données|  
|**dbid**|**smallint**|ID de la base de données|  
|**sid**|**varbinary(85)**|ID système du créateur de la base de données.|  
|**Mode**|**smallint**|Champ utilisé de manière interne pour verrouiller une base de données pendant sa création.|  
|**status**|**int**|Bits d’état, certains d'entre eux peuvent être définis à l’aide de [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) comme indiqué :<br /><br /> 1 = **autoclose** (ALTER DATABASE)<br /><br /> 4 = **sélectionner dans / bulkcopy** (ALTER DATABASE via SET RECOVERY)<br /><br /> 8 = **trunc. log sur chkpt** (ALTER DATABASE via SET RECOVERY)<br /><br /> 16 = **détection de page endommagée** (ALTER DATABASE)<br /><br /> 32 = **chargement**<br /><br /> 64 = **avant la restauration**<br /><br /> 128 = **récupération**<br /><br /> 256 = **ne pas récupérés**<br /><br /> 512 = **hors connexion** (ALTER DATABASE)<br /><br /> 1024 = **en lecture seule** (ALTER DATABASE)<br /><br /> 2048 = **utilisation dbo uniquement** (ALTER DATABASE via SET RESTRICTED_USER)<br /><br /> 4096 = **mono-utilisateur** (ALTER DATABASE)<br /><br /> 32768 = **mode d’urgence**<br /><br /> 65536 = **SOMME DE CONTRÔLE** (ALTER DATABASE)<br /><br /> 4194304 = **autoshrink** (ALTER DATABASE)<br /><br /> 1073741824 = **arrêtée**<br /><br /> Plusieurs bits peuvent être activés à la fois.|  
|**status2**|**int**|16384 = **par défaut ANSI null** (ALTER DATABASE)<br /><br /> 65536 = **concaténation de null donne null** (ALTER DATABASE)<br /><br /> 131072 = **les déclencheurs récursifs** (ALTER DATABASE)<br /><br /> 1048576 = **curseur local par défaut** (ALTER DATABASE)<br /><br /> 8388608 = **identificateur entre guillemets** (ALTER DATABASE)<br /><br /> 33554432 = **fermer le curseur lors de la validation** (ALTER DATABASE)<br /><br /> 67108864 = **ANSI nulls** (ALTER DATABASE)<br /><br /> 268435456 = **ANSI warnings** (ALTER DATABASE)<br /><br /> 536870912 = **activé de texte intégral** (défini à l’aide de **sp_fulltext_database**)|  
|**crdate**|**datetime**|Date de création|  
|**Réservé**|**datetime**|Réservé pour un usage ultérieur.|  
|**Catégorie**|**int**|Contient un bitmap des informations utilisées pour la réplication :<br /><br /> 1 = Publié pour la réplication transactionnelle et d'instantané.<br /><br /> 2 = Abonné à une publication transactionnelle ou d'instantané.<br /><br /> 4 = Publié pour une réplication de fusion.<br /><br /> 8 = Abonné à une publication de fusion.<br /><br /> 16 = Base de données de distribution.|  
|**cmptlevel**|**tinyint**|Niveau de compatibilité pour la base de données. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**filename**|**nvarchar(260)**|Nom et chemin d'accès du système d'exploitation pour le fichier primaire de la base de données.<br /><br /> **nom de fichier** est visible pour **dbcreator**, **sysadmin**, le propriétaire de la base de données avec les autorisations CREATE ANY DATABASE ou les bénéficiaires de l’une des autorisations suivantes : ALTER ANY DATABASE CRÉER UNE BASE DE DONNÉES, VIEW ANY DEFINITION. Pour retourner le chemin d’accès et nom de fichier, interrogez la [sys.sysfiles](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md) vue de compatibilité ou la [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) vue.|  
|**version**|**smallint**|Numéro de version interne du code [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec lequel la base de données a été créée. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
