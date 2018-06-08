---
title: Sys.dm_db_resource_stats (de base de données SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 04/06/2018
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: a91988c36604ce38c7022e6bc111cc1941e43a03
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbresourcestats-azure-sql-database"></a>sys.dm_db_resource_stats (base de données Azure SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Retourne la consommation d’UC, d’e/s et de mémoire pour un [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] base de données. Il existe une ligne pour chaque période de 15 secondes, même s'il n'y a aucune activité dans la base de données. Les données historiques sont conservées pendant une heure.  
  
|Columns|Type de données| Description|  
|-------------|---------------|-----------------|  
|end_time|**datetime**|Heure UTC indiquant la fin de l'intervalle de rapports actuel.|  
|avg_cpu_percent|**décimale (5,2)**|Utilisation moyenne du calcul en pourcentage de la limite de la couche de service.|  
|avg_data_io_percent|**décimale (5,2)**|Moyenne des données d’utilisation des e/s de pourcentage de la limite de la couche de service.|  
|avg_log_write_percent|**décimale (5,2)**|Utilisation moyenne de la ressource d'écriture en pourcentage de la limite de la couche de service.|  
|avg_memory_usage_percent|**décimale (5,2)**|Utilisation moyenne de la mémoire en pourcentage de la limite de la couche de service.<br /><br /> Cela inclut la mémoire utilisée pour le stockage d’objets de l’OLTP en mémoire.|  
|xtp_storage_percent|**décimale (5,2)**|Utilisation du stockage pour l’OLTP en mémoire en pourcentage de la limite de la couche de service (à la fin de l’intervalle de création de rapports). Cela inclut la mémoire utilisée pour le stockage des objets OLTP en mémoire suivants : tables optimisées en mémoire, les index et les variables de table. Il inclut également la mémoire utilisée pour le traitement des opérations ALTER TABLE.<br /><br /> Retourne 0 si l’OLTP en mémoire n’est pas utilisé dans la base de données.|  
|max_worker_percent|**décimale (5,2)**|Traitements simultanés maximum (demandes) en pourcentage de la limite de niveau de service de la base de données.|  
|max_session_percent|**décimale (5,2)**|Nombre maximal de sessions simultané en pourcentage de la limite de niveau de service de la base de données.|  
|dtu_limit|**int**|Base de données max DTU paramètre actuel de cette base de données pendant cet intervalle. |
|||
  
> [!TIP]  
>  Pour plus d’informations sur ces limites et les niveaux de service, consultez les rubriques [niveaux de Service](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/) et [les limites et les fonctionnalités de niveau de Service](https://azure.microsoft.com/documentation/articles/sql-database-performance-guidance/).  
  
## <a name="permissions"></a>Autorisations  
 Cette vue nécessite l'autorisation VIEW DATABASE STATE.  
  
## <a name="remarks"></a>Notes  
 Les données retournées par **sys.dm_db_resource_stats** est exprimée en pourcentage de la valeur maximale autorisée de limites au niveau de service et niveau de performances qui vous sont en cours d’exécution.
 
 Si la base de données a basculé vers un autre serveur lors des 60 dernières minutes, l’affichage retourne uniquement les données pour le temps durant lequel elle est la base de données primaire, depuis ce basculement.  
  
 Pour obtenir un affichage moins granulaire de ces données, utilisez **sys.resource_stats** vue de catalogue le **master** base de données. Cette vue capture des données toutes les 5 minutes et conserve 14 jours d'historique des données.  Pour plus d’informations, consultez [sys.resource_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md).  
  
 Lorsqu’une base de données est un membre d’un pool élastique, présentées sous forme de valeurs de pourcentage, statistiques des ressources sont exprimées en pourcentage de la limite maximale pour les bases de données défini dans la configuration du pool élastique.  
  
## <a name="example"></a>Exemple  
  
L'exemple suivant retourne les données d'utilisation de ressources ordonnées selon l’heure la plus récente pour la base de données actuellement connectée.  
  
```  
SELECT * FROM sys.dm_db_resource_stats ORDER BY end_time DESC;  
  
```  
  
 L'exemple suivant identifie la consommation moyenne en UDBD, sous forme de pourcentage de la limite maximale autorisée d’UDBD du niveau de performance de la base de données utilisateur sur la dernière heure. Envisagez d'augmenter le niveau de performance si ces pourcentages sont souvent proches de 100 %.  
  
```  
SELECT end_time,   
  (SELECT Max(v)    
   FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS    
   value(v)) AS [avg_DTU_percent]   
FROM sys.dm_db_resource_stats;  
  
```  
  
 L'exemple suivant retourne les valeurs moyennes et maximales de pourcentage de CPU, d’E/S de données et de journaux et de consommation de mémoire sur la dernière heure.  
  
```  
SELECT    
    AVG(avg_cpu_percent) AS 'Average CPU Utilization In Percent',   
    MAX(avg_cpu_percent) AS 'Maximum CPU Utilization In Percent',   
    AVG(avg_data_io_percent) AS 'Average Data IO In Percent',   
    MAX(avg_data_io_percent) AS 'Maximum Data IO In Percent',   
    AVG(avg_log_write_percent) AS 'Average Log Write Utilization In Percent',   
    MAX(avg_log_write_percent) AS 'Maximum Log Write Utilization In Percent',   
    AVG(avg_memory_usage_percent) AS 'Average Memory Usage In Percent',   
    MAX(avg_memory_usage_percent) AS 'Maximum Memory Usage In Percent'   
FROM sys.dm_db_resource_stats;  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.resource_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [Niveaux de service](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)   
 [Limites et des fonctions de niveau de Service](https://azure.microsoft.com/documentation/articles/sql-database-performance-guidance/)  
  
  
