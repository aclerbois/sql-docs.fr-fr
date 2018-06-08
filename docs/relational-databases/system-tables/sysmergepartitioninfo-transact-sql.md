---
title: sysmergepartitioninfo (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2d0c9ed5edb8f143a0e3742a7c49d23d95266640
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Fournit des informations sur les partitions de chaque article. Contient une ligne pour chaque article de fusion défini dans la base de données locale. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|Numéro d'identification unique de l'article donné.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique de la publication, généré lors de l'ajout de la publication|  
|**partition_view_id**|**int**|ID de la vue de partition sur cette table. La vue montre un mappage de chaque ligne de l'article avec les différents ID de partition auxquels elle appartient.|  
|**repl_view_id**|**int**|À ajouter.|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|Instruction SQL utilisée dans un déclencheur de réplication de fusion pour extraire l'ID de partition de chaque ligne supprimée ou mise à jour en fonction de ses anciennes valeurs de colonne.|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|Instruction SQL utilisée dans un déclencheur de réplication de fusion pour extraire l'ID de partition de chaque ligne insérée ou mise à jour en fonction de ses nouvelles valeurs de colonne.|  
|**membership_eval_proc_name**|**sysname**|Le nom de la procédure qui évalue les ID de partition actuel de lignes dans **MSmerge_contents**.|  
|**column_list**|**nvarchar(4000)**|Liste séparée par des virgules des colonnes répliquées dans un article.|  
|**column_list_blob**|**nvarchar(4000)**|Liste séparée par des virgules des colonnes répliquées dans un article, y compris les colonnes BLOB (Binary Large Object).|  
|**expand_proc**|**sysname**|Nom de la procédure qui réévalue les ID de partition de toutes les lignes enfants d'une ligne parente nouvellement insérée et des lignes parentes supprimées ou soumises à une modification de partition.|  
|**logical_record_parent_nickname**|**int**|Surnom du parent de niveau supérieur d'un article donné dans un enregistrement logique.|  
|**logical_record_view**|**int**|Vue qui génère la colonne rowguid d'article de parent de niveau supérieur correspondant à chaque colonne rowguid enfant.|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|Semblable à **logical_record_view**, à ceci près qu’elle montre les lignes enfants de la table « supprimée » dans mettre à jour et supprimer les déclencheurs.|  
|**logical_record_level_conflict_detection**|**bit**|Indique si les conflits doivent être détectés au niveau des enregistrements logiques ou au niveau des lignes ou des colonnes.<br /><br /> **0** = niveau ligne ou colonne-détection des conflits.<br /><br /> **1** = logique détection de conflit d’enregistrement est utilisée, où une modification d’une ligne sur le serveur de publication et de modification dans une fonction de lignes la même logique enregistrement au niveau de l’abonné est gérée comme un conflit.<br /><br /> Lorsque cette valeur est **1**, résolution de conflit au niveau d’enregistrement logique uniquement peut être utilisée.|  
|**logical_record_level_conflict_resolution**|**bit**|Indique si les conflits doivent être résolus au niveau des enregistrements logiques ou au niveau des lignes ou des colonnes.<br /><br /> **0** = niveau ligne ou colonne-résolution est utilisée.<br /><br /> **1** = en cas de conflit, l’enregistrement logique entière du vainqueur du remplace l’intégralité enregistrement logique côté perdant.<br /><br /> La valeur **1** utilisables avec les deux détection au niveau des enregistrements logiques et la détection au niveau de la ligne ou de la colonne.|  
|**partition_options**|**tinyint**|Définit le mode de partitionnement des données de l'article, ce qui permet l'optimisation des performances lorsque toutes les lignes appartiennent à une seule partition ou à un seul abonnement. *partition_options* peut prendre l’une des valeurs suivantes.<br /><br /> **0** = le filtrage de l’article est statique ou ne produit pas un sous-ensemble unique de données pour chaque partition, c'est-à-dire une partition en « chevauchement ».<br /><br /> **1** = les partitions se chevauchent et les mises à jour DML effectuées sur l’abonné ne peut pas modifier la partition à laquelle appartient une ligne.<br /><br /> **2** = le filtrage de l’article génère sans chevauchement de partitions, mais plusieurs abonnés peuvent recevoir la même partition.<br /><br /> **3** = le filtrage de l’article génère des partitions sans chevauchement qui sont uniques pour chaque abonnement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
