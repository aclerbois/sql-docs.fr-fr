---
title: Créer des requêtes Insert Results (Visual Database Tools) | Microsoft Docs
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
- queries [SQL Server], types
- result sets [SQL Server], queries
- results [SQL Server], query
- Insert Results query
- queries [SQL Server], results
ms.assetid: 8770d630-09cc-47ec-a0e9-e9de2d7bbc89
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 91c84391103acc56a4602bb91d960557f190cb52
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-insert-results-queries-visual-database-tools"></a>Créer des requêtes Insert Results (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez copier des lignes d'une table à une autre ou au sein d'une même table à l'aide d'une requête Insert Results. Dans une table `titles` par exemple, vous pouvez utiliser une requête Insert Results pour copier des informations sur les titres publiés par un éditeur dans une seconde table que vous pouvez mettre à la disposition de cet éditeur. Une requête Insert Results ressemble à une requête Make Table, à la seule différence qu'elle copie des lignes dans une table existante.  
  
> [!TIP]  
> Vous pouvez également copier des lignes d'une table à une autre à l'aide de la fonctionnalité Couper-Coller. Créez une requête pour chaque table et exécutez les requêtes. Copiez les lignes de votre choix d'une grille de résultats vers une autre.  
  
Lorsque vous créez une requête Insert Results, spécifiez :  
  
-   la table de la base de données vers laquelle copier des lignes (la table de destination) ;  
  
-   les tables à partir desquelles copier les lignes (les tables sources), celles-ci devenant partie d'une sous-requête (si la copie est réalisée au sein d'une même table, la table source est la même que la table de destination) ;  
  
-   les colonnes de la table source dont vous souhaitez copier le contenu ;  
  
-   les colonnes cibles de la table de destination vers laquelle vous copiez les données ;  
  
-   les conditions de recherche pour définir les lignes à copier ;  
  
-   l'ordre de tri, afin de copier, le cas échéant, les lignes dans un ordre précis ;  
  
-   les options Group By, si vous ne souhaitez copier que des informations de synthèse.  
  
Dans l'exemple ci-dessous, la requête copie les informations de titres de la table `titles` vers une table d'archive appelée `archivetitles`. La requête copie le contenu des quatre colonnes de tous les titres appartenant à un éditeur donné :  
  
```  
INSERT INTO archivetitles   
   (title_id, title, type, pub_id)  
SELECT title_id, title, type, pub_id  
FROM titles  
WHERE (pub_id = '0766')  
```  
  
> [!NOTE]  
> Pour insérer des valeurs dans une nouvelle ligne, utilisez une requête Insert Values.  
  
Vous pouvez copier le contenu de colonnes sélectionnées ou de toutes les colonnes dans une ligne. Dans les deux cas, les données copiées doivent être compatibles avec les colonnes des lignes de destination de la copie. Par exemple, si vous copiez le contenu d'une colonne telle que `price`, la colonne de la ligne vers laquelle vous copiez les données doit accepter les données numériques avec décimales. Si vous copiez toute une ligne, la table de destination doit comporter des colonnes compatibles dont la position physique est identique à celle des colonnes de la table source.  
  
Lorsque vous créez une requête Insert Results, le volet Critères change afin de refléter les options de copie de données disponibles. Une colonne Ajout apparaît afin que vous puissiez spécifier les colonnes vers lesquelles copier les données.  
  
> [!CAUTION]  
> Il est impossible d'annuler l'action entraînée par l'exécution d'une requête Insert Results. Par sécurité, sauvegardez vos données avant d'exécuter la requête.  
  
### <a name="to-create-an-insert-results-query"></a>Pour créer une requête Insert Results  
  
1.  Créez une nouvelle requête et ajoutez la table dont vous souhaitez copier les lignes (la table source). Si vous copiez des lignes au sein d'une même table, vous pouvez ajouter la table source comme table de destination.  
  
2.  Dans le menu **Concepteur de requêtes** , pointez sur **Modifier le type**, puis cliquez sur **Insérer les résultats**.  
  
3.  Dans la boîte de dialogue [Choisir la table cible pour la requête Insérer les résultats](../../ssms/visual-db-tools/choose-target-table-for-insert-results-dialog-box-visual-database-tools.md), sélectionnez la table vers laquelle vous voulez copier les lignes (table de destination).  
  
    > [!NOTE]  
    > Le Concepteur de requêtes et de vues ne peut pas déterminer à l'avance les tables et vues qu'il est possible de mettre à jour. Ainsi, la liste **Nom de la table** de la boîte de dialogue **Choisir la table cible pour la requête Insérer les résultats** affiche toutes les tables et vues disponibles dans la connexion de données sur laquelle porte la requête, même celles vers lesquelles il est impossible de copier des lignes.  
  
4.  Dans le rectangle représentant la table ou l'objet table, choisissez les noms des colonnes dont vous souhaitez copier le contenu. Pour copier des lignes entières, choisissez **\&#42; (Toutes les colonnes)**.  
  
    Le Concepteur de requêtes et de vues ajoute les colonnes sélectionnées à la colonne **Colonne** du volet Critères.  
  
5.  Dans la colonne **Ajouter** du volet Critères, sélectionnez une colonne cible dans la table de destination pour chaque colonne copiée. Choisissez *nom_table.\&#42;* si vous copiez des lignes entières. Les colonnes de la table de destination doivent avoir des types de données identiques (ou compatibles) à ceux des colonnes de la table source.  
  
6.  Si vous souhaitez copier les lignes dans un ordre précis, spécifiez un ordre de tri. Pour plus d’informations, consultez [Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md).  
  
7.  Spécifiez les lignes à copier en entrant des conditions de recherche dans la colonne **Filtre**. Pour plus d’informations, consultez [Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md).  
  
    Si vous ne spécifiez pas de condition de recherche, toutes les lignes de la table source seront copiées vers la table de destination.  
  
    > [!NOTE]  
    > Lorsque vous ajoutez, dans le volet Critères, une colonne à utiliser dans une condition de recherche, le Concepteur de requêtes et de vues l'ajoute également à la liste des colonnes à copier. Pour utiliser une colonne dans le cadre de la recherche sans toutefois la copier, désactivez la case à cocher en regard du nom de la colonne dans le rectangle représentant la table ou l'objet table.  
  
8.  Si vous souhaitez copier des informations de synthèse, spécifiez des options Group By. Pour plus d’informations, consultez [Résumer les résultats de la requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md).  
  
Quand vous exécutez une requête Insert Results, aucun résultat n’apparaît dans le [volet Résultats](../../ssms/visual-db-tools/results-pane-visual-database-tools.md). En fait, un message indiquant le nombre de lignes copiées s'affiche.  
  
## <a name="see-also"></a> Voir aussi  
[Types de requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/types-of-queries-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
