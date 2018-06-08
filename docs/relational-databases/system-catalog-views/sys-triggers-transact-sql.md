---
title: Sys.triggers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- triggers
- triggers_TSQL
- sys.triggers
- sys.triggers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.triggers catalog view
ms.assetid: cefa4fc4-b8b9-4cd7-b124-eed5283acbfc
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7c176e5cdf68b5aa9516cd054a57f36c630b0c64
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="systriggers-transact-sql"></a>sys.triggers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Contient une ligne pour chaque objet qui est un déclencheur de type TR ou TA. Les noms de déclencheurs DML ont une portée de schéma et, par conséquent, sont visibles dans **sys.objects**. Les noms de déclencheurs DDL ont une portée définie par l'entité parente et ils sont visibles uniquement dans cette vue.  
  
 Le **parent_class** et **nom** colonnes qui identifient de façon unique le déclencheur dans la base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom du déclencheur. Les noms de déclencheurs DML ont une portée de schéma. Les noms de déclencheurs DDL ont une portée définie par rapport à l'entité parente.|  
|**object_id**|**int**|Numéro d'identification de l'objet. Unique dans une base de données.|  
|**parent_class**|**tinyint**|Classe du parent du déclencheur.<br /><br /> 0 = Base de données, pour les déclencheurs DDL.<br /><br /> 1 = Objet ou colonne pour les déclencheurs DML.|  
|**parent_class_desc**|**nvarchar(60)**|Description de la classe parente du déclencheur.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|ID du parent du déclencheur, comme suit :<br /><br /> 0 = Déclencheurs apparentés à une base de données.<br /><br /> Pour les déclencheurs DML, il s’agit du **object_id** de la table ou vue sur laquelle le déclencheur DML est défini.|  
|**type**|**char(2)**|Type d’objet :<br /><br /> TA = Déclencheur assembly (CLR)<br /><br /> TR = Déclencheur SQL|  
|**type_desc**|**nvarchar(60)**|Description du type d’objet.<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**create_date**|**datetime**|Date de création du déclencheur.|  
|**modify_date**|**datetime**|Date de la dernière modification de l'objet avec l'instruction ALTER.|  
|**is_ms_shipped**|**bit**|Déclencheur créé pour l'utilisateur par un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interne.|  
|**is_disabled**|**bit**|Déclencheur désactivé.|  
|**is_not_for_replication**|**bit**|Déclencheur créé sous la forme NOT FOR REPLICATION.|  
|**is_instead_of_trigger**|**bit**|1 = Déclencheurs INSTEAD OF<br /><br /> 0 = Déclencheurs AFTER|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
