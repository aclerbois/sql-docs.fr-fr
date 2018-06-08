---
title: sysmergesubsetfilters (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmergesubsetfilters
- sysmergesubsetfilters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubsetfilters system table
ms.assetid: f91d1c6c-3132-47f6-926c-88f56848cafe
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c26608c25b2bd5d9778f076a4f5ee1054730b836
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergesubsetfilters-transact-sql"></a>sysmergesubsetfilters (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient des informations sur les filtres de jointure des articles partitionnés. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom de filtre**|**sysname**|Nom du filtre utilisé pour créer l'article.|  
|**join_filterid**|**int**|ID de l'objet représentant le filtre de jointure.|  
|**pubid**|**uniqueidentifier**|ID de la publication.|  
|**artid**|**uniqueidentifier**|L’ID de l’article.|  
|**art_nickname**|**int**|Surnom de l'article.|  
|**join_articlename**|**sysname**|Nom de la table avec laquelle il faut effectuer une jointure pour déterminer si la ligne lui appartient.|  
|**join_nickname**|**int**|Surnom de la table avec laquelle il faut effectuer une jointure pour déterminer si la ligne lui appartient.|  
|**join_unique_key**|**int**|Indique une jointure sur une clé unique de **join_tablename**:<br /><br /> 0 = N'est pas une clé unique<br /><br /> 1 = Est une clé unique|  
|**expand_proc**|**sysname**|Nom de la procédure stockée utilisée par l'Agent de fusion pour identifier les lignes à envoyer à un Abonné ou à supprimer de celui-ci.|  
|**join_filterclause**|**nvarchar(1000)**|Clause de filtre utilisée pour la jointure.|  
|**filter_type**|**tinyint**|Spécifie le type de filtre parmi les types suivants :<br /><br /> 1 = Filtre de jointure.<br /><br /> 2 = Lien d'enregistrement logique.<br /><br /> 3 = Filtre de jointure et lien d'enregistrement logique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
