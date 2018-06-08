---
title: Réduire des groupes de lignes (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- group collapsing [SQL Server]
- collapsing rows
- row collapsing [SQL Server]
ms.assetid: 7338dad0-965d-44ba-8c1a-b993acb7156d
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5657c180de1c66320b4eba4c33058f625c0d1402
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="collapse-groups-of-rows-visual-database-tools"></a>Réduire des groupes de lignes (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez créer un résultat de requête dans lequel chaque ligne de résultat correspond à tout un groupe de lignes des données d'origine. La réduction de lignes donne les possibilités suivantes :  
  
-   **Élimination de lignes en double** Certaines requêtes donnent des ensembles de résultats faisant apparaître plusieurs lignes identiques. Par exemple, vous pouvez créer un ensemble de résultats où les lignes citent une ville et son état si un auteur quelconque y réside, mais si plusieurs auteurs résident dans la même ville, vous obtiendrez plusieurs lignes identiques. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
    ```  
    SELECT city, state  
    FROM authors  
    ```  
  
    L'ensemble des résultats généré par la requête ci-dessus n'est pas très utile. Si quatre auteurs résident dans la même ville, l'ensemble des résultats inclut quatre lignes identiques. Comme l'ensemble des résultats n'inclut pas d'autres colonnes que celles de la ville et de l'état, il n'y a aucun moyen de faire une distinction entre les lignes identiques obtenues. Une façon d'éviter ces doublons serait d'inclure d'autres colonnes pour établir une distinction entre les lignes. Par exemple, si vous incluez le nom de l'auteur, les risques de doublons diminuent (les seuls doublons seront les lignes dans lesquelles deux auteurs de même nom résident dans la même ville). L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
    ```  
    SELECT city, state, fname, minit, lname  
    FROM authors  
    ```  
  
    Si la requête précédente élimine le symptôme, elle ne résout cependant pas vraiment le problème. Vous pouvez obtenir un ensemble de résultats sans doublon, mais il ne s'agit plus d'un ensemble de résultats sur les villes. Pour éliminer les doublons de l'ensemble des résultats d'origine et lui garder son but, les villes dans lesquelles réside un auteur, créez une requête ne retournant des lignes que lorsqu'elles sont différentes les unes des autres. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
    ```  
    SELECT DISTINCT city, state  
    FROM authors  
    ```  
  
    Pour plus de détails sur l’élimination des doublons, consultez [Exclure les doublons de lignes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/exclude-duplicate-rows-visual-database-tools.md).  
  
-   **Calculs sur des groupes de lignes** Vous pouvez synthétiser des informations par groupe de lignes. Par exemple, il serait possible de demander un ensemble de résultats dans lequel chaque ligne indiquerait le nom d'une ville si elle contient un auteur, ainsi que le nombre d'auteurs résidant dans la ville. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
    ```  
    SELECT city, state, COUNT(*)  
    FROM authors  
    GROUP BY city, state  
    ```  
  
    Pour plus d’informations sur les calculs possibles sur des groupes de lignes, consultez [Résumer les résultats de la requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md) et [Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md).  
  
-   **Utilisation de critères de sélection pour inclure des groupes de lignes** Par exemple, il serait possible de demander un ensemble de résultats dans lequel chaque ligne indiquerait le nom d'une ville et son état si elle contient plusieurs auteurs, ainsi que le nombre d'auteurs résidant dans la ville. L'instruction SQL obtenue peut se présenter de la manière suivante :  
  
    ```  
    SELECT city, state, COUNT(*)  
    FROM authors  
    GROUP BY city, state  
    HAVING COUNT(*) > 1  
    ```  
  
    Pour plus d’informations sur la façon d’appliquer des critères de sélection aux groupes de lignes, consultez [Spécifier des conditions pour des groupes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-conditions-for-groups-visual-database-tools.md) et [Utiliser les clauses HAVING et WHERE dans la même requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/use-having-and-where-clauses-in-the-same-query-visual-database-tools.md).  
  
## <a name="see-also"></a> Voir aussi  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
