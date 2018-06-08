---
title: Volet Critères (Visual Database Tools) | Microsoft Docs
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
- Query Designer [SQL Server], Criteria pane
- View Designer, Criteria pane
- entering query options into grid [SQL Server]
- Criteria pane
- inserting query options into grid
- grid showing query options [SQL Server]
- adding query options into grid
ms.assetid: 6291affe-580e-482f-a7ff-45ce3837956a
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 83859095e8a252b59ab4c1bd03ceb65e971bc907
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="criteria-pane-visual-database-tools"></a>Volet Critères (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Ce volet permet d'utiliser une grille similaire à une feuille de calcul pour spécifier des options de requêtes, telles que les colonnes de données à afficher, la façon de trier les résultats et les lignes à sélectionner. Dans le volet Critères, vous pouvez spécifier les éléments suivants :  
  
-   Les colonnes à afficher et des alias de noms de colonnes.  
  
-   La table à laquelle une colonne appartient.  
  
-   Des expressions de colonnes calculées.  
  
-   L'ordre de tri de la requête.  
  
-   Des conditions de recherche.  
  
-   Des critères de regroupement, dont des fonctions d'agrégation à utiliser pour les synthèses.  
  
-   Des nouvelles valeurs pour les requêtes UPDATE ou INSERT INTO.  
  
-   Des noms de colonnes cibles dans le cas de requêtes INSERT FROM.  
  
Les modifications que vous apportez dans le volet Critères se reflètent automatiquement dans les volets Schéma et SQL. De même, le volet Critères est automatiquement mis à jour pour refléter les modifications apportées dans les autres volets.  
  
## <a name="about-the-criteria-pane"></a>A propos du volet Critères  
Les lignes du volet Critères affichent les colonnes de données utilisées dans votre requête ; les colonnes affichent les options de la requête.  
  
Les informations spécifiques apparaissant dans le volet Critères dépendent du type de requête que vous créez.  
  
Si le volet Critères n'est pas visible, cliquez avec le bouton droit sur le concepteur, pointez sur **Volet**, puis cliquez sur **Critères**.  
  
## <a name="options"></a>Options  
  
|**Colonne**|**Type de requête**|**Description**|  
|--------------|------------------|-------------------|  
|colonne|All|Affiche soit le nom d'une colonne de données utilisée pour la requête, soit l'expression d'une colonne calculée. La colonne est verrouillée pour rester toujours visible lors d'un défilement horizontal.|  
|Alias|SELECT, INSERT FROM, UPDATE, MAKE TABLE|Spécifie soit un autre nom à donner à la colonne, soit le nom d'une colonne calculée.|  
|Table de charge de travail|SELECT, INSERT FROM, UPDATE, MAKE TABLE|Spécifie le nom de la table ou de l'objet structuré en table contenant la colonne de données associée. Cette colonne est vide dans le cas de colonnes calculées.|  
|Sortie|SELECT, INSERT FROM, MAKE TABLE|Spécifie si une colonne de données apparaît dans le résultat de la requête.<br /><br />Remarque : Si la base de données le permet, vous pouvez mettre les clauses de tri ou de recherche dans une colonne de données que vous n'afficherez pas dans l'ensemble des résultats.|  
|Type de tri|SELECT, INSERT FROM|Spécifie si la colonne de données associée sert à trier les résultats de la requête et si le tri est croissant ou décroissant.|  
|Ordre de tri|SELECT, INSERT FROM|Spécifie l'ordre de priorité dans lequel les colonnes de données seront triées dans l'ensemble des résultats. Lorsque vous modifiez l'ordre de tri d'une colonne de données, toutes les autres colonnes sont mises à jour en conséquence.|  
|Regrouper par|SELECT, INSERT FROM, MAKE TABLE|Spécifie que la colonne de données associée est utilisée pour créer une requête d'agrégation. Cette colonne n'apparaît que si, dans le menu **Outils** , vous avez choisi **Regroupement** ou si, dans le volet SQL, vous avez ajouté une clause GROUP BY.<br /><br />Par défaut, cette colonne a la valeur **Regrouper par**et la colonne associée devient partie intégrante de la clause GROUP BY.<br /><br />Si vous allez dans une cellule de cette colonne, puis sélectionnez une fonction d'agrégation à appliquer à la colonne de données associée, par défaut l'expression obtenue s'ajoute en tant que colonne de sortie de l'ensemble des résultats.|  
|Critères|All|Spécifie une condition de recherche (filtre) dans la colonne de données associée. Entrez un opérateur (« = » par défaut) ainsi que la valeur à rechercher. Si la valeur est un texte, encadrez ce dernier par des apostrophes.<br /><br />Si la colonne de données associée fait partie d'une clause GROUP BY, l'expression entrée est utilisée dans une clause HAVING.<br /><br />Si, dans la colonne **Critères** de la grille, vous entrez des valeurs dans plusieurs cellules, les conditions de recherche qui en résultent sont automatiquement liées par un ET logique.<br /><br />Pour que plusieurs expressions de conditions de recherche s'appliquent à une même colonne de base de données, par exemple (fname > 'A') AND (fname < 'M'), ajoutez deux fois la colonne de données au volet Critères et, dans la colonne **Critères**, entrez une valeur distincte par instance de cette colonne de données.|  
|Ou...|All|Spécifie une expression ajoutant une condition de recherche de la colonne de données, liée aux expressions précédentes par un OU logique. Pour ajouter des colonnes **Ou...** à la grille, appuyez sur la touche Tab dans la colonne **Ou...** la plus à droite.|  
|Ajouter|INSERT FROM|Spécifie le nom de la colonne de données cible de la colonne de données associée. Lorsque vous créez une requête Insert From, le Concepteur de requêtes et de vues essaie de faire correspondre la source à une colonne de données cible appropriée. Si le Concepteur de requêtes et de vues ne peut pas choisir de correspondance, vous devez indiquer le nom de la colonne.|  
|Nouvelle valeur|UPDATE, INSERT INTO|Spécifie la valeur à placer dans la colonne associée. Entrez une valeur littérale ou une expression.|  
  
## <a name="see-also"></a> Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Volet Diagramme &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md)  
[Règles pour l'entrée de valeurs de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/rules-for-entering-search-values-visual-database-tools.md)  
[Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Résultats (volet) &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/results-pane-visual-database-tools.md)  
[SQL (volet) &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md)  
  
