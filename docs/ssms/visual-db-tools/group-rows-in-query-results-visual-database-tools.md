---
title: Regrouper des lignes dans les résultats de requête (Visual Database Tools) | Microsoft Docs
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
- summarizing table subsets
- grouping rows
- grouping query results
ms.assetid: b07082d5-4d55-4903-9af9-4c470554c6d3
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 72ef5527dd8cc1c6feae1eeb41e09b479425810c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="group-rows-in-query-results-visual-database-tools"></a>Regrouper des lignes dans les résultats de requête (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Pour créer des sous-totaux ou afficher d'autres informations de synthèse pour les sous-ensembles d'une table, créez des groupes à l'aide d'une requête d'agrégation. Chaque groupe synthétise les données pour toutes les lignes de la table qui ont la même valeur.  
  
Imaginons, par exemple, que vous souhaitiez voir le prix moyen d'un livre dans la table `titles` , en détaillant toutefois les résultats par éditeur. Pour ce faire, groupez la requête par éditeur (par exemple, `pub_id`). La requête donnera, par exemple, le résultat suivant :  
  
![Résultats de la requête : prix moyen groupé par éditeur](../../ssms/visual-db-tools/media/dv3w9e1.gif "Résultats de la requête : prix moyen groupé par éditeur")  
  
Lorsque vous groupez des données, vous pouvez uniquement afficher des données groupées ou des données de synthèse, dont notamment les suivantes :  
  
-   Valeurs des colonnes groupées (celles qui apparaissent dans la clause GROUP BY). Dans l'exemple ci-dessus, `pub_id` est la colonne groupée.  
  
-   Valeurs générées par des fonctions d’agrégation comme une somme SUM( ) et une moyenne AVG( ). Dans l’exemple ci-dessus, la deuxième colonne est obtenue par l’utilisation de la fonction AVG( ) avec la colonne `price` .  
  
Il est impossible d'afficher des valeurs issues de lignes individuelles. Par exemple, si vous effectuez un regroupement uniquement par éditeur, vous ne pouvez pas afficher en plus les titres individuels dans la requête. Ainsi, si vous ajoutez des colonnes au résultat de la requête, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) les ajoute automatiquement à la clause GROUP BY de l’instruction dans le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md). Si en revanche vous souhaitez agréger une colonne, spécifiez une fonction d'agrégation pour cette colonne.  
  
Si vous groupez par plusieurs colonnes, chaque groupe de la requête affiche les valeurs d'agrégation pour toutes les colonnes de regroupement.  
  
Ainsi, la requête suivante exécutée sur la table `titles` regroupe par éditeur (`pub_id`) et par type de livre (`type`). Les résultats de la requête sont classés par éditeur et affichent des informations de synthèse pour chaque type de livre publié par l'éditeur :  
  
```  
SELECT pub_id, type, SUM(price) Total_price  
FROM titles  
GROUP BY pub_id, type  
```  
  
Le résultat obtenu peut se présenter comme suit :  
  
![Résultats de la requête : prix groupé par éditeur et par type](../../ssms/visual-db-tools/media/dv3w9e2.gif "Résultats de la requête : prix groupé par éditeur et par type")  
  
### <a name="to-group-rows"></a>Pour grouper des lignes  
  
1.  Commencez la requête en ajoutant les tables à synthétiser dans le volet Schéma.  
  
2.  Cliquez avec le bouton droit sur un point de l’arrière-plan du volet Schéma, puis choisissez **Ajouter un groupe par** dans le menu contextuel. Le Concepteur de requêtes et de vues ajoute une colonne **Grouper par** à la grille dans le volet Critères.  
  
3.  Ajoutez la ou les colonnes à grouper dans le volet Critères. Si la colonne doit apparaître dans le résultat de la requête, vérifiez que la colonne **Sortie** est sélectionnée pour la sortie.  
  
    Le Concepteur de requêtes et de vues ajoute une clause GROUP BY à l'instruction dans le volet SQL. Par exemple, l'instruction SQL peut se présenter de la manière suivante :  
  
    ```  
    SELECT pub_id  
    FROM titles  
    GROUP BY pub_id  
    ```  
  
4.  Ajoutez la ou les colonnes à agréger dans le volet Critères. Assurez-vous que la colonne est marquée pour la sortie.  
  
5.  Dans la cellule de la grille **Grouper par** de la colonne à agréger, sélectionnez la fonction d’agrégation appropriée.  
  
    Le Concepteur de requêtes et de vues assigne automatiquement un alias de colonne à la colonne que vous agrégez. Il est possible de remplacer l'alias généré automatiquement par un autre, plus significatif. Pour plus d’informations, consultez [Créer des alias de colonnes (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
    ![Ajout d’un alias de colonne au jeu de résultats de la requête](../../ssms/visual-db-tools/media/dv3w9e3.gif "Ajout d’un alias de colonne au jeu de résultats de la requête")  
  
    L’instruction correspondante affichée dans le volet **SQL** peut se présenter comme suit :  
  
    ```  
    SELECT   pub_id, SUM(price) AS Totalprice  
    FROM     titles  
    GROUP BY pub_id  
    ```  
  
## <a name="see-also"></a> Voir aussi  
[Trier et regrouper des résultats de requête (Visual Database Tools)](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
  
