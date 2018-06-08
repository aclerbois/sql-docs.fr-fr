---
title: Arrêt du contrôle de version du système sur une table temporelle avec contrôle de version par le système | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: dddd707e-bfb1-44ff-937b-a84c5e5d1a94
caps.latest.revision: 10
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4e29857b52e2845599ea35dda7b4bb39b6ec1bd2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stopping-system-versioning-on-a-system-versioned-temporal-table"></a>Arrêt du contrôle de version par le système sur une table temporelle à version contrôlée par le système
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Vous pouvez interrompre le contrôle des versions sur votre table temporelle temporairement ou définitivement.   
Pour ce faire, vous pouvez affecter à la clause **SYSTEM_VERSIONING** la valeur **OFF**.  
  
## <a name="setting-systemversioning--off"></a>Configuration de SYSTEM_VERSIONING = OFF  
 Arrêtez le contrôle de version par le système si vous souhaitez effectuer des opérations de maintenance spécifiques sur la table temporelle ou si vous n’avez plus besoin une table avec version. Suite à cette opération, vous obtiendrez deux tables indépendantes :  
  
-   La table actuelle avec définition de la période  
  
-   Une table historique sous la forme d’une table normale  
  
### <a name="important-remarks"></a>Remarques importantes  
  
-   Aucune perte de données ne se produit quand vous définissez  **SYSTEM_VERSIONING = OFF** ou supprimez la période **SYSTEM_TIME** .  
  
-   Quand vous définissez **SYSTEM_VERSIONING = OFF** sans supprimer la période **SYSTEM_TIME** , le système continue à mettre à jour les colonnes de période pour chaque opération d’insertion et de mise à jour. Les suppressions effectuées sur la table actuelle sont définitives.  
  
-   Supprimez la période **SYSTEM_TIME** pour supprimer les colonnes de période définitivement.  
  
-   Quand vous définissez **SYSTEM_VERSIONING = OFF**, tous les utilisateurs disposant des autorisations suffisantes peuvent modifier le schéma et le contenu de la table historique, et même supprimer définitivement la table historique.  
  
### <a name="permanently-remove-systemversioning"></a>Supprimer définitivement SYSTEM_VERSIONING  
 Cet exemple supprime définitivement SYSTEM_VERSIONING et complètement les colonnes de période. La suppression des colonnes de période est facultative.  
  
```  
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);   
/*Optionally, DROP PERIOD if you want to revert temporal table to a non-temporal*/   
ALTER TABLE dbo.Department   
DROP PERIOD FOR SYSTEM_TIME;  
  
```  
  
### <a name="temporarily-remove-systemversioning"></a>Supprimer temporairement SYSTEM_VERSIONING  
 Voici la liste des opérations qui nécessitent la définition du contrôle de version par le système avec la valeur **OFF**:  
  
-   Suppression des données superflues de l’historique (**DELETE** ou **TRUNCATE**)  
  
-   Suppression des données de la table actuelle sans contrôle de version (**DELETE**, **TRUNCATE**)  
  
-   Partition **SWITCH OUT** à partir de la table actuelle  
  
-   Partition **SWITCH IN** dans la table historique  
  
 Cet exemple arrête temporairement SYSTEM_VERSIONING pour que vous puissiez effectuer des opérations de maintenance spécifiques. Si vous arrêtez le contrôle de version temporairement à des fins de maintenance de tables, nous vous recommandons vivement d’effectuer cette opération dans une transaction pour préserver la cohérence des données.  
  
```  
BEGIN TRAN   
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);   
TRUNCATE TABLE [History].[DepartmentHistory]   
WITH (PARTITIONS (1,2))   
ALTER TABLE dbo.Department SET    
(   
SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)   
);   
COMMIT ;  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [Création d’une table temporelle avec contrôle de version du système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)   
 [Modification des données dans une table temporelle avec système par version](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)   
 [Interrogation des données dans une table temporelle avec système par version](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)   
 [Modification du schéma d’une table temporelle à version contrôlée par le système](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)  
  
  
