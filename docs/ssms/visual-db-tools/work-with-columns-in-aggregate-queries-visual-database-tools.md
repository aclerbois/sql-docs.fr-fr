---
title: Utiliser des colonnes dans des requêtes d’agrégation (Visual Database Tools) | Microsoft Docs
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
- HAVING clause, query summary results
- GROUP BY clause, query summary results
- aggregate queries [SQL Server]
- WHERE clause, query summary results
ms.assetid: 1b82681f-3d4f-4b9a-bb1d-2060e44f2577
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3a653cc44b5f9b654df1a46cf1b76d4064b808a7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-columns-in-aggregate-queries-visual-database-tools"></a>Utiliser des colonnes dans des requêtes d'agrégation (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Quand vous créez des requêtes d’agrégation, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) fait certaines suppositions afin de construire une requête valide. Par exemple, si vous créez une requête d'agrégation puis placez un marqueur de sortie sur une colonne de données, cette dernière est automatiquement intégrée à la clause GROUP BY par le Concepteur de requêtes et de vues pour éviter l'affichage accidentel du contenu d'une ligne individuelle dans une synthèse.  
  
## <a name="using-group-by"></a>Utilisation de Regrouper par  
Le Concepteur de requêtes et de vues suit les instructions ci-dessous lorsque vous utilisez des colonnes :  
  
-   Si vous choisissez l'option Regrouper par ou lorsque vous ajoutez une fonction d'agrégation à une requête, toutes les colonnes ayant reçu un marqueur de sortie ou utilisées dans des tris s'ajoutent automatiquement à la clause GROUP BY. Les colonnes ne s'ajoutent pas automatiquement à la clause GROUP BY si elles font déjà partie d'une fonction d'agrégation.  
  
    Si vous ne souhaitez pas qu'une colonne particulière fasse partie de la clause GROUP BY, sélectionnez manuellement une autre option dans la colonne Regrouper par du volet Critères. Cependant, le Concepteur de requêtes et de vues ne vous interdit pas de choisir une option qui risque d'empêcher l'exécution de la requête.  
  
-   Si vous ajoutez manuellement à une fonction d'agrégation une colonne de sortie de la requête, que ce soit dans le volet Critères ou le volet SQL, le Concepteur de requêtes et de vues ne supprime pas automatiquement les autres colonnes de sortie de la requête. Par conséquent, vous devez supprimer du résultat de la requête les colonnes restantes ou les intégrer à la clause GROUP BY ou à une fonction d'agrégation.  
  
Lorsque vous entrez une condition de recherche dans la colonne Filtre du volet Critères, le Concepteur de requêtes et de vues respecte les règles suivantes :  
  
-   Si la colonne **Regrouper par** de la grille ne s’affiche pas (car vous n’avez pas encore spécifié de requête d’agrégation), la condition de recherche se place dans la clause WHERE.  
  
-   Si vous vous trouvez déjà dans une requête d’agrégation et avez sélectionné, dans la colonne **Regrouper par** , l’option **Où** , la condition de recherche se place dans la clause WHERE.  
  
-   Si la colonne **Regrouper par** contient une autre valeur que la valeur **Où**, la condition de recherche se place dans la clause HAVING.  
  
## <a name="using-the-having-and-where-clauses"></a>Utilisation des clauses HAVING et WHERE  
Les principes suivants décrivent comment, dans une requête d'agrégation, référencer des colonnes dans des conditions de recherche : en général, dans une condition de recherche, une colonne sert à filtrer les lignes dont vous souhaitez faire la synthèse (clause WHERE) ou à déterminer quels résultats groupés apparaîtront dans la sortie finale (clause HAVING).  
  
-   Des colonnes de données individuelles peuvent apparaître dans la clause WHERE ou HAVING, selon la façon dont elles sont utilisées ailleurs dans la requête.  
  
-   Comme les clauses WHERE sélectionnent un sous-ensemble de lignes faisant l'objet de la synthèse ou du regroupement, elles s'appliquent avant tout regroupement. Par conséquent, vous pouvez utiliser une colonne de données dans une clause WHERE même si elle ne fait pas partie de la clause GROUP BY ou si elle n'est pas incluse dans une fonction d'agrégation. Par exemple, l'instruction suivante sélectionne tous les titres dépassant 10 dollars et donne le prix moyen :  
  
    ```  
    SELECT AVG(price)  
    FROM titles  
    WHERE price > 10  
    ```  
  
-   Selon ce que vous décidez lorsque vous créez une condition de recherche impliquant une colonne également utilisée dans une clause GROUP BY ou dans une fonction d'agrégation, cette condition peut apparaître soit sous la forme d'une clause WHERE, soit sous la forme d'une clause HAVING. Par exemple, l'instruction suivante calcule le prix moyen des titres par éditeur, puis affiche la moyenne globale des prix pratiqués par les éditeurs dont le prix moyen est supérieur à 10 dollars :  
  
    ```  
    SELECT pub_id, AVG(price)  
    FROM titles  
    GROUP BY pub_id  
    HAVING (AVG(price) > 10)  
    ```  
  
-   Si vous utilisez une fonction d'agrégation dans une condition de recherche, cette dernière implique une synthèse. Par conséquent, elle doit faire partie de la clause HAVING.  
  
## <a name="see-also"></a> Voir aussi  
[Résumer les résultats de la requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
[Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
  
