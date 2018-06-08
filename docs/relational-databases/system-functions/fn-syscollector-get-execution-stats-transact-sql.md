---
title: fn_syscollector_get_execution_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_stats
- fn_syscollector_get_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_stats function
ms.assetid: 793ad72c-a992-4a8d-8584-bcb6b3b476f1
caps.latest.revision: 18
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 1a80d59325234108f16a75c081f94d4102c44154
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="fnsyscollectorgetexecutionstats-transact-sql"></a>fn_syscollector_get_execution_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne des statistiques détaillées sur le jeu d'éléments de collecte ou le package, y compris le nombre de lignes d'erreur qui sont enregistrées par une tâche de flux de données de package. Une tâche de flux de données est un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui traite les données. Ces données étant au format relationnel, elles comprennent des groupes de données d'entrée et de sortie composés de lignes.  
  
 Les statistiques sont calculées à partir des entrées de la vue syscollector_execution_stats.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_syscollector_get_execution_stats ( log_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *log_id*  
 Identificateur unique local pour le journal des exécutions. *log_id* est **int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|avg_row_count_in|**int**|Nombre moyen de lignes qui ont entré les tâches de flux de données du package.<br /><br /> Remarque : Une tâche de flux de données est un [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] composant qui traite les données. Ces données étant au format relationnel, elles comprennent un groupe de données d'entrée composé de lignes. Il s'agit du nombre de lignes qui ont entré la tâche. Une fois les données transformées, elles sont générées en sortie sous forme d'un jeu de résultats composé de lignes. La tâche de flux de données transforme les données et génère en sortie un jeu de résultats composé de lignes. Cette sortie est le nombre de lignes qui ont quitté la tâche.|  
|min_row_count_in|**int**|Nombre minimal de lignes qui ont entré les données de flux de tâches du package.|  
|max_row_count_in|**int**|Nombre maximal de lignes qui ont entré les données de flux de tâches du package.|  
|avg_row_count_out|**int**|Nombre moyen de lignes qui ont quitté les tâches de flux de données du package.|  
|min_row_count_out|**int**|Nombre minimal de lignes qui ont quitté les données de flux de tâches du package.|  
|max_row_count_out|**int**|Nombre maximal de lignes qui ont quitté les tâches de flux de données du package.|  
|avg_duration|**int**|Durée moyenne, en millisecondes, passée dans le composant de flux de données du package.|  
|min_duration|**int**|Durée minimale, en millisecondes, passée dans le composant de flux de données du package.|  
|max_duration|**int**|Durée maximale, en millisecondes, passée dans le composant de flux de données du package.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation SELECT pour **dc_operator**.  
  
## <a name="see-also"></a>Voir aussi  
 [syscollector_execution_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-execution-stats-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
