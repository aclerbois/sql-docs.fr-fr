---
title: Sys.master_files (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.master_files
- master_files_TSQL
- sys.master_files_TSQL
- master_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_files catalog view
ms.assetid: 803b22f2-0016-436b-a561-ce6f023d6b6a
caps.latest.revision: 56
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cd7c2b9aac08fe6133c2138f5a1c2ea5369ec34c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmasterfiles-transact-sql"></a>sys.master_files (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-pdw-md.md)]

  Contient une ligne, par fichier d'une base de données, stockée dans la base de données master. Il s'agit d'une vue système unique.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données à laquelle ce fichier s'applique. Le masterdatabase_id est toujours 1.|  
|file_id|**int**|ID du fichier dans la base de données. Le file_id principal est toujours 1.|  
|file_guid|**uniqueidentifier**|Identificateur unique du fichier.<br /><br /> NULL = la base de données a été mise à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|Type|**tinyint**|Type de fichier :<br /><br /> 0 = Lignes.<br /><br /> 1 = journal<br /><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = Texte intégral (le catalogue de texte intégral antérieur à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ; les catalogues de texte intégral qui sont mis à niveau ou créés dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou version ultérieure signaleront un type de fichier 0.)|  
|type_desc|**nvarchar(60)**|Description du type de fichier :<br /><br /> ROWS<br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT (catalogues de texte intégral antérieurs à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].)|  
|data_space_id|**int**|ID de l'espace de données auquel ce fichier appartient. L'espace de données est un groupe de fichiers.<br /><br /> 0 = fichiers journaux|  
|name|**sysname**|Nom logique du fichier dans la base de données.|  
|physical_name|**nvarchar(260)**|Nom de fichier du système d'exploitation.|  
|state|**tinyint**|État du fichier :<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|state_desc|**nvarchar(60)**|Description de l'état du fichier :<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Pour plus d’informations, consultez [États des fichiers](../../relational-databases/databases/file-states.md).|  
|size|**int**|Taille actuelle du fichier, en pages de 8 Ko. Dans le cas d'un instantané de base de données, size reflète l'espace maximal que celle-ci peut utiliser pour le fichier.<br /><br /> Remarque : Ce champ est rempli en tant que zéro pour les conteneurs FILESTREAM. Requête le *sys.database_files* affichage pour la taille réelle de conteneurs FILESTREAM catalogue.|  
|max_size|**int**|Taille maximale du fichier, en pages de 8 Ko :<br /><br /> 0 = aucune croissance n'est autorisée.<br /><br /> -1 = Le fichier peut croître tant que le disque n'est pas saturé.<br /><br /> 268435456 = Le fichier journal peut croître pour atteindre une taille maximale de 2 To.<br /><br /> Remarque : Les bases de données qui sont mis à niveau avec une taille de fichier journal illimitée signalera -1 pour la taille maximale du fichier journal.|  
|growth|**int**|0 = la taille du fichier est fixe et celui-ci ne croît pas.<br /><br /> >0 = le fichier croît automatiquement.<br /><br /> Si is_percent_growth a pour valeur 0, l'incrément de croissance est exprimé en unités de pages de 8 Ko, arrondies aux 64 Ko les plus proches.<br /><br /> Si is_percent_growth a pour valeur 1, l'incrément de croissance est exprimé sous la forme d'un pourcentage à nombre entier.|  
|is_media_read_onlyF|**bit**|1 = le fichier se trouve sur un support en lecture seule.<br /><br /> 0 = le fichier se trouve sur un support en lecture/écriture.|  
|is_read_only|**bit**|1 = le fichier est marqué comme étant accessible en lecture seule.<br /><br /> 1 = le fichier est marqué comme étant accessible en lecture/écriture.|  
|is_sparse|**bit**|1 = le fichier est un fichier partiellement alloué.<br /><br /> 0 = le fichier n'est pas un fichier partiellement alloué.<br /><br /> Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|is_percent_growth|**bit**|1 = la croissance du fichier est exprimée en pourcentage.<br /><br /> 0 = importance de croissance absolue en pages.|  
|is_name_reserved|**bit**|1 = le nom de fichier supprimé est réutilisable. Avant de réutiliser le nom (name ou physical_name) pour un nouveau nom de fichier, vous devez réaliser une sauvegarde de fichier journal.<br /><br /> 0 = le nom de fichier n'est pas réutilisable.|  
|create_lsn|**numeric(25,0)**|Numéro séquentiel dans le journal (LSN) auquel le fichier a été créé.|  
|drop_lsn|**numeric(25,0)**|LSN auquel le fichier a été supprimé.|  
|read_only_lsn|**numeric(25,0)**|LSN auquel le groupe de fichiers qui contient le fichier est passé de l'état lecture/écriture à l'état lecture seule (changement le plus récent).|  
|read_write_lsn|**numeric(25,0)**|LSN auquel le groupe de fichiers qui contient le fichier est passé de l'état lecture seule à l'état lecture/écriture (changement le plus récent).|  
|differential_base_lsn|**numeric(25,0)**|Base des sauvegardes différentielles. Les étendues de données modifiées après ce LSN sont incluses dans une sauvegarde différentielle.|  
|differential_base_guid|**uniqueidentifier**|Identificateur unique de la sauvegarde de base sur laquelle repose une sauvegarde différentielle.|  
|differential_base_time|**datetime**|Heure correspondant à differential_base_lsn.|  
|redo_start_lsn|**numeric(25,0)**|LSN auquel doit démarrer la restauration par progression suivante.<br /><br /> A pour valeur NULL sauf si state a pour valeur RESTORING ou RECOVERY_PENDING.|  
|redo_start_fork_guid|**uniqueidentifier**|Identificateur unique du branchement de récupération. Le first_fork_guid de la prochaine sauvegarde de fichier journal restaurée doit correspondre à cette valeur. Celle-ci représente l'état actuel du conteneur.|  
|redo_target_lsn|**numeric(25,0)**|LSN auquel peut s'arrêter la restauration par progression en ligne sur ce fichier.<br /><br /> A pour valeur NULL sauf si state a pour valeur RESTORING ou RECOVERY_PENDING.|  
|redo_target_fork_guid|**uniqueidentifier**|Branchement de récupération sur lequel le conteneur peut être récupéré. Associé à redo_target_lsn.|  
|backup_lsn|**numeric(25,0)**|LSN de la sauvegarde de données ou différentielle du fichier la plus récente.|  
|credential_id|**int**|Le `credential_id` de `sys.credentials` utilisé pour stocker le fichier. Par exemple, lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution sur une Machine virtuelle Azure et de la base de données de fichiers sont stockés dans le stockage d’objets blob Azure, informations d’identification sont configurée avec les informations d’identification de l’accès à l’emplacement de stockage.|  
  
> [!NOTE]  
>  Lorsque vous supprimez ou reconstruisez des index volumineux ou lorsque vous supprimez ou tronquez des tables volumineuses, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations des pages actives et de leurs blocs associés jusqu'à ce que la transaction soit validée. Les opérations de suppression différées ne libèrent pas immédiatement l'espace alloué. Par conséquent, dès qu'un objet volumineux est supprimé ou tronqué, les valeurs retournées par sys.master_files peuvent ne pas refléter l'espace disque réellement disponible.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations minimales requises pour consulter la ligne correspondante sont les autorisations CREATE DATABASE, ALTER ANY DATABASE ou VIEW ANY DEFINITION.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [États des fichiers](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
