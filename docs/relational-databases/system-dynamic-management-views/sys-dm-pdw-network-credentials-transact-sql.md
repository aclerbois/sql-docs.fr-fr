---
title: Sys.dm_pdw_network_credentials (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: d4fee3ad-6285-4ea5-8513-5e6eb617abb0
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e7b4534410eabf1186b115c07fef8a8d79960938
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwnetworkcredentials-transact-sql"></a>Sys.dm_pdw_network_credentials (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Retourne une liste de toutes les informations d’identification de réseau est stockée dans le [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] matériel pour tous les serveurs cibles. Les résultats sont répertoriés pour le nœud de contrôle et chaque nœud de calcul.  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|Id numérique unique associé au nœud.|  
|target_server_name|**nvarchar(32)**|Adresse IP du serveur cible qui [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] en utilisant les informations d’identification de nom d’utilisateur et mot de passe.|  
|username|**nvarchar(32)**|Nom d’utilisateur pour lequel le mot de passe est stocké.|  
|last_modified|**datetime**|Date et heure de la dernière opération qui a modifié les informations d’identification.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert VIEW SERVER STATE.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 La clé pour cette vue de gestion dynamique est *pdw_node_id* plus *nom_du_serveur_cible*.  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
