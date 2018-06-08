---
title: Sys.syscharsets (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.syscharsets
- syscharsets
- sys.syscharsets_TSQL
- syscharsets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscharsets system table
- sys.syscharsets compatibility view
ms.assetid: f16d987c-bd19-4668-9ef7-785b8fb9ff5b
caps.latest.revision: 42
author: rothja
ms.author: jroth
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 05812269c42b4d20a694d8a51362a376ea62d527
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssyscharsets-transact-sql"></a>sys.syscharsets (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne pour chaque jeu de caractères et ordre de tri défini pour être utilisé par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Parmi les ordres de tri est marqué dans **sysconfigures** en tant que l’ordre de tri par défaut. Il s'agit du seul ordre de tri effectivement utilisé.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**type**|**smallint**|Type de l'entité décrite par cette ligne :<br /><br /> 1001 = jeu de caractères.<br /><br /> 2001 = ordre de tri.|  
|**id**|**tinyint**|Numéro d'identification unique du jeu de caractères ou de l'ordre de tri. Notez que les ordres de tri et les jeux de caractères ne peuvent pas avoir le même numéro d'identification. Les identificateurs compris entre 1 et 240 sont réservés à l'utilisation par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**csid**|**tinyint**|Si la ligne représente un jeu de caractères, ce champ n'est pas utilisé. Si elle représente un ordre de tri, ce champ contient le numéro d'identification du jeu de caractères sur lequel cet ordre de tri est construit. Il est supposé dans ce cas qu'une ligne de jeu de caractères contenant ce numéro d'identification figure dans la table.|  
|**status**|**smallint**|Bits d'information d'état à usage interne.|  
|**nom**|**sysname**|Nom unique du jeu de caractères ou de l'ordre de tri. Ce champ peut contenir uniquement des lettres (A-Z ou a-z), des chiffres (0 - 9) et des traits de soulignement (_) ; il doit commencer par une lettre.|  
|**description**|**nvarchar(255)**|Description facultative des caractéristiques du jeu de caractères ou de l'ordre de tri.|  
|**binarydefinition**|**varbinary(6000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Définition**|**image**|Définition interne du jeu de caractères ou de l'ordre de tri. La structure des données de ce champ dépend du type.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
