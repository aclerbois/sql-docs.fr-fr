---
title: 'Leçon 7 : Créer des mesures | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 146d93bc2c7257ce409f3a293f6c9050acde9ca7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-6-create-measures"></a>Leçon 6 : Créer des mesures
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon, vous allez créer des mesures à inclure dans votre modèle. Comme pour les colonnes calculées que vous avez créé dans la leçon précédente, une mesure est un calcul créé à l’aide d’une formule DAX. Toutefois, contrairement aux colonnes calculées, les mesures sont évaluées en fonction d'un *filtre*sélectionné par l'utilisateur ; par exemple, une colonne ou un segment particulier ajouté au champ des étiquettes de ligne dans un tableau croisé dynamique. Une valeur pour chaque cellule dans le filtre est ensuite calculée par la mesure appliquée. Les mesures sont des calculs puissants et flexibles que vous pouvez inclure dans pratiquement tous les modèles tabulaires pour effectuer des calculs dynamiques sur des données numériques. Pour plus d’informations, consultez [mesures](../analysis-services/tabular-models/measures-ssas-tabular.md).  
  
Pour créer des mesures, vous allez utiliser le *grille de mesures*. Par défaut, chaque table contient une grille de mesures vide ; toutefois, en général, vous ne créez pas de mesures pour chaque table. La grille de mesures s'affiche sous une table au sein du générateur de modèles dans la vue de données. Pour cacher ou afficher la grille de mesures d'une table, cliquez sur le menu **Table** , puis sur **Afficher la grille de mesures**.  
  
Vous pouvez créer une mesure en cliquant sur une cellule vide dans la grille de mesures, puis en tapant une formule DAX dans la barre de formule. Lorsque vous cliquez sur ENTRÉE pour terminer la formule, la mesure apparaît dans la cellule. Vous pouvez également créer des mesures à l’aide d’une fonction d’agrégation standard en cliquant sur une colonne, puis en cliquant sur le bouton de somme automatique (**∑**) dans la barre d’outils. Les mesures créées à l’aide de la fonction Somme automatique seront affiche dans la cellule de grille de mesures directement sous la colonne, mais peuvent être déplacés.  
  
Dans cette leçon, vous allez créer des mesures en entrant une formule DAX dans la barre de formule et en utilisant la fonction de somme automatique.  
  
Durée estimée pour effectuer cette leçon : **30 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 5 : créer des colonnes calculées](../analysis-services/lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Créer des mesures  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a>Pour créer une mesure DaysCurrentQuarterToDate dans la table DimDate  
  
1.  Dans le Générateur de modèles, cliquez sur le **DimDate** table.  
  
2.  Dans la grille de mesures, cliquez sur la cellule vide en haut à gauche.  
  
3.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Notez que la cellule en haut à gauche contient maintenant une mesure nommée **DaysCurrentQuarterToDate**, suivi par le résultat, **92**.
    
      ![en tant que-tabulaire-lesson6-newmeasure](../analysis-services/media/as-tabular-lesson6-newmeasure.png) 
    
    Contrairement aux colonnes calculées, des formules de mesure, vous pouvez taper le nom de mesure, suivi par une virgule, suivie de l’expression de la formule.

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a>Pour créer une mesure DaysInCurrentQuarter dans la table DimDate  
  
1.  Avec la **DimDate** toujours active dans le Générateur de modèles, dans la grille de mesures, cliquez sur la cellule vide sous la mesure que vous venez de créer.  
  
2.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Lorsque vous créez un ratio de comparaison entre une période incomplète et la période précédente, la formule doit prendre en compte la partie de la période qui s'est écoulée, et la comparer à la même partie de la période précédente. Dans ce cas, [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] donne la proportion écoulé dans la période actuelle.  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a>Pour créer une mesure InternetDistinctCountSalesOrder dans la table FactInternetSales  
  
1.  Cliquez sur le **FactInternetSales** table.   
  
2.  Cliquez sur le **SalesOrderNumber** en-tête de colonne.  
  
3.  Dans la barre d’outils, cliquez sur la flèche du bas en regard du bouton de somme automatique (**∑**), puis sélectionnez **DistinctCount**.  
  
    La fonction de somme automatique crée automatiquement une mesure pour la colonne sélectionnée à l'aide de la formule de regroupement standard DistinctCount.  
    
       ![en tant que-tabulaire-lesson6-newmeasure2](../analysis-services/media/as-tabular-lesson6-newmeasure2.png)
  
4.  Dans la grille de mesures, cliquez sur la nouvelle mesure, puis, dans le **propriétés** fenêtre, dans **nom de la mesure**, renommez la mesure en **InternetDistinctCountSalesOrder**. 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a>Pour créer des mesures supplémentaires dans la table FactInternetSales  
  
1.  En utilisant la fonction de somme automatique, créez et nommez les mesures suivantes :  
  
    |Nom de la mesure|Colonne|Somme automatique (∑)|Formule|  
    |----------------|----------|-----------------|-----------|  
    |InternetOrderLinesCount|SalesOrderLineNumber|Compter|=COUNTA([SalesOrderLineNumber])|  
    |InternetTotalUnits|OrderQuantity|Sum|=SUM([OrderQuantity])|  
    |InternetTotalDiscountAmount|DiscountAmount|Sum|=SUM([DiscountAmount])|  
    |InternetTotalProductCost|TotalProductCost|Sum|=SUM([TotalProductCost])|  
    |InternetTotalSales|SalesAmount|Sum|=SUM([SalesAmount])|  
    |InternetTotalMargin|Margin|Sum|=SUM([Margin])|  
    |InternetTotalTaxAmt|TaxAmt|Sum|=SUM([TaxAmt])|  
    |InternetTotalFreight|Freight|Sum|=SUM([Freight])|  
  
2.  En cliquant sur une cellule vide dans la grille de mesures et à l’aide de la barre de formule, créez et nommez les mesures suivantes dans l’ordre :  
  
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
Accédez à la leçon suivante : [leçon 7 : créer des indicateurs de Performance clés](../analysis-services/lesson-7-create-key-performance-indicators.md).  

  
