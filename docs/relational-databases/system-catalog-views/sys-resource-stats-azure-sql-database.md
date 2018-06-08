---
title: Sys.resource_stats (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 04/06/2018
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: 02379a1b-3622-4578-8c59-a1b8f1a17914
caps.latest.revision: 28
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: c2f8a0e0cebcf64bedac33861184e806f322d7d1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysresourcestats-azure-sql-database"></a>sys.resource_stats (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Retourne les données de stockage et d'utilisation de l'UC pour Azure SQL Database. Les données sont collectées et agrégées dans des intervalles de cinq minutes. Pour chaque base de données utilisateur, il existe une ligne pour chaque fenêtre de rapports toutes les cinq minutes dans laquelle a lieu une modification de la consommation des ressources. Les données retournées incluent l’utilisation processeur, la modification de taille de stockage ou base de données de modification de la référence. Bases de données inactives sans modification peut-être pas les lignes pour chaque intervalle de cinq minutes. Les données historiques sont conservées pendant environ 14 jours.  
  
 Le **sys.resource_stats** vue a des définitions différentes selon la version du serveur de base de données SQL Azure qui est associé à la base de données. Tenez compte de ces différences et des modifications requises par votre application lors de la mise à niveau vers une nouvelle version de serveur.  
  
 Le tableau suivant décrit les colonnes disponibles dans un serveur v12 :  
  
|Columns|Type de données| Description|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime**|Heure UTC indiquant le début de l’intervalle de création de rapports de cinq minutes.|  
|end_time|**datetime**|Heure UTC indiquant la fin de l’intervalle de création de rapports de cinq minutes.|  
|database_name|**varchar**|Nom de la base de données utilisateur.|  
|sku|**varchar**|Niveau de service de la base de données. Les valeurs possibles sont les suivantes :<br /><br /> Basic<br /><br /> Standard<br /><br /> Premium<br /><br />Usage général<br /><br />Critique pour l’entreprise|  
|storage_in_megabytes|**float**|Taille de stockage maximale en mégaoctets pour la période de temps, y compris les données de la base de données, les index, les procédures stockées et les métadonnées.|  
|avg_cpu_percent|**numeric**|Utilisation moyenne du calcul en pourcentage de la limite de la couche de service.|  
|avg_data_io_percent|**numeric**|Utilisation moyenne des E-S en pourcentage en fonction de la limite du niveau de service.|  
|avg_log_write_percent|**numeric**|Utilisation moyenne de la ressource d'écriture en pourcentage de la limite de la couche de service.|  
|max_worker_percent|**Decimal(5,2)**|Traitements simultanés maximum (demandes) en pourcentage de la limite de niveau de service de la base de données.<br /><br /> Nombre maximal est actuellement calculée pour l’intervalle de cinq minutes basée sur les échantillons de 15 secondes des nombres de travail simultanés.|  
|max_session_percent|**Decimal(5,2)**|Nombre maximal de sessions simultané en pourcentage de la limite de niveau de service de la base de données.<br /><br /> Nombre maximal est actuellement calculée pour l’intervalle de cinq minutes basée sur les échantillons de 15 secondes du nombre de sessions simultanées.|  
|dtu_limit|**int**|Base de données max DTU paramètre actuel de cette base de données pendant cet intervalle. |  
  
> [!TIP]  
>  Pour plus d’informations sur ces limites et les niveaux de service, consultez les rubriques [niveaux de Service](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/).  
    
## <a name="permissions"></a>Autorisations  
 Cette vue est disponible pour tous les rôles d’utilisateur disposant des autorisations pour se connecter à virtuel **master** base de données.  
  
## <a name="remarks"></a>Notes  
 Les données retournées par **sys.resource_stats** est exprimée en pourcentage de la valeur maximale autorisée de limites au niveau de service et niveau de performances qui vous sont en cours d’exécution.  
  
 Lorsqu’une base de données est un membre d’un pool élastique, présentées sous forme de valeurs de pourcentage, statistiques des ressources sont exprimées en pourcentage de la limite maximale pour les bases de données défini dans la configuration du pool élastique.  
  
 Pour obtenir une vue plus granulaire de ces données, utilisez **sys.dm_db_resource_stats** vue de gestion dynamique dans une base de données utilisateur. Cette vue capture des données toutes les 15-secondes et conserve les données d’historique pour 1 heure.  Pour plus d’informations, consultez [sys.dm_db_resource_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md).  

## <a name="examples"></a>Exemples  
 L'exemple suivant retourne toutes les bases de données dont la moyenne s'établit à au moins 80 % de l'utilisation du calcul au cours de la dernière semaine.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT database_name, AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.resource_stats   
WHERE start_time BETWEEN @s AND @e  
GROUP BY database_name  
HAVING AVG(avg_cpu_percent) >= 80  
```  
    
## <a name="see-also"></a>Voir aussi  
 [Niveaux de service](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)   
 [Limites et des fonctions de niveau de Service](https://azure.microsoft.com/documentation/articles/sql-database-performance-guidance/)  
  
  
