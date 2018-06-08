---
title: Créer manuellement des jointures réflexives (Visual Database Tools) | Microsoft Docs
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
- self-joins
- manual joins [SQL Server]
- joins [SQL Server], self
ms.assetid: 910ed516-cb84-481b-95d0-cba3e89afdba
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e7046e5ac1593f9af287065f7c486d9b1c60a945
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-self-joins-manually-visual-database-tools"></a>Créer manuellement des jointures réflexives (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez joindre une table à elle-même, même si elle ne possède pas de relation réflexive dans la base de données. Vous pouvez, par exemple, utiliser une jointure réflexive pour trouver des paires d'auteurs vivant dans la même ville.  
  
À l'instar de toute autre jointure, une jointure réflexive nécessite au moins deux tables. En revanche, elle diffère des autres jointures par le fait que vous ajoutez une seconde instance de la même table plutôt qu'une autre table à la requête. Ainsi, vous pouvez comparer une colonne de la première instance de la table à la même colonne de la seconde, ce qui vous permet de comparer les valeurs d'une même colonne. Le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) assigne un alias à la seconde instance de la table.  
  
Par exemple, si vous créez une jointure réflexive pour trouver toutes les paires d'auteurs résidant à Berkeley, comparez la colonne `city` de la première instance de la table à la colonne `city` de la seconde instance. La requête obtenue peut se présenter comme suit :  
  
```  
SELECT   
         authors.au_fname,   
         authors.au_lname,   
         authors1.au_fname AS Expr2,   
         authors1.au_lname AS Expr3  
      FROM   
         authors   
            INNER JOIN  
            authors authors1   
               ON authors.city   
                = authors1.city  
      WHERE  
         authors.city = 'Berkeley'  
```  
  
La création d'une jointure réflexive exige souvent plusieurs conditions de jointure. Pour en comprendre les raisons, observez le résultat de la requête précédente :  
  
```  
Cheryl Carson       Cheryl Carson  
   Abraham Bennet      Abraham Bennet  
   Cheryl Carson       Abraham Bennet  
   Abraham Bennet      Cheryl Carson  
```  
  
La première ligne est sans importance, elle indique que Cheryl Carson vit dans la même ville que Cheryl Carson. La deuxième est tout aussi inutile. Pour éliminer les données inutiles, ajoutez une autre condition afin de ne conserver que les lignes de résultats affichant des noms d'auteur différents. La requête obtenue peut se présenter comme suit :  
  
```  
SELECT   
         authors.au_fname,   
         authors.au_lname,   
         authors1.au_fname AS Expr2,   
         authors1.au_lname AS Expr3  
      FROM   
         authors   
            INNER JOIN  
            authors authors1   
               ON authors.city   
                = authors1.city  
               AND authors.au_id  
                <> authors1.au_id  
      WHERE  
         authors.city = 'Berkeley'  
```  
  
Le jeu de résultats est affiné :  
  
```  
Cheryl Carson       Abraham Bennet  
   Abraham Bennet      Cheryl Carson  
```  
  
Néanmoins, les deux lignes de résultats sont redondantes. La première indique que Carson vit dans la même ville que Bennet et la seconde que Bennet vit dans la même ville que Carson. Pour éliminer cette redondance, vous pouvez remplacer la seconde condition de jointure « différent de » par « inférieur à ». La requête obtenue peut se présenter comme suit :  
  
```  
SELECT   
         authors.au_fname,   
         authors.au_lname,   
         authors1.au_fname AS Expr2,   
         authors1.au_lname AS Expr3  
      FROM   
         authors   
            INNER JOIN  
            authors authors1   
               ON authors.city   
                = authors1.city  
               AND authors.au_id  
                < authors1.au_id  
      WHERE  
         authors.city = 'Berkeley'  
```  
  
Le jeu de résultats ressemble alors à ceci :  
  
```  
Cheryl Carson       Abraham Bennet  
```  
  
### <a name="to-create-a-self-join-manually"></a>Pour créer manuellement une jointure réflexive  
  
1.  Ajoutez au [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) la table ou l’objet table que vous souhaitez utiliser.  
  
2.  Ajoutez de nouveau la même table, afin que le volet Schéma affiche deux fois la même table ou le même objet table.  
  
    Le Concepteur de requêtes et de vues assigne un alias à la seconde instance en ajoutant un numéro séquentiel au nom de la table. Par ailleurs, il crée une ligne de jointure entre les deux occurrences de la table ou l'objet table dans le volet Schéma.  
  
3.  Cliquez avec le bouton droit sur la ligne de jointure et cliquez sur **Propriétés** dans le menu contextuel.  
  
4.  Dans la fenêtre Propriétés, cliquez sur **Condition et type de jointure** et cliquez sur le bouton de sélection **(…)** , à droite de la propriété.  
  
5.  Dans la [boîte de dialogue Joindre](../../ssms/visual-db-tools/join-dialog-box-visual-database-tools.md) , modifiez éventuellement l’opérateur de comparaison entre les clés primaires. Indiquez par exemple l'opérateur inférieur à (<).  
  
6.  Créez la condition de jointure supplémentaire (par exemple, authors.zip = authors1.zip) en faisant glisser le nom de la colonne de jointure primaire figurant dans la première occurrence de la table ou de l'objet table jusqu'à la colonne correspondante de la seconde occurrence.  
  
7.  Spécifiez d'autres options de requête comme les colonnes de sortie, les conditions de recherche et l'ordre de tri.  
  
## <a name="see-also"></a> Voir aussi  
[Créer automatiquement des jointures réflexives &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-self-joins-automatically-visual-database-tools.md)  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
