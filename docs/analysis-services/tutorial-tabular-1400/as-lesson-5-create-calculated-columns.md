---
title: 'Leçon du didacticiel Analysis Services 5 : créer des colonnes calculées | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 476eca07ed1367141372586ca13bd2a93d9d8105
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-calculated-columns"></a>Créer des colonnes calculées

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous créez des données dans votre modèle en ajoutant des colonnes calculées. Vous pouvez créer des colonnes calculées (en tant que colonnes personnalisées) lorsque vous utilisez obtenir des données, à l’aide de l’éditeur de requête, ou plus loin dans le type de Concepteur de modèle vous effectuez ici. Pour plus d’informations, consultez [des colonnes calculées](../tabular-models/ssas-calculated-columns.md).
  
Vous créez cinq nouvelles colonnes calculées dans trois tables différentes. Les étapes sont légèrement différentes pour chaque tâche montrant il existe plusieurs façons de créer des colonnes, les renommer et les placer à différents emplacements dans une table.  

Cette leçon est également où vous utilisez tout d’abord les Expressions DAX (Data Analysis). DAX est un langage spéciaux pour créer des expressions de formule hautement personnalisables pour les modèles tabulaires. Dans ce didacticiel, vous utilisez DAX pour créer des colonnes calculées, les mesures et les filtres de rôle. Pour plus d’informations, consultez [DAX dans les modèles tabulaires](../tabular-models/understanding-dax-in-tabular-models-ssas-tabular.md). 
  
Durée estimée pour effectuer cette leçon : **15 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 4 : créer des relations](../tutorial-tabular-1400/as-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Créer des colonnes calculées  
  
#### <a name="create-a-monthcalendar-calculated-column-in-the-dimdate-table"></a>Créer une colonne calculée MonthCalendar dans la table DimDate  
  
1.  Cliquez sur le **modèle** menu > **vue de modèle** > **vue données**.  
  
    Les colonnes calculées ne peuvent être créées qu'à l'aide du concepteur de modèles dans la vue de données.  
  
2.  Dans le Générateur de modèles, cliquez sur le **DimDate** table (onglet).  
  
3.  Cliquez sur le **CalendarQuarter** en-tête de colonne, puis cliquez sur **insérer une colonne**.  
  
    Une nouvelle colonne nommée **Calculated Column 1** est insérée à gauche de la colonne **Calendar Quarter** .  
  
4.  Dans la barre de formule au-dessus de la table, tapez la formule DAX suivante : la saisie semi-automatique vous aide à taper les noms complets des colonnes et tables et répertorie les fonctions qui sont disponibles.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Des valeurs remplissent ensuite toutes les lignes de la colonne calculée. Si vous faites défiler vers le bas dans la table, vous consultez lignes peuvent avoir des valeurs différentes pour cette colonne, selon les données dans chaque ligne.    
  
5.  Renommer cette colonne **MonthCalendar**. 

    ![en tant que-lesson5-nouvelle colonne](../tutorial-tabular-1400/media/as-lesson5-newcolumn.png) 
  
Le MonthCalendar calculée de colonne qui fournit un nom triable pour le mois.  
  
#### <a name="create-a-dayofweek-calculated-column-in-the-dimdate-table"></a>Créer une colonne calculée DayOfWeek dans la table DimDate  
  
1.  Avec la **DimDate** table toujours active, cliquez sur le **colonne** menu, puis sur **ajouter une colonne**.  
  
2.  Dans la barre de formule, entrez la formule suivante :  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Lorsque vous avez terminé de générer la formule, appuyez sur ENTRÉE. La nouvelle colonne est ajoutée à droite de la table.  
  
3.  Renommer la colonne **DayOfWeek**.  
  
4.  Cliquez sur l’en-tête de colonne, puis faites glisser la colonne entre la **EnglishDayNameOfWeek** colonne et la **DayNumberOfMonth** colonne.  
  
    > [!TIP]  
    > Déplacer les colonnes dans la table facilite la navigation.  
  
L’énumération DayOfWeek calculée de colonne qui fournit un nom triable pour le jour de semaine.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-the-dimproduct-table"></a>Créer une colonne calculée ProductSubcategoryName dans la table DimProduct  
  
  
1.  Dans le **DimProduct** table, faites défiler vers la droite de la table. Notez que la colonne la plus à droite est nommée **Add Column** (en italique), cliquez sur l’en-tête de colonne.  
  
2.  Dans la barre de formule, entrez la formule suivante :  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Renommer la colonne **ProductSubcategoryName**.  
  
La colonne calculée ProductSubcategoryName est utilisée pour créer une hiérarchie dans la table DimProduct, qui inclut des données à partir de la colonne EnglishProductSubcategoryName dans la table DimProductSubcategory. Les hiérarchies ne peuvent pas couvrir plusieurs tables. Pour créer des hiérarchies plus loin dans la leçon 9.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-the-dimproduct-table"></a>Créer une colonne calculée ProductCategoryName dans la table DimProduct  
  
1.  Avec la **DimProduct** table toujours active, cliquez sur le **colonne** menu, puis sur **ajouter une colonne**.  
  
2.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Renommer la colonne **ProductCategoryName**.  
  
La colonne ProductCategoryName est utilisée pour créer une hiérarchie dans la table DimProduct, qui inclut des données à partir de la colonne EnglishProductCategoryName dans la table DimProductCategory. Les hiérarchies ne peuvent pas couvrir plusieurs tables.  
  
#### <a name="create-a-margin-calculated-column-in-the-factinternetsales-table"></a>Créer une colonne calculée de marge dans la table FactInternetSales  
  
1.  Dans le Générateur de modèles, sélectionnez le **FactInternetSales** table.  
  
2.  Créer une nouvelle colonne calculée entre le **SalesAmount** colonne et la **TaxAmt** colonne.  
  
3.  Dans la barre de formule, entrez la formule suivante :  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Renommez la colonne en **Margin**.  
 
      ![en tant que newmargin de lesson5](../tutorial-tabular-1400/media/as-lesson5-newmargin.png)
      
    La colonne calculée Margin est utilisée pour analyser les marges bénéficiaires pour chaque vente.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

[Leçon 6 : Créer des mesures](../tutorial-tabular-1400/as-lesson-6-create-measures.md).
  
  
  
