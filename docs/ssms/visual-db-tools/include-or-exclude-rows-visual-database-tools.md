---
title: Inclure ou exclure des lignes (Visual Database Tools) | Microsoft Docs
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
- search criteria [SQL Server], excluding rows
- search criteria [SQL Server], WHERE clause
- included rows
- search conditions [SQL Server], HAVING clause
- search criteria [SQL Server], including rows
- row excluded in search [SQL Server]
- search criteria [SQL Server], HAVING clause
- search conditions [SQL Server], WHERE clause
- row included in search [SQL Server]
- excluding rows
ms.assetid: ba4e1202-31a2-444d-8365-c68a530ef223
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 20f1a6b78a13a7ca81386dbccb7358378ec6f76b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="include-or-exclude-rows-visual-database-tools"></a>Inclure ou exclure des lignes (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Pour limiter le nombre de lignes qu'une requête SELECT doit renvoyer, créez des conditions de recherche ou des critères de filtre. Dans SQL, les conditions de recherche apparaissent dans la clause WHERE de l'instruction ou, si vous créez une requête d'agrégation, dans la clause HAVING.  
  
> [!NOTE]  
> Les conditions de recherche permettent également d'indiquer quelles lignes sont affectées par une requête appartenant à un des types suivants : Update, Insert Results, Insert Values, Delete ou Make Table.  
  
Lors de l'exécution de la requête, [!INCLUDE[ssDE](../../includes/ssde_md.md)] examine et applique la condition de recherche à chacune des tables dans lesquelles vous demandez une recherche. Si la ligne remplit la condition, elle est incluse dans la requête. Voici un exemple d'une condition de recherche de tous les employés d'une région particulière :  
  
```  
region = 'UK'  
```  
  
Pour établir les critères d'inclusion d'une ligne dans un résultat, vous pouvez utiliser plusieurs conditions de recherche. Par exemple, le critère de recherche suivant se compose de deux conditions de recherche. La requête n'inclut une ligne dans l'ensemble des résultats que si elle remplit les deux conditions.  
  
```  
region = 'UK' AND product_line = 'Housewares'  
```  
  
Vous pouvez combiner ces conditions par AND ou OR. L'exemple ci-dessus utilise AND. Le critère suivant utilise OR. Le jeu de résultats inclut toutes les lignes remplissant l'une des conditions de recherche, ou les deux :  
  
```  
region = 'UK' OR product_line = 'Housewares'  
```  
  
Vous pouvez même combiner plusieurs conditions de recherche dans une seule et même colonne. Par exemple, le critère suivant combine deux conditions dans la colonne region :  
  
```  
region = 'UK' OR region = 'US'  
```  
  
Pour plus de détails sur la combinaison de conditions de recherche, consultez les rubriques suivantes :  
  
-   [Conventions pour la combinaison de conditions de recherche dans le volet Critères &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
  
-   [Spécifier plusieurs conditions de recherche pour une colonne &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md)  
  
-   [Spécifier plusieurs conditions de recherche pour plusieurs colonnes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md)  
  
-   [Associer des conditions avec priorité à l'opérateur AND &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/combine-conditions-when-and-has-precedence-visual-database-tools.md)  
  
-   [Associer des conditions avec priorité à l'opérateur OR &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/combine-conditions-when-or-has-precedence-visual-database-tools.md)  
  
## <a name="examples"></a>Exemples  
Voici quelques exemples des requêtes utilisant plusieurs opérateurs et critères de lignes :  
  
-   **Littéral** Valeur logique, date, numérique ou de type texte. Dans l'exemple suivant, un littéral recherche toutes les lignes contenant des employés de la société travaillant au Royaume-Uni :  
  
    ```  
    WHERE region = 'UK'  
    ```  
  
-   **Référence de colonne** Compare les valeurs contenues dans une colonne avec celles contenues dans une autre. L'exemple suivant recherche dans une table `products` toutes les lignes dont le coût de production est inférieur au coût d'expédition :  
  
    ```  
    WHERE prod_cost < ship_cost  
    ```  
  
-   **Fonction** Référence à une fonction que la base de données principale peut résoudre pour calculer une valeur à rechercher. La fonction peut être définie par le serveur de base de données ou par l'utilisateur et renvoyer une valeur scalaire. L'exemple suivant recherche les commandes passées aujourd'hui (la fonction GETDATE( ) retourne la date du jour) :  
  
    ```  
    WHERE order_date = GETDATE()  
    ```  
  
-   **NULL** L'exemple suivant recherche dans une table `authors` tous les auteurs dont le prénom a été renseigné :  
  
    ```  
    WHERE au_fname IS NOT NULL  
    ```  
  
-   **Calcul** Résultat d'un calcul impliquant des littéraux, des références de colonnes ou d'autres expressions. L'exemple suivant recherche dans une table `products` toutes les lignes dont le prix de vente au détail dépasse le double du coût de production :  
  
    ```  
    WHERE sales_price > (prod_cost * 2)  
    ```  
  
## <a name="see-also"></a> Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
[Requête avec des paramètres &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-parameters-visual-database-tools.md)  
  
