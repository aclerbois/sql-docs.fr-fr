---
title: Traitement de requêtes intelligent dans les bases de données Microsoft SQL | Microsoft Docs
description: Fonctionnalités de traitement de requêtes intelligent pour améliorer les performances des requêtes dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 05/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords: ''
ms.assetid: ''
author: joesackmsft
ms.author: josack
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7786fd048f1698c90f379450b31e0bac3457706e
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="intelligent-query-processing-in-sql-databases"></a>Traitement de requêtes intelligent dans les bases de données SQL
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

La famille des fonctionnalités de **traitement de requêtes intelligent** contient des fonctionnalités qui améliorent les performances des charges de travail existantes avec un minimum d’effort d’implémentation.   Il s’agit notamment d’améliorations des constructions préexistantes et de l’introduction de méthodes et de capacités adaptatives.  

## <a name="adaptive-query-processing"></a>Traitement de requêtes adaptatif
Dans la famille des fonctionnalités de traitement de requête intelligent, l’une d’entre elles a été introduite dans SQL Server 2017 et Azure SQL Database pour ajouter de nouvelles capacités générales de traitement des requêtes afin d’adapter les stratégies d’optimisation aux conditions d’exécution de votre charge de travail d’application :
- **Jointures adaptatives en mode batch**. Cette fonctionnalité permet à votre plan de basculer dynamiquement sur une meilleure stratégie de jointure pendant l’exécution à l’aide d’un seul plan mis en cache.
- **Retour d’allocation de mémoire en mode batch**. Cette fonctionnalité recalcule la mémoire réelle nécessaire pour une requête, puis met à jour la valeur d’allocation pour le plan mis en cache, ce qui réduit les allocations de mémoire excessives qui impactent la concurrence et corrige les allocations de mémoire sous-estimées qui entraînent des dépassements coûteux sur le disque.
- **Exécution entrelacée pour les fonctions table à instructions multiples (MSTVF)**. Avec l’exécution entrelacée, nous utilisons le nombre réel de lignes de la fonction pour prendre des décisions de plan de requête en aval plus avisées. 

Pour plus d’informations sur le traitement de requêtes adaptatif, consultez [Traitement de requêtes adaptatif dans les bases de données SQL](../../relational-databases/performance/adaptive-query-processing.md).

## <a name="see-also"></a>Voir aussi
[Centre de performances pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)    
[Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[Jointures](../../relational-databases/performance/joins.md)    
[Illustration du traitement de requêtes adaptatif](https://github.com/joesackmsft/Conferences/blob/master/Data_AMP_Detroit_2017/Demos/AQP_Demo_ReadMe.md)        
