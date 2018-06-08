---
title: Sys.sequences (Transact-SQL) | Documents Microsoft
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
- sys.sequences_TSQL
- sys.sequences
- sequences
- sequences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sys. sequences catalog view
- sys.sequences catalog view
ms.assetid: 0e1b0e32-1cce-40f7-83c8-860ec660138a
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 22284069373e5eed5786486c6e1e49409d3ea14c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssequences-transact-sql"></a>sys.sequences (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Contient une ligne pour chaque objet séquence dans une base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|\<héritée de colonnes >||Hérite de toutes les colonnes de [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**start_value**|**sql_variant non NULL**|Valeur de départ pour l'objet séquence. Si l'objet séquence est redémarré à l'aide d'ALTER SEQUENCE, il redémarrera à cette valeur. Lorsque l’objet séquence cycles il passe à la **minimum_value** ou **maximum_value**, et non le **start_value**.|  
|**increment**|**sql_variant non NULL**|Valeur utilisée pour incrémenter l'objet séquence après chaque valeur générée.|  
|**minimum_value**|**sql_variant NULL**|Valeur minimale qui peut être générée par l'objet séquence. Une fois cette valeur est atteinte, l’objet séquence retourne soit une erreur lorsque vous tentez de générer des valeurs plus ou si l’option CYCLE est spécifiée. Si aucun MINVALUE n'a été spécifié, cette colonne retourne la valeur minimale prise en charge par le type de données du générateur de séquence.|  
|**maximum_value**|**sql_variant NULL**|Valeur maximale qui peut être générée par l'objet séquence. Une fois cette valeur atteinte, l'objet séquence retourne une erreur lors de la tentative de génération de plus de valeurs ou redémarre si l'option CYCLE est spécifiée. Si aucun MAXVALUE n'a été spécifié, cette colonne retourne la valeur maximale prise en charge par le type de données de l'objet séquence.|  
|**is_cycling**|**bit non NULL**|Retourne 0 si NO CYCLE a été spécifié pour l'objet séquence et 1 si CYCLE a été spécifié.|  
|**is_cached**|**bit non NULL**|Retourne 0 si NO CACHE a été spécifié pour l'objet séquence et 1 si CACHE a été spécifié.|  
|**cache_size**|**int NULL**|Retourne la taille de cache spécifiée pour l'objet séquence. Cette colonne contient NULL si la séquence a été créée avec l'option NO CACHE ou si CACHE a été spécifié sans indication de taille de cache. Si la valeur spécifiée par la taille du cache est supérieure au nombre maximal de valeurs qui peuvent être retournées par l'objet séquence, cette taille du cache impossible à obtenir est toujours affichée.|  
|**system_type_id**|**tinyint non NULL**|ID du type de système pour le type de données de l'objet séquence.|  
|**user_type_id**|**int non NULL**|ID du type de données pour l'objet séquence, comme défini par l'utilisateur.|  
|**precision**|**tinyint non NULL**|Précision maximale du type de données.|  
|**scale**|**tinyint non NULL**|Échelle maximale du type. L'échelle est retournée avec la précision afin de donner des métadonnées complètes aux utilisateurs. L'échelle est toujours de 0 pour les objets séquences car seuls les types entiers sont autorisés.|  
|**Current_value**|**sql_variant non NULL**|La dernière valeur engagée. Autrement dit, la valeur retournée à partir de la dernière exécution de la dernière valeur d’exécution ou de la fonction NEXT VALUE FOR la **sp_sequence_get_range** procédure. Retourne la valeur START WITH si la séquence n'a jamais été utilisée.|  
|**is_exhausted**|**bit non NULL**|0 indique que davantage de valeurs peuvent être générées à partir de la séquence. 1 indique que l'objet séquence a atteint le paramètre MAXVALUE et que la séquence n'est pas définie sur CYCLE. La fonction NEXT VALUE FOR retourne une erreur jusqu'à ce que la séquence soit redémarrée à l'aide d'ALTER SEQUENCE.|  
  
## <a name="permissions"></a>Autorisations  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures, la visibilité des métadonnées dans les affichages catalogue est limitée aux éléments sécurisables qu'un utilisateur détient ou pour lesquels des autorisations lui ont été accordées. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [sp_sequence_get_range &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
