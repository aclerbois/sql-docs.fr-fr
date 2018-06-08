---
title: Interrogation des données dans une table temporelle avec système par version | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2d358c2e-ebd8-4eb3-9bff-cfa598a39125
caps.latest.revision: 7
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: cfdb035f176c2fcdb9e71b5621b76e4ecb72c2b4
ms.sourcegitcommit: b3bb41424249de198f22d9c6d40df4996f083aa6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="querying-data-in-a-system-versioned-temporal-table"></a>Interrogation des données dans une table temporelle avec système par version
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Lorsque vous souhaitez obtenir l’état le plus récent (réel) des données d’une table temporelle, l’interrogation est exactement la même que pour une table non temporelle. Si les colonnes PERIOD ne sont pas masquées, leurs valeurs apparaissent dans une requête SELECT \* . Si vous avez spécifié les colonnes **PERIOD** comme étant masquées, leurs valeurs n’apparaissent pas dans une requête SELECT \* . Lorsque les colonnes **PERIOD** sont masquées, référencez spécifiquement les colonnes **PERIOD** dans la clause SELECT pour retourner les valeurs de ces colonnes.  
  
 Pour exécuter une analyse temporelle, utilisez la nouvelle clause **FOR SYSTEM_TIME** avec quatre sous-clauses temporelles spécifiques pour interroger les données des tables actuelles et d’historique. Pour plus d’informations sur ces clauses, consultez [Tables temporelles](../../relational-databases/tables/temporal-tables.md) et [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
-   AS OF <date_time>  
  
-   FROM <start_date_time> TO <end_date_time>  
  
-   BETWEEN <start_date_time> AND <end_date_time>  
  
-   CONTAINED IN (<start_date_time> , <end_date_time>)  
  
-   ALL  
  
 La clause**FOR SYSTEM_TIME** peut être spécifiée de façon indépendante pour chaque table dans une requête. Elle peut être utilisée à l'intérieur d’expressions de table communes, de fonctions table incluses et de procédures stockées.  
  
## <a name="query-for-a-specific-time-using-the-as-of-sub-clause"></a>Requête d’un point précis dans le temps à l'aide de la sous-clause AS OF  
 Utilisez la sous-clause **AS OF** quand vous devez reconstruire l’état des données tel qu’il était à un point précis dans le temps. Vous pouvez reconstruire les données avec la précision de type datetime2 qui avait été spécifiée dans les définitions de colonne **PERIOD** .    
La sous-clause **AS OF** peut être utilisée avec des constantes littérales ou des variables, ce qui vous permet de spécifier de manière dynamique la condition de temps. Les valeurs fournies sont interprétées en heure UTC.  
  
 Ce premier exemple retourne l'état de la table dbo.Department à partir (AS OF) d’une date spécifique dans le passé.  
  
```  
/*State of entire table AS OF specific date in the past*/   
SELECT [DeptID], [DeptName], [SysStartTime],[SysEndTime]   
FROM [dbo].[Department]   
FOR SYSTEM_TIME AS OF '2015-09-01 T10:00:00.7230011' ;  
  
```  
  
 Ce second exemple compare les valeurs entre deux points dans le temps pour un sous-ensemble de lignes.  
  
```  
DECLARE @ADayAgo datetime2   
SET @ADayAgo = DATEADD (day, -1, sysutcdatetime())   
/*Comparison between two points in time for subset of rows*/   
SELECT D_1_Ago.[DeptID], D.[DeptID],   
D_1_Ago.[DeptName], D.[DeptName],   
D_1_Ago.[SysStartTime], D.[SysStartTime],   
D_1_Ago.[SysEndTime], D.[SysEndTime]   
FROM [dbo].[Department] FOR SYSTEM_TIME AS OF @ADayAgo AS D_1_Ago   
JOIN [Department] AS D ON  D_1_Ago.[DeptID] = [D].[DeptID]    
AND D_1_Ago.[DeptID] BETWEEN 1 and 5 ;  
```  
  
### <a name="using-views-with-as-of-sub-clause-in-temporal-queries"></a>Utilisation de vues avec la sous-clause AS OF dans des requêtes temporelles  
 Les vues sont très utiles dans les scénarios nécessitant une analyse complexe à un point précis dans le temps.   
Un exemple courant est la création aujourd’hui d’un rapport d'entreprise s’appuyant sur les valeurs du mois précédent.   
En règle générale, les clients utilisent un modèle de base de données normalisé qui implique plusieurs tables avec des relations de clés étrangères. Il peut être très difficile de connaître l’état des données de ce modèle normalisé à un point précis dans le temps, car toutes les tables changent de manière indépendante, à leur propre rythme.   
Dans ce cas, la meilleure solution consiste à créer une vue et à appliquer la sous-clause **AS OF** à toute la vue. Cette approche vous permet de dissocier la modélisation de la couche d’accès aux données de l’analyse à un point précis dans le temps, car SQL Server appliquera la clause **AS OF** de manière transparente à toutes les tables temporelles impliquées dans la définition de la vue. En outre, vous pouvez combiner des tables temporelles et non temporelles dans la même vue, et **AS OF** s’appliquera uniquement aux tables temporelles. Si la vue ne référence pas au moins une table temporelle, l’application de clauses de requêtes temporelles échoue et affiche une erreur.  
  
```  
/* Create view that joins three temporal tables: Department, CompanyLocation, LocationDepartments */   
CREATE VIEW [dbo].[vw_GetOrgChart]   
AS   
SELECT   
     [CompanyLocation].LocID  
   , [CompanyLocation].LocName  
   , [CompanyLocation].City  
   , [Department].DeptID  
   , [Department].DeptName    
FROM [dbo].[CompanyLocation]   
LEFT JOIN [dbo].[LocationDepartments]    
   ON [CompanyLocation].LocID = LocationDepartments.LocID   
LEFT JOIN [dbo].[Department]    
   ON LocationDepartments.DeptID = [Department].DeptID ;  
GO   
/* Querying view AS OF */   
SELECT * FROM [vw_GetOrgChart]   
FOR SYSTEM_TIME AS OF '2015-09-01 T10:00:00.7230011' ;  
  
```  
  
## <a name="query-for-changes-to-specific-rows-over-time"></a>Rechercher des modifications sur des lignes spécifiques dans le temps  
 Les sous-clauses temporelles **FROM...TO**, **BETWEEN...AND** et **CONTAINED IN** sont utiles quand vous souhaitez effectuer un audit des données, autrement dit lorsque vous devez obtenir l’historique de toutes les modifications appliquées à une ligne spécifique dans la table actuelle.   
Les deux premières sous-clauses renvoient les versions de ligne qui se chevauchent sur une période donnée (c'est-à-dire celles qui ont démarré avant une certaine période et qui se sont terminées après celle-ci), tandis que CONTAINED IN retourne uniquement celles qui existaient dans des plages précises de la période.  
  
> [!IMPORTANT]  
>  Si vous recherchez uniquement les versions de ligne non actuelles, nous vous recommandons d’utiliser la clause **CONTAINED IN** , car elle s’applique uniquement à la table d’historique et génère les meilleurs résultats. Utilisez **ALL** lorsque vous devez interroger des données historiques et actuelles sans aucune restriction.  
  
```  
/* Query using BETWEEN...AND sub-clause*/  
SELECT   
     [DeptID]  
   , [DeptName]  
   , [SysStartTime]  
   , [SysEndTime]  
   , IIF (YEAR(SysEndTime) = 9999, 1, 0) AS IsActual   
FROM [dbo].[Department]   
FOR SYSTEM_TIME BETWEEN  '2015-01-01' AND '2015-12-31'   
WHERE DeptId = 1   
ORDER BY SysStartTime DESC;   
  
/*  Query using CONTAINED IN sub-clause */  
SELECT [DeptID], [DeptName], [SysStartTime],[SysEndTime]   
FROM [dbo].[Department]   
FOR SYSTEM_TIME CONTAINED IN ('2015-04-01', '2015-09-25')   
WHERE DeptId = 1   
ORDER BY SysStartTime DESC ;  
  
/*  Query using ALL sub-clause */   
SELECT    
     [DeptID]   
   , [DeptName]   
   , [SysStartTime]   
   , [SysEndTime]   
   , IIF (YEAR(SysEndTime) = 9999, 1, 0) AS IsActual    
FROM [dbo].[Department] FOR SYSTEM_TIME ALL   
ORDER BY [DeptID], [SysStartTime] Desc  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Création d’une table temporelle avec contrôle de version du système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)   
 [Modification des données dans une table temporelle avec système par version](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)   
 [Modification du schéma d’une table temporelle à version contrôlée par le système](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)   
 [Arrêt du contrôle de version par le système sur une table temporelle à version contrôlée par le système](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)  
  
  
