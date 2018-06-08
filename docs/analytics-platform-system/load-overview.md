---
title: Chargement de données dans Parallel Data Warehouse | Documents Microsoft
description: Vous pouvez charger ou insérer des données dans SQL Server Parallel Data Warehouse (PDW) à l’aide des Services d’intégration, utilitaire bcp, dwloader ou l’instruction SQL INSERT.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 3fed89686683616164132cf0322e3709eab78f32
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="loading-data-into-parallel-data-warehouse"></a>Chargement des données dans Parallel Data Warehouse
Vous pouvez charger ou insérer des données dans SQL Server Parallel Data Warehouse (PDW) à l’aide des Services d’intégration, [utilitaire bcp](../tools/bcp-utility.md), **dwloader** du chargeur de ligne de commande ou l’instruction SQL INSERT.  

## <a name="loading-environment"></a>Environnement de chargement  
Pour charger des données, vous avez besoin d’un ou plusieurs serveurs de chargement. Vous pouvez utiliser votre propre ETL existant ou autres serveurs, ou vous pouvez acheter de nouveaux serveurs. Pour plus d’informations, consultez [acquérir et de configurer un serveur de chargement](acquire-and-configure-loading-server.md). Ces instructions incluent un [le chargement des feuille de planification de capacité de serveur](loading-server-capacity-planning-worksheet.md) pour vous aider à planifier la solution appropriée pour le chargement.  
  
## <a name="load-with-dwloader"></a>Charger avec dwloader  
À l’aide de la [dwloader de ligne de commande chargeur](dwloader.md) est le moyen le plus rapide pour charger des données dans PDW.  
  
![Processus de chargement](media/loading-process.png "processus de chargement")  
  
dwloader charge les données directement aux nœuds de calcul sans passer les données via le nœud du contrôle. Pour charger des données, dwloader communique en premier lieu avec le nœud de contrôle pour obtenir des informations de contact pour les nœuds de calcul. dwloader configure un canal de communication avec chaque nœud de calcul, puis envoie les segments de 256 Ko de données pour les nœuds de calcul de manière alternée.  
  
Sur chaque nœud de calcul, Service de déplacement des données (DMS) reçoit et traite les segments de données. Le traitement des données inclut la conversion de chaque ligne dans le format natif de SQL Server et calculer le hachage de distribution pour déterminer le nœud de calcul à laquelle chaque ligne appartient.  
  
Après avoir traité les lignes, DMS utilise un déplacement aléatoire pour transférer chaque ligne vers le nœud de calcul approprié et l’instance de SQL Server. Que SQL Server reçoit les lignes, il traite les conformément à la **– b** paramètre de taille de lot définie dans dwloader et en bloc charge ensuite le lot.  

## <a name="load-with-prepared-statements"></a>Charger avec des instructions préparées

Vous pouvez utiliser des instructions préparées pour charger des données dans les tables répliquées et distribuées. Lorsque les données d’entrée ne correspondent pas le type de données cible, une conversion implicite est effectuée. Les conversions implicites pris en charge par PDW d’instructions préparées sont un sous-ensemble des conversions prises en charge par SQL Server. Autrement dit, seul un sous-ensemble de conversions sont prises en charge, mais les conversions prises en charge correspondent aux conversions implicites de SQL Server. Indépendamment de si la table cible à charger est définie comme une table distribuée ou répliquée, les conversions implicites sont appliquées (si nécessaire) pour toutes les colonnes qui existent dans la table cible. 

<!-- MISSING LINK
For more information, see [Prepared statements](prepared-statements.md).
-->
  
## <a name="related-tasks"></a>Tâches associées  
  
|Tâche| Description|  
|--------|---------------|  
|Créer la base de données mise en lots.|[Créer la base de données mise en lots](staging-database.md)|  
|Charger avec Integration Services.|[Charger avec Integration Services](load-with-ssis.md)|  
|Comprendre les conversions de types de dwloader.|[Règles de conversion du type de données pour dwloader](dwloader-data-type-conversion-rules.md)|  
|Charger les données avec dwloader.|[Chargeur de ligne de commande de dwloader](dwloader.md)|  
|Comprendre les conversions de type pour l’insertion.|[Charger des données avec INSERT](load-with-insert.md)|  
 
<!-- MISSING LINKS
## See Also  
[Grant permissions to load data](grant-permissions-to-load-data.md)  
[Common metadata query examles](metadata-query-examples.md)  
  
-->
