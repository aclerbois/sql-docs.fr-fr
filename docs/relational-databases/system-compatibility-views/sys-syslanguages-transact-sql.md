---
title: Sys.syslanguages (Transact-SQL) | Documents Microsoft
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
- sys.syslanguages
- sys.syslanguages_TSQL
- syslanguages
- syslanguages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syslanguages system table
- sys.syslanguages compatibility view
ms.assetid: f216d1cd-997c-42f0-a737-abbdfcd88383
caps.latest.revision: 37
author: rothja
ms.author: jroth
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4c879ed679052a14d420211c5977d2152d5ccb8f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssyslanguages-transact-sql"></a>sys.syslanguages (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne pour chaque langue présente dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|langid|**smallint**|ID unique de la langue.|  
|dateformat|**nchar(3)**|Ordre des éléments de la date, par exemple JMA.|  
|datefirst|**tinyint**|Premier jour de la semaine : 1 pour lundi, 2 pour mardi, etc., jusqu'à 7 pour dimanche.|  
|Mise à niveau|**int**|Réservé pour le système.|  
|name|**sysname**|Nom officiel de la langue, par exemple « Français ».|  
|alias|**sysname**|Nom de la langue de remplacement, par exemple « French ».|  
|mois|**nvarchar(372)**|Liste des noms complets des mois, séparés par des virgules, dans l'ordre, de janvier à décembre. Chaque nom peut comporter jusqu'à 20 caractères.|  
|mois courts|**nvarchar(132)**|Liste des noms abrégés des mois, séparés par des virgules, dans l'ordre, de janvier à décembre. Chaque nom peut comporter jusqu'à 9 caractères.|  
|jours|**nvarchar(217)**|Liste des noms des jours, séparés par des virgules, dans l'ordre, de lundi à dimanche. Chaque nom peut comporter jusqu'à 30 caractères.|  
|lcid|**int**|Identificateur des paramètres régionaux [!INCLUDE[msCoName](../../includes/msconame-md.md)] de la langue.|  
|msglangid|**smallint**|ID du groupe de messages du [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
  
 Les langues suivantes sont contenues dans [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
|Nom en anglais|LCID Windows|ID du groupe de messages [!INCLUDE[ssDE](../../includes/ssde-md.md)]|  
|---------------------|------------------|-----------------------------------------|  
|Anglais|1033|1033|  
|Allemand|1031|1031|  
|Français|1036|1036|  
|Japonais|1041|1041|  
|Danish|1030|1030|  
|Espagnol|3082|3082|  
|Italien|1040|1040|  
|Néerlandais|1043|1043|  
|Norvégien|2068|2068|  
|Portugais|2070|2070|  
|Finlandais|1035|1035|  
|Suédois|1053|1053|  
|Czech|1029|1029|  
|Hongrois|1038|1038|  
|Polonais|1045|1045|  
|Roumain|1048|1048|  
|Croate|1050|1050|  
|Slovaque|1051|1051|  
|Slovene|1060|1060|  
|Greek|1032|1032|  
|Bulgare|1026|1026|  
|Russe|1049|1049|  
|Turc|1055|1055|  
|British English|2057|1033|  
|Estonien|1061|1061|  
|Letton|1062|1062|  
|Lituanien|1063|1063|  
|Portugais (Brésil)|1046|1046|  
|Chinois traditionnel|1028|1028|  
|Coréen|1042|1042|  
|Chinois simplifié|2052|2052|  
|Arabe|1025|1025|  
|Thaïlandais|1054|1054|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
