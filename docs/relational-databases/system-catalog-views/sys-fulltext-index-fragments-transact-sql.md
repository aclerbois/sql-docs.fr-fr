---
title: Sys.fulltext_index_fragments (Transact-SQL) | Documents Microsoft
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
- fulltext_index_fragments
- sys.fulltext_index_fragments_TSQL
- fulltext_index_fragments_TSQL
- sys.fulltext_index_fragments
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], fragments
- full-text indexes [SQL Server], metadata
- troubleshooting [SQL Server], full-text search
- sys.fulltext_index_fragments catalog view
ms.assetid: a82e5018-5d88-45c0-9a47-c251e17a6cdb
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 18997da5a5eeb691245166be015b0678403b52ff
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfulltextindexfragments-transact-sql"></a>sys.fulltext_index_fragments (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Un index de recherche en texte intégral utilise des tables internes appelées *fragments d’index de recherche en texte intégral* pour stocker les données de l’index inversé. Cette vue permet d'interroger les métadonnées relatives à ces fragments. Cette vue contient une ligne pour chaque fragment d'index de recherche en texte intégral dans chaque table qui contient un index.  
 
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|table_id|**int**|ID d'objet de la table qui contient le fragment de l'index de recherche en texte intégral.|  
|fragment_object_id|**int**|ID d'objet de la table interne associée au fragment.|  
|fragment_id|**int**|ID logique du fragment d'index de recherche en texte intégral. Celui-ci est unique dans l'ensemble des fragments de cette table.|  
|TIMESTAMP|**timestamp**|Horodateur associé à la création de fragment. Les horodateurs de fragments plus récents sont plus grands que les horodateurs de fragments plus anciens.|  
|data_size|**int**|Taille logique du fragment en octets.|  
|row_count|**int**|Nombre de lignes individuelles dans le fragment.|  
|status|**int**|Statut du fragment, une des valeurs suivantes :<br /><br /> 0 = Récemment créé mais pas encore utilisé<br /><br /> 1 = Utilisé pour l'insertion pendant l'alimentation ou la fusion d'index de recherche en texte intégral<br /><br /> 4 = Fermé. Prêt à être interrogé<br /><br /> 6 = Utilisé pour l'entrée de fusion et prêt à être interrogé<br /><br /> 8 = Marqué pour la suppression. Ne sera pas utilisé pour interroger et fusionner la source.<br /><br /> État 4 ou 6 signifie que le fragment fait partie de l’index de recherche en texte intégral logique peut être interrogé ; Autrement dit, il est un *requêtable* fragment.|  
  
## <a name="remarks"></a>Notes  
 L'affichage catalogue sys.fulltext_index_fragments peut être utilisé pour interroger le nombre des fragments qui comprennent un index de recherche en texte intégral. Si les performances des requêtes de texte intégral sont lentes, vous pouvez utiliser sys.fulltext_index_fragments pour déterminer le nombre de fragments requêtables (statut = 4 ou 6) dans l'index de recherche en texte intégral, comme suit :  
  
```  
SELECT table_id, status FROM sys.fulltext_index_fragments  
   WHERE status=4 OR status=6;  
```  
  
 Si de nombreux fragments requêtables existent, Microsoft vous recommande de réorganiser le catalogue de texte intégral qui contient l'index de recherche en texte intégral pour fusionner les fragments. Pour réorganiser une utilisation du catalogue de texte intégral [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)*catalog_name* RÉORGANISER. Par exemple, pour réorganiser un catalogue de texte intégral nommé `ftCatalog` dans la base de données `AdventureWorks2012`, entrez :  
  
```  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog REORGANIZE;  
GO  
```  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
