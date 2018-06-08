---
title: MSmerge_conflicts_info (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d5990e8dc08545597e18832e6f6b44bfbfce6514
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergeconflictsinfo-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_conflicts_info** table effectue le suivi des conflits qui se produisent lors de la synchronisation d’un abonnement à une publication de fusion. Les données de la ligne perdante de conflits sont stockées dans le [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) table pour l’article dans lequel le conflit s’est produit. Cette table est stockée dans la base de données de publication du serveur de publication et dans la base de données d'abonnement de l'Abonné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Surnom de la table publiée.|  
|**ROWGUID**|**uniqueidentifier**|Identificateur de la ligne de conflit|  
|**origin_datasource**|**nvarchar(255)**|Nom de la base de données d'où provient la modification conflictuelle|  
|**conflict_type**|**int**|Type de conflit qui s'est produit, qui peut être l'un des suivants :<br /><br /> **1** = conflit mise à jour : le conflit est détecté au niveau de la ligne.<br /><br /> **2** = conflit de mise à jour de colonne : le conflit est détecté au niveau de la colonne.<br /><br /> **3** = mise à jour supprimer Wins conflit : la suppression gagne le conflit.<br /><br /> **4** = conflit de suppression Wins mise à jour : le rowguid supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **5** = télécharger Insert a échoué : l’insertion provenant de l’abonné n’a pas pu être appliquée sur le serveur de publication.<br /><br /> **6** = Échec du téléchargement du insérer : l’insertion provenant du serveur de publication n’a pas pu être appliquée sur l’abonné.<br /><br /> **7** = télécharger supprimer a échoué : la suppression à l’abonné n’a pas pu être téléchargée sur le serveur de publication.<br /><br /> **8** = Échec du téléchargement du supprimer : la suppression appliquée au serveur de publication n’a pas pu être téléchargée à l’abonné.<br /><br /> **9** = Échec de télécharger la mise à jour : la mise à jour à l’abonné n’a pas pu être appliquée sur le serveur de publication.<br /><br /> **10** = Échec du téléchargement du jour : la mise à jour au serveur de publication n’a pas pu être appliquée à l’abonné.<br /><br /> **11** = résolution<br /><br /> **12** = logique mise à jour Wins suppression de l’enregistrement : l’enregistrement logique supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **13** = logique enregistrement conflit Insérer Mettre à jour : insertion dans un enregistrement logique est en conflit avec une mise à jour.<br /><br /> **14** = logique enregistrement supprimer Wins mise à jour en conflit : l’enregistrement logique mis à jour qui perd le conflit est enregistré dans cette table.|  
|**reason_code**|**int**|Le code d’erreur qui peut dépendre du contexte. Dans le cas de conflits de mise à jour de la mise à jour et suppression de la mise à jour, la valeur utilisée pour cette colonne est le même que le **conflict_type**. Par contre, dans les conflits d'échec de modification, le code de la raison correspond à l'erreur qui a empêché l'Agent de fusion d'appliquer la modification. Par exemple, si l’Agent de fusion ne peut pas appliquer une insertion sur l’abonné en raison d’une violation de clé primaire, il se connecte un **conflict_type** de 6 (« téléchargement insert a échoué ») et un **reason_code** de valeur 2627, qui est le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message d’erreur interne pour une violation de clé primaire : « Violation de contrainte %ls ' %. * %.*ls. Impossible d’insérer une clé dupliquée dans l’objet ' %. \*%.*ls. »|  
|**reason_text**|**nvarchar(720)**|La description d’erreur qui peut dépendre du contexte.|  
|**pubid**|**uniqueidentifier**|Identificateur de la publication.|  
|**MSrepl_create_time**|**datetime**|Heure à laquelle le conflit s'est produit.|  
|**origin_datasource_id**|**uniqueidentifier**|Identificateur de la base de données d'où provient la modification à l'origine du conflit|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
