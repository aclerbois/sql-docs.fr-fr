---
title: Index spatiaux, boîte de dialogue (Visual Database Tools) | Microsoft Docs
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
f1_keywords:
- vdt.dlgbox.spatialindexes
ms.assetid: 4d84239a-68c7-4aa2-8602-2b51dd07260f
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: fb7c869dbf087db4c59eb9e5ba30c48f6e33f055
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spatial-indexes-dialog-box-visual-database-tools"></a>Index spatiaux, boîte de dialogue (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Utilisez la boîte de dialogue **Index spatiaux** pour créer des index pour les colonnes du type de données **géométrie** or **géographie** (*colonnes spatiales*), qui ne peuvent pas être indexées à l'aide de la boîte de dialogue **Index/Clés** . Chaque colonne spatiale peut avoir plusieurs index spatiaux, mais ils doivent être créés un par un.  
  
Pour plus d'informations sur les restrictions relatives à la création d'index spatiaux, consultez [Vue d'ensemble des index spatiaux](http://msdn.microsoft.com/en-us/b1ae7b78-182a-459e-ab28-f743e43f8293).  
  
## <a name="options"></a>Options  
**Index spatial sélectionné**  
Répertorie les index spatiaux existants. Sélectionnez un index pour afficher ses propriétés. Si la liste est vide, aucun index spatial n'est défini pour la table.  
  
**Ajouter**  
Crée un index spatial.  
  
**Supprimer**  
Supprime l'index spatial sélectionné dans la liste **Index spatial sélectionné** .  
  
**Cellules par objet**  
Indique le nombre de cellules par objet de pavage qui peuvent être utilisées pour un objet spatial unique dans l'index. Ce nombre peut être un entier compris entre 1 et 8192 (inclus). La valeur par défaut est 16.  
  
Si un objet couvre plus de cellules que le nombre spécifié par *n*, l'indexation utilise autant de cellules que nécessaire pour fournir un pavage de niveau supérieur complet. Dans de tels cas, un objet peut recevoir plus de cellules que le nombre spécifié. Dans ce cas, le nombre maximal est le nombre de cellules générées par la grille de niveau supérieur, qui dépend de la densité du **Niveau 1** .  
  
**Colonnes**  
Indique le nom de colonne et l'ordre de tri.  
  
**IsSpatialIndex**  
Indique qu'un index spatial est sélectionné.  
  
**Niveau 1**  
Indique la densité de la grille de premier niveau (supérieur).  
  
**Niveau 2**  
Indique la densité de la grille de second niveau.  
  
**Niveau 3**  
Indique la densité de la grille de troisième niveau.  
  
**Niveau 4**  
Indique la densité de la grille de quatrième niveau.  
  
**Schéma de pavage**  
Indique le schéma de pavage :  
  
Options de colonne**geometry** :  
  
-   **Grille géométrique** pour une colonne de géométrie  
  
-   **Grille géographique** pour une colonne de géographie  
  
**Type**  
Indique qu'un index spatial est sélectionné.  
  
**Max. X**  
Spécifie la coordonnée x de l'angle supérieur droit du rectangle englobant. Cette propriété est estompée si le **Schéma de pavage** est une **Grille géographique**.  
  
**Min. X**  
Spécifie la coordonnée x de l'angle inférieur gauche du rectangle englobant. Cette propriété est estompée si le **Schéma de pavage** est une **Grille géographique**.  
  
**Max. Y**  
Spécifie la coordonnée y de l'angle supérieur droit du rectangle englobant. Cette propriété est estompée si le **Schéma de pavage** est une **Grille géographique**.  
  
**Min. Y**  
Spécifie la coordonnée y de l'angle inférieur gauche du rectangle englobant. Cette propriété est estompée si le **Schéma de pavage** est une **Grille géographique**.  
  
**Identity**  
Développée, elle affiche les champs de propriétés de **Nom** et **Description** .  
  
**(Nom)**  
Indique le nom de l'index spatial. Lorsqu'un nouvel index est créé, il obtient un nom par défaut basé sur la table affichée dans la fenêtre active du Concepteur de tables. Vous pouvez modifier le nom à tout moment.  
  
**Description**  
Décrit l'index. Pour écrire une description plus détaillée, cliquez sur **Description** , puis sur le bouton de sélection **(...)** qui apparaît à droite du champ de propriété. Vous obtiendrez une zone d'écriture plus large.  
  
**Catégorie Concepteur de tables**  
Développée, elle affiche des informations sur les propriétés de cet index spatial.  
  
**Spécification du remplissage**  
Développée, elle affiche des informations sur les options **Taux de remplissage** et **Index Pad**.  
  
**Taux de remplissage**  
Spécifie le pourcentage de la page d'index que le système peut remplir. Lorsqu'une page est pleine, le système doit la fractionner au cas où de nouvelles données seraient ajoutées, ce qui affaiblit les performances.  
  
-   La valeur 100 signifie que les pages seront remplies et occuperont un espace de stockage minimal, mais c'est la solution la moins efficace. Ce paramètre ne doit être utilisé que lorsqu'aucune modification ne sera apportée aux données, par exemple dans un tableau en lecture seule.  
  
-   Une valeur inférieure laisse davantage d'espace vide sur les ensembles de données, ce qui réduit le besoin de les fractionner à mesure que la taille des index augmente. Néanmoins, l'espace de stockage requis est plus important. Ce paramètre est plus approprié si vous avez l'intention de modifier les données contenues dans la table.  
  
**Index Pad**  
Donne aux pages de cet index le même pourcentage d'espace libre (de remplissage) que celui spécifié dans **Facteur de remplissage**.  
  
**Verrouillage de page autorisé**  
Spécifie si le verrouillage au niveau des pages est autorisé dans cet index. L'autorisation ou non du verrouillage au niveau de la page affecte les performances de la base de données.  
  
**Recalculer** **les statistiques**  
Spécifie si de nouvelles statistiques sont calculées une fois l'index créé. Le calcul des statistiques ralentit la génération des index, mais améliore généralement les performances des requêtes.  
  
**Verrouillage de ligne autorisé**  
Spécifie si le verrouillage au niveau des lignes est autorisé dans cet index. L'autorisation ou non du verrouillage au niveau de la ligne affecte les performances de la base de données.  
  
## <a name="see-also"></a> Voir aussi  
[Vue d'ensemble des index spatiaux](http://msdn.microsoft.com/en-us/b1ae7b78-182a-459e-ab28-f743e43f8293)  
  
