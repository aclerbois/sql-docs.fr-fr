---
title: Sys.database_event_sessions (de base de données SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 02c2cd71-d35e-4d4c-b844-92b240f768f4
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: f7002c353a3f47293c7c07567bbae606219c8228
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabaseeventsessions-azure-sql-database"></a>Sys.database_event_sessions (de base de données SQL Azure)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Répertorie toutes les définitions de session d’événements qui existent dans la base de données en cours dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
> [!NOTE]  
>  L’affichage catalogue similaire nommée `sys.server_event_sessions` s’applique uniquement aux [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
||  
|-|  
|**S’applique aux**: [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]et pour les versions ultérieures.|  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID unique de la session d'événements. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom défini par l'utilisateur pour identifier la session d'événements. nom est unique. N'accepte pas la valeur NULL.|  
|event_retention_mode|**nchar(1)**|Détermine comment est gérée la perte d'événements. La valeur par défaut est S. Elle n'accepte pas la valeur Null. Prend l'une des valeurs suivantes :<br /><br /> S. Mappe avec event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. Mappe avec event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> N. Mappe avec event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Décrit comment est gérée la perte d'événements. La valeur par défaut est ALLOW_SINGLE_EVENT_LOSS. N'accepte pas la valeur NULL. Prend l'une des valeurs suivantes :<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Il est possible de perdre des événements de la session. Les événements uniques ne sont abandonnés uniquement lorsque toutes les mémoires tampons d'événements sont saturées. La perte d'événements uniques lorsque les mémoires tampons d'événements sont saturées permet d'obtenir des caractéristiques de performance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptables, tout en réduisant la perte de données du flux d'événements traité.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. Il est possible de perdre des mémoires tampons d'événements saturées de la session. Le nombre d'événements perdus dépend de la taille de la mémoire allouée à la session, du partitionnement de la mémoire et de la taille des événements dans la mémoire tampon. Cette option atténue l'impact sur les performances du serveur lorsque les mémoires tampons d'événements sont rapidement remplies. Toutefois, il est possible de perdre un grand nombre d'événements de la session.<br /><br /> NO_EVENT_LOSS. Aucune perte d'événements n'est autorisée. Cette option garantit que tous les événements déclenchés sont conservés. Elle force toutes les tâches qui déclenchent des événements à attendre que de l'espace se libère dans une mémoire tampon d'événements. Cette option peut entraîner des problèmes de performance détectables pendant que la session d'événements est active.|  
|max_dispatch_latency|**int**|Durée (en millisecondes) pendant laquelle les événements sont mis en mémoire tampon avant d'être distribués aux cibles de la session. Les valeurs possibles vont de 1 à 2147483648 et -1. Une valeur de -1 indique que la latence de répartition est infinie. Autorise la valeur NULL.|  
|max_memory|**int**|Quantité de mémoire allouée à la session pour la mise en mémoire tampon d'événements. La valeur par défaut est 4 Mo. Autorise la valeur NULL.|  
|max_event_size|**int**|Quantité de mémoire réservée aux événements pour lesquels les mémoires tampons de session d'événements ne suffisent pas. Si max_event_size dépasse la taille de mémoire tampon calculée, deux mémoires tampons supplémentaires de max_event_size sont allouées à la session d'événements. Autorise la valeur NULL.|  
|memory_partition_mode|**nchar(1)**|Emplacement dans la mémoire où les mémoires tampons d'événements sont créées. Le mode de partition par défaut est G. N'accepte pas la valeur Null. MEMORY_PARTITION_MODE fait partie de :<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|La valeur par défaut est NONE. N'accepte pas la valeur NULL. Prend l'une des valeurs suivantes :<br /><br /> NONE. Un jeu unique de mémoires tampons sont créés dans une instance de SQL Server.<br /><br /> PER_CPU. Un jeu de mémoires tampons est créé pour chaque UC.<br /><br /> PER_NODE. Un jeu de mémoires tampons est créé pour chaque nœud NUMA (Non-Uniform Memory Access).|  
|track_causality|**bit**|Activer ou désactiver le suivi de causalité. Si cette option a la valeur 1 (ON), le suivi est activé et les événements associés de différentes connexions au serveur peuvent être corrélés. Le paramètre par défaut est 0 (OFF). N'accepte pas la valeur NULL.|  
|startup_state|**bit**|Cette valeur détermine si la session est lancée automatiquement au démarrage du serveur. La valeur par défaut est 0. N'accepte pas la valeur NULL. Les valeurs suivantes sont possibles :<br /><br /> 0 (OFF). La session ne se lance pas au démarrage du serveur.<br /><br /> 1 (ON). La session d'événements se lance au démarrage du serveur.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
  
