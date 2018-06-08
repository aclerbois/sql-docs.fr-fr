---
title: Sys.dm_repl_schemas (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_repl_schemas_TSQL
- dm_repl_schemas
- sys.dm_repl_schemas_TSQL
- sys.dm_repl_schemas
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_schemas dynamic management function
ms.assetid: 6f5fefff-8492-4360-bd5b-a97287367914
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f9dfbe8ca8cf3261e898731967aeec34e81c010c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmreplschemas-transact-sql"></a>sys.dm_repl_schemas (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les colonnes de table publiées par la réplication.  
  
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artcache_schema_address**|**varbinary(8)**|Adresse en mémoire de la structure de schéma du cache pour l'article de table publié.|  
|**tabid**|**bigint**|ID de la table répliquée.|  
|**indexid**|**smallint**|ID d'un index cluster sur la table publiée.|  
|**idSch**|**bigint**|ID du schéma de la table.|  
|**tabschema**|**nvarchar(510)**|Nom du schéma de la table.|  
|**ccTabschema**|**smallint**|Longueur en caractères du schéma de la table.|  
|**tabname**|**nvarchar(510)**|Nom de la table publiée.|  
|**ccTabname**|**smallint**|Longueur en caractères du nom de la table publiée.|  
|**rowsetid_delete**|**bigint**|ID de la ligne supprimée.|  
|**rowsetid_insert**|**bigint**|ID de la ligne insérée.|  
|**num_pk_cols**|**int**|Nombre de colonnes clés primaires.|  
|**pcitee**|**binary(8000)**|Pointeur sur la structure d'expression de requête utilisée pour évaluer la colonne calculée.|  
|**re_numtextcols**|**int**|Nombre de colonnes d'objets BLOB (Binary Large Object) dans la table répliquée.|  
|**re_schema_lsn_begin**|**binary(8000)**|Numéro séquentiel dans le journal de départ pour l'enregistrement de la version du schéma.|  
|**re_schema_lsn_end**|**binary(8000)**|Dernier numéro séquentiel dans le journal de la version de schéma.|  
|**re_numcols**|**int**|Nombre de colonnes publiées.|  
|**re_colid**|**int**|Identificateur de colonne au niveau du serveur de publication.|  
|**re_awcName**|**nvarchar(510)**|Nom de la colonne publiée.|  
|**re_ccName**|**smallint**|Nombre de caractères dans le nom de colonne.|  
|**re_pk**|**tinyint**|Indique si la colonne publiée fait partie d'une clé primaire.|  
|**re_unique**|**tinyint**|Indique si la colonne publiée fait partie d'un index unique.|  
|**re_maxlen**|**smallint**|Longueur maximale de la colonne publiée.|  
|**re_prec**|**tinyint**|Précision de la colonne publiée.|  
|**re_scale**|**tinyint**|Échelle de la colonne publiée.|  
|**re_collatid**|**bigint**|ID de classement de la colonne publiée.|  
|**re_xvtype**|**smallint**|Type de la colonne publiée.|  
|**re_offset**|**smallint**|Décalage de la colonne publiée.|  
|**re_bitpos**|**tinyint**|Position binaire de la colonne publiée, dans le vecteur d'octets.|  
|**re_fNullable**|**tinyint**|Indique si la colonne publiée accepte les valeurs NULL.|  
|**re_fAnsiTrim**|**tinyint**|Indique si la troncature ANSI est utilisée sur la colonne publiée.|  
|**re_computed**|**smallint**|Indique si la colonne publiée est une colonne calculée.|  
|**se_rowsetid**|**bigint**|ID de l'ensemble de lignes.|  
|**se_schema_lsn_begin**|**binary(8000)**|Numéro de séquence d'enregistrement de départ pour l'enregistrement de la version du schéma.|  
|**se_schema_lsn_end**|**binary(8000)**|Dernier numéro séquentiel dans le journal de la version de schéma.|  
|**se_numcols**|**int**|Nombre de colonnes.|  
|**se_colid**|**int**|ID de la colonne au niveau de l'abonné.|  
|**se_maxlen**|**smallint**|Longueur maximale de la colonne.|  
|**se_prec**|**tinyint**|Précision de la colonne.|  
|**se_scale**|**tinyint**|Échelle de la colonne.|  
|**se_collatid**|**bigint**|ID de classement de la colonne.|  
|**se_xvtype**|**smallint**|Type de la colonne.|  
|**se_offset**|**smallint**|Décalage de la colonne.|  
|**se_bitpos**|**tinyint**|Position binaire de la colonne, dans le vecteur d'octets.|  
|**se_fNullable**|**tinyint**|Spécifie si la colonne prend en charge les valeurs NULL.|  
|**se_fAnsiTrim**|**tinyint**|Indique si la troncature ANSI est utilisée sur la colonne.|  
|**se_computed**|**smallint**|Indique si la colonne est une colonne calculée.|  
|**se_nullBitInLeafRows**|**int**|Indique si la valeur de la colonne est NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation VIEW DATABASE STATE sur la base de données de publication pour appeler **dm_repl_schemas**.  
  
## <a name="remarks"></a>Notes  
 Les informations ne sont retournées que pour les objets de base de données répliqués actuellement chargés dans le cache des articles de réplication.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la réplication &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

