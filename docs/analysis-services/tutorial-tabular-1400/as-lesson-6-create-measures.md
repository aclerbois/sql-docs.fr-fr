---
title: 'Leçon du didacticiel Analysis Services 6 : créer des mesures | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 61ead234a52f258f2c535f85c0992523b5b4e146
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-measures"></a>Créer des mesures

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous créez des mesures à inclure dans votre modèle. Comme pour les colonnes calculées que vous avez créé, une mesure est un calcul créé à l’aide d’une formule DAX. Toutefois, contrairement aux colonnes calculées, les mesures sont évaluées basé sur un utilisateur sélectionné *filtre*. Par exemple, une colonne particulière ou du segment ajouté dans le champ des étiquettes de ligne dans un tableau croisé dynamique. Une valeur pour chaque cellule dans le filtre est ensuite calculée par la mesure appliquée. Les mesures sont des calculs puissants et flexibles que vous souhaitez inclure dans pratiquement tous les modèles tabulaires pour effectuer des calculs dynamiques sur des données numériques. Pour plus d’informations, consultez [mesures](../tabular-models/measures-ssas-tabular.md).
  
Pour créer des mesures, vous utilisez la *grille de mesures*. Par défaut, chaque table possède une grille de mesures vide ; Toutefois, vous en général, ne créez pas des mesures pour chaque table. La grille de mesures s'affiche sous une table au sein du générateur de modèles dans la vue de données. Pour cacher ou afficher la grille de mesures d'une table, cliquez sur le menu **Table** , puis sur **Afficher la grille de mesures**.  
  
Vous pouvez créer une mesure en cliquant sur une cellule vide dans la grille de mesures, puis en tapant une formule DAX dans la barre de formule. Lorsque vous appuyez sur ENTRÉE pour terminer la formule, la mesure, puis s’affiche dans la cellule. Vous pouvez également créer des mesures à l’aide d’une fonction d’agrégation standard en cliquant sur une colonne, puis en cliquant sur le bouton Somme automatique (**∑**) dans la barre d’outils. Les mesures créées à l’aide de la fonction Somme automatique apparaissent dans la cellule de grille de mesures directement sous la colonne, mais peuvent être déplacés.  
  
Dans cette leçon, vous créez les mesures en entrant une formule DAX dans la barre de formule et à l’aide de la fonction Somme automatique.  
  
Durée estimée pour effectuer cette leçon : **30 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 5 : créer des colonnes calculées](../tutorial-tabular-1400/as-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Créer des mesures  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a>Pour créer une mesure DaysCurrentQuarterToDate dans la table DimDate  
  
1.  Dans le Générateur de modèles, cliquez sur le **DimDate** table.  
  
2.  Dans la grille de mesures, cliquez sur la cellule vide en haut à gauche.  
  
3.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Notez que la cellule en haut à gauche contient maintenant une mesure nommée **DaysCurrentQuarterToDate**, suivi par le résultat, **92**. Le résultat s’applique pas à ce stade, car aucun filtre de l’utilisateur n’a été appliqué.
    
      ![en tant que newmeasure de lesson6](../tutorial-tabular-1400/media/as-lesson6-newmeasure.png) 
    
    Contrairement aux colonnes calculées, des formules de mesure, vous pouvez taper le nom de la mesure, suivi du signe deux-points, suivi par l’expression de formule.

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a>Pour créer une mesure DaysInCurrentQuarter dans la table DimDate  
  
1.  Avec la **DimDate** toujours active dans le Générateur de modèles, dans la grille de mesures, cliquez sur la cellule vide sous la mesure que vous avez créé.  
  
2.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Lorsque vous créez un rapport de comparaison entre une période incomplète et la période précédente. La formule doit calculer la proportion de la période qui s’est écoulé et le compare à la même partie de la période précédente. Dans ce cas, [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] donne la proportion écoulé dans la période actuelle.  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a>Pour créer une mesure InternetDistinctCountSalesOrder dans la table FactInternetSales  
  
1.  Cliquez sur le **FactInternetSales** table.   
  
2.  Cliquez sur le **SalesOrderNumber** en-tête de colonne.  
  
3.  Dans la barre d’outils, cliquez sur la flèche du bas en regard du bouton de somme automatique (**∑**), puis sélectionnez **DistinctCount**.  
  
    La fonction de somme automatique crée automatiquement une mesure pour la colonne sélectionnée à l'aide de la formule de regroupement standard DistinctCount.  
    
       ![en tant que newmeasure2 de lesson6](../tutorial-tabular-1400/media/as-lesson6-newmeasure2.png)
  
4.  Dans la grille de mesures, cliquez sur la nouvelle mesure, puis, dans le **propriétés** fenêtre, dans **nom de la mesure**, renommez la mesure en **InternetDistinctCountSalesOrder**. 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a>Pour créer des mesures supplémentaires dans la table FactInternetSales  
  
1.  En utilisant la fonction de somme automatique, créez et nommez les mesures suivantes :  

    |Colonne|Nom de la mesure|Somme automatique (∑)|Formule|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Compter|=COUNTA([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Sum|=SUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Sum|=SUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Sum|=SUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Sum|=SUM([SalesAmount])|  
    |Margin|InternetTotalMargin|Sum|=SUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Sum|=SUM([TaxAmt])|  
    |Freight|InternetTotalFreight|Sum|=SUM([Freight])|  
  
2.  En cliquant sur une cellule vide dans la grille de mesures et à l’aide de la barre de formule, créer, les mesures personnalisées suivantes dans l’ordre :  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Les mesures créées pour la table FactInternetSales peuvent être utilisées pour analyser des données financières critiques telles que les ventes et les coûts, marge bénéficiaire des éléments définis par le filtre d’utilisateur sélectionné.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

[Leçon 7 : Créer des indicateurs de Performance clés](../tutorial-tabular-1400/as-lesson-7-create-key-performance-indicators.md).  

  
