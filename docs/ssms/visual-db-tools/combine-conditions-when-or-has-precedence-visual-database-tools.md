---
title: Associer des conditions avec priorité à l’opérateur OR (Visual Database Tools) | Microsoft Docs
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
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- combining search conditions
- OR operator
ms.assetid: b30f5ac9-25e7-4163-80ed-44e4bccb455d
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4c8ef6c7e40e86710f405d1c293f55f48ee2197d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="combine-conditions-when-or-has-precedence-visual-database-tools"></a>Associer des conditions avec priorité à l'opérateur OR (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Pour relier des conditions à l'aide de l'opérateur OR et leur accorder la priorité sur les conditions reliées à l'aide de l'opérateur AND, vous devez répéter la condition AND pour chaque condition OR.  
  
Imaginez que vous souhaitez, par exemple, rechercher tous les employés travaillant dans la société depuis plus de cinq ans et occupant des postes de faible niveau ou étant à la retraite. Cette requête nécessite trois conditions, une condition reliée à deux autres conditions à l'aide de l'opérateur AND :  
  
-   Employés dont la date d'embauche remonte à plus de cinq ans et  
  
-   Employés dont le niveau de poste s'élève à 100 ou dont le statut est défini sur « R » (Retraite)  
  
Pour créer ce type de requête dans le volet Critères, suivez la procédure ci-dessous.  
  
### <a name="to-combine-conditions-when-or-has-precedence"></a>Pour associer des conditions avec priorité à l'opérateur OR  
  
1.  Dans le [volet Critères](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md), ajoutez les colonnes de données dans lesquelles vous souhaitez effectuer la recherche. Si vous souhaitez effectuer la recherche dans une même colonne à l'aide de deux conditions (voire plus) reliées à l'aide de l'opérateur AND, vous devez ajouter le nom de cette colonne de données à la grille pour chacune des valeurs que vous souhaitez rechercher.  
  
2.  Créez les conditions à relier à l'aide de l'opérateur OR en entrant la première condition dans la colonne de la grille **Filtre** et la deuxième condition (ainsi que les conditions suivantes) dans des colonnes **Ou...** distinctes. Par exemple, pour relier à l'aide de l'opérateur OR des conditions effectuant une recherche dans les colonnes `job_lvl` et `status` , entrez `= 100` dans la colonne **Filtre** pour `job_lvl` et `= 'R'` dans la colonne **Ou...** pour `status`.  
  
    La saisie de ces valeurs dans la grille génère la clause WHERE suivante dans l'instruction figurant dans le volet SQL :  
  
    ```  
    WHERE (job_lvl = 100) OR (status = 'R')  
    ```  
  
3.  Créez la condition AND en la définissant pour chaque condition OR. Effectuez chaque entrée dans la même colonne de la grille que celle de la condition OR à laquelle elle correspond. Pour ajouter, par exemple, une condition AND effectuant une recherche dans la colonne `hire_date` et s'appliquant aux deux conditions OR, entrez `< '1/1/91'` dans la colonne Critères et dans la colonne **Ou...** .  
  
    La saisie de ces valeurs dans la grille génère la clause WHERE suivante dans l'instruction figurant dans le volet SQL :  
  
    ```  
    WHERE (job_lvl = 100) AND   
      (hire_date < '01/01/91' ) OR  
      (status = 'R') AND   
      (hire_date < '01/01/91' )  
    ```  
  
    > [!TIP]  
    > Vous pouvez répéter une condition AND en l'ajoutant une fois, puis en utilisant les commandes **Couper** et **Coller** du menu **Edition** pour la répéter dans d'autres conditions OR.  
  
La clause WHERE créée par le Concepteur de requêtes et de vues équivaut à la clause WHERE suivante, qui utilise des parenthèses pour spécifier la priorité de l'opérateur OR sur l'opérateur AND :  
  
```  
WHERE (job_lvl = 100 OR status = 'R') AND  
   (hire_date < '01/01/91')  
```  
  
> [!NOTE]  
> Quand vous entrez des conditions de requête au format indiqué immédiatement au-dessus du [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md), mais que vous apportez par la suite des modifications à la requête figurant dans le volet Schéma ou dans le volet Critères, le Concepteur de requêtes recrée l'instruction SQL pour qu'elle corresponde à ce format, avec la condition AND répartie de manière explicite vers les deux conditions OR.  
  
## <a name="see-also"></a> Voir aussi  
[Conventions pour la combinaison de conditions de recherche dans le volet Critères &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
