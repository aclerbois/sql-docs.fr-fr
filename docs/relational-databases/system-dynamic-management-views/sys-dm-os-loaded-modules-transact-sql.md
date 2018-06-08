---
title: Sys.dm_os_loaded_modules (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_loaded_modules
- dm_os_loaded_modules
- sys.dm_os_loaded_modules_TSQL
- dm_os_loaded_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_loaded_modules dynamic management view
ms.assetid: 56c7743a-b568-4943-bd3b-73c57d9d641c
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1888f39f6024a0b299834217c2f8b69052761b65
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosloadedmodules-transact-sql"></a>sys.dm_os_loaded_modules (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque module chargé dans l'espace d'adressage du serveur.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_loaded_modules**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**base_address**|**varbinary(8)**|Adresse du module dans le processus.|  
|**file_version**|**varchar(23)**|Version du fichier. Apparaît sous le format suivant :<br /><br /> x.x:x.x|  
|**version_produit**|**varchar(23)**|Version du produit. Apparaît sous le format suivant :<br /><br /> x.x:x.x|  
|**Débogage**|**bit**|1 = le module est une version de débogage du module chargé.|  
|**corrigé**|**bit**|1 = le module a été corrigé.|  
|**version préliminaire**|**bit**|1 = le module est une version préliminaire du module chargé.|  
|**private_build**|**bit**|1 = le module est une version privée du module chargé.|  
|**special_build**|**bit**|1 = le module est une version spéciale du module chargé.|  
|**Langage**|**int**|Langue des informations de version du module.|  
|**Société**|**nvarchar (256)**|Nom de la société qui a créé le module.|  
|**description**|**nvarchar (256)**|Description du module.|  
|**nom**|**nvarchar(255)**|Nom du module. Inclut le chemin d'accès complet du module.|  
|**pdw_node_id**|**int**|**S’applique à** : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
