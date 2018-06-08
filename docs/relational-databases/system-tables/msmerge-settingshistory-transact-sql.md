---
title: MSmerge_settingshistory (Transact-SQL) | Documents Microsoft
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
applies_to:
- SQL Server
f1_keywords:
- MSmerge_settingshistory
- MSmerge_settingshistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_settingshistory system table
ms.assetid: 0bdf2d5f-5502-44cd-aa9d-2d5006ad20ce
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2bb8007d97619b3cbe7302fbf00bcfd98584539e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergesettingshistory-transact-sql"></a>MSmerge_settingshistory (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_settingshistory** table est utilisée pour conserver un historique des modifications apportées aux propriétés de l’article et la publication pour la réplication de fusion, avec une ligne pour chaque modification apportée à une topologie de réplication de fusion. Cette table contient également des informations sur la date et l'heure à laquelle les paramètres de propriété initiaux ont été définis. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**EventTime**|**datetime**|Date et heure auxquels l'événement s'est produit.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique d'une publication donnée.|  
|**artid**|**uniqueidentifier**|Numéro d'identification unique de l'article donné.|  
|**type d’événement**|**tinyint**|Spécifie le type d'événement qui est enregistré, à savoir :<br /><br /> **1** : paramètre de propriété de niveau de publication initial.<br /><br /> **2** -modifier dans une propriété de publication.<br /><br /> **101** -paramètre de propriété de l’article initial.<br /><br /> **102** -changement de la propriété de l’article.|  
|**propertyname**|**sysname**|Nom de la propriété définie ou modifiée.|  
|**previousvalue**|**sysname**|Valeur précédente de la propriété si une propriété a été modifiée.|  
|**nouvelle valeur**|**sysname**|Valeur à laquelle la propriété a été modifiée ou créée.|  
|**eventText**|**nvarchar(2000)**|Chaîne de caractères décrivant l'événement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
