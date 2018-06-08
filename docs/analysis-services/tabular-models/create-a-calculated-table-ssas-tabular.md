---
title: Créer une Table calculée | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f920d7ef7ae8a8fb5016e4bb4833b3637b325f8a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-calculated-table"></a>Créer une table calculée 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Un *table calculée* est un objet calculé basé sur une requête ou une expression DAX, dérivé de tout ou partie des autres tables dans le même modèle.  
  
 Un problème de conception courant que les tables calculées peuvent résoudre est la présentation d’une dimension de rôle actif dans un contexte spécifique pour vous permettre de l'exposer comme une structure de requête dans les applications clientes.  Vous vous souvenez peut-être qu'une dimension de rôle actif est une simple table présentée dans plusieurs contextes : un exemple classique est la table Date, représentée par OrderDate, ShipDate ou DueDate, en fonction de la relation de clé étrangère. En créant explicitement une table calculée pour ShipDate, vous obtenez une table autonome et disponible pour les requêtes, et totalement opérationnelle comme toute autre table.  
  
 Une deuxième utilisation d’une table calculée est la configuration d’un ensemble de lignes filtré ou d’un sous-ensemble ou sur-ensemble de colonnes provenant d’autres tables existantes. Cela vous permet de conserver intacte la table d’origine, tout en créant des variations de cette table pour prendre en charge des scénarios spécifiques.  
  
 Pour tirer le meilleur parti des tables calculées, vous devez connaître quelques expressions DAX. Lorsque vous utilisez des expressions pour votre table, il peut être utile de savoir qu'une table calculée contient une partition unique avec un élément DAXSource, où l'expression est une expression DAX.  
Il existe un élément CalculatedTableColumn pour chaque colonne renvoyée par l'expression, où SourceColumn est le nom de la colonne retournée (identique à celui de DataColumns sur les tables non calculées).  
  
## <a name="how-to-create-a-calculated-table"></a>Comment créer une table calculée  
  
1.  Vérifiez d’abord que le modèle tabulaire affiche un niveau de compatibilité supérieur ou égal à 1200. Vous pouvez vérifier la propriété **Niveau de compatibilité** sur le modèle dans SSDT.  
  
2.  Basculez en mode Données. Vous ne pouvez pas créer une table calculée dans la vue de diagramme.  
  
3.  Sélectionnez **Table** > **Nouvelle table calculée**.  
  
4.  Tapez ou collez une expression DAX (voir ci-dessous pour quelques suggestions).  
  
5.  Nommez la table.  
  
6.  Créez des relations vers d'autres tables dans le modèle. Consultez [créer une relation entre deux Tables](../../analysis-services/tabular-models/create-a-relationship-between-two-tables-ssas-tabular.md) si vous avez besoin d’aide pour cette étape.  
  
7.  Référencez la table dans les calculs ou expressions de votre modèle, ou utilisez **Analyser dans Excel** pour une exploration des données ad hoc.  
  
### <a name="replicate-a-role-playing-dimension"></a>Répliquer une dimension de rôle actif  
 Dans la barre de formule, entrez une formule DAX qui obtient une copie d’une autre table. Après avoir rempli la table calculée, attribuez-lui un nom descriptif puis définissez une relation qui utilise la clé étrangère spécifique au rôle. Par exemple, dans la base de données Adventure Works, vous pouvez créer une table calculée pour la date d'échéance et utiliser la valeur DueDateKey comme base d'une relation vers la table de faits.  
  
```  
=DimDate  
```  
  
### <a name="summarized-or-filtered-dataset"></a>Jeu de données synthétisé ou filtré  
 Dans la barre de formule, entrez une expression DAX afin de filtrer, résumer ou manipuler un jeu de données pour contenir les lignes souhaitées. Cet exemple regroupe les données par ventes, par couleur et par devise.  
  
```  
=SUMMARIZECOLUMNS(DimProduct[Color]  
, DimCurrency[CurrencyName]   
, "Sales" , SUM(FactInternetSales[SalesAmount])  
)  
```  
  
### <a name="superset-using-columns-from-multiple-tables"></a>Sur-ensemble utilisant des colonnes de plusieurs tables  
 Dans la barre de formule, entrez une expression DAX qui combine des colonnes provenant de plusieurs tables. Dans ce cas, le résultat de la requête affiche la catégorie de produit pour chaque devise.  
  
```  
=CROSSJOIN(DimProductCategory, DimCurrency)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Niveau de compatibilité](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)   
 [Data Analysis Expressions &#40;DAX&#41; dans Analysis Services](http://msdn.microsoft.com/library/abb336c9-3346-4cab-b91b-90f93f4575e5)   
 [Présentation de DAX dans les modèles tabulaires](../../analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular.md)  
  
  
