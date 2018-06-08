---
title: Colonnes calculées | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3a79910d324a1e0c157a638745ad96a4bfff800e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="calculated-columns"></a>Colonnes calculées
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Dans les modèles tabulaires, les colonnes calculées permettent d’ajouter de nouvelles données à votre modèle. Au lieu de coller ou importer des valeurs dans la colonne, vous créez une formule DAX qui définit les valeurs de niveau de ligne de la colonne. La colonne calculée peut alors être utilisée dans un rapport, un tableau croisé dynamique ou un graphique croisé dynamique comme toute autre colonne.  
 
  
  
##  <a name="bkmk_understanding"></a> Avantages  
 Les formules dans les colonnes calculées ressemblent aux formules dans Excel. Toutefois, contrairement à Excel, vous ne pouvez pas créer des formules différentes pour des lignes distinctes de la table ; la formule DAX est appliquée automatiquement à la colonne entière.  
  
 Lorsqu'une colonne contient une formule, la valeur est calculée pour chaque ligne. Les résultats sont calculés pour la colonne lorsque vous entrez une formule valide. Les valeurs de colonne sont ensuite recalculées si nécessaire, par exemple lorsque les données sous-jacentes sont actualisées.  
  
 Vous pouvez également créer des colonnes calculées reposant sur des mesures ou sur d'autres colonnes calculées. Par exemple, vous pouvez créer une colonne calculée pour extraire un nombre d'une chaîne de texte, puis utiliser ce nombre dans une autre colonne calculée.  
  
 Une colonne calculée est basée sur les données qui figurent déjà dans une table existante, ou créées à l'aide d'une formule DAX. Par exemple, vous pouvez choisir de concaténer des valeurs, effectuer une addition, extraire des sous-chaînes ou comparer les valeurs dans d'autres champs. Pour ajouter une colonne calculée, vous devez déjà disposer d'au moins une table dans votre modèle.  
  
 Cet exemple illustre une formule simple dans une colonne calculée :  
  
```  
=EOMONTH([StartDate],0])  
  
```  
  
 Cette formule extrait le mois de la colonne StartDate. Elle calcule ensuite la valeur de fin du mois pour chaque ligne de la table. Le deuxième paramètre spécifie le nombre de mois avant ou après le mois indiqué dans StartDate ; dans cette cas, 0 signifie le même mois. Par exemple, si la valeur de la colonne StartDate est 1/6/2001, la valeur dans la colonne calculée sera 30/6/2001.  
  
##  <a name="bkmk_naming"></a> Naming a calculated column  
 Par défaut, les nouvelles colonnes calculées sont ajoutées à droite des autres colonnes d'une table, et le nom par défaut **CalculatedColumn1**, **CalculatedColumn2**, etc. est automatiquement affecté à la colonne. Vous pouvez également cliquer avec le bouton droit sur une colonne, puis sélectionner Insérer une colonne pour créer une nouvelle colonne entre deux colonnes existantes. Vous pouvez réorganiser les colonnes dans la même table en cliquant et en faisant glisser, et vous pouvez renommer des colonnes une fois créées ; toutefois, tenez compte des restrictions suivantes relatives aux modifications apportées aux colonnes calculées :  
  
-   Chaque nom de colonne doit être unique à l'intérieur d'une table.  
  
-   Évitez les noms qui ont été déjà utilisés pour les mesures dans le même modèle. Bien qu'il soit possible qu'une colonne calculée et une mesure portent le même nom, des erreurs de calcul peuvent survenir si les noms ne sont pas uniques. Pour éviter d'appeler une mesure par inadvertance, utilisez toujours une référence de colonne complète lorsque vous faites référence à une colonne.  
  
-   Lorsque vous renommez une colonne calculée, toutes les formules qui s'appuient sur la colonne doivent être mises à jour manuellement. À moins que vous ne soyez en mode de mise à jour manuel, la mise à jour des résultats des formules a lieu automatiquement. Toutefois, cette opération peut prendre quelque temps.  
  
-   Certains caractères ne peuvent pas être utilisés dans les noms de colonnes. Pour plus d’informations, consultez « Exigences concernant l’affectation des noms » dans [Référence syntaxique de DAX](http://msdn.microsoft.com/en-us/098630f4-7d1d-467e-976c-99b2279430d5).  
  
##  <a name="bkmk_perf"></a> Performance of calculated columns  
 La formule d'une colonne calculée peut solliciter beaucoup plus de ressources que la formule utilisée pour une mesure. L'une des raisons en est que le résultat d'une colonne calculée est toujours calculé pour chaque ligne d'une table, alors qu'une mesure est calculée uniquement pour les cellules définies par le filtre utilisé dans le rapport, le tableau croisé dynamique ou le graphique croisé dynamique. Par exemple, une table d'un million de lignes aura toujours une colonne calculée avec un million de résultats et les conséquences que cela implique en termes de performances. Toutefois, un tableau croisé dynamique filtre généralement les données en appliquant des titres de lignes et de colonnes ; par conséquent, une mesure est calculée uniquement pour le sous-ensemble de données dans chaque cellule du tableau croisé dynamique.  
  
 Une formule présente des dépendances vis-à-vis des objets auxquels elle fait référence, tels que d'autres colonnes ou expressions qui évaluent des valeurs. Par exemple, une colonne calculée qui est basée sur une autre colonne, ou un calcul qui contient une expression avec une référence de colonne, ne peuvent pas être évalués tant que l'autre colonne n'a pas été évaluée. Par défaut, l'actualisation automatique est activée dans les classeurs, ce qui signifie que de telles dépendances peuvent affecter les performances lorsque les valeurs sont mises à jour et les formules actualisées.  
  
 Pour éviter les problèmes de performances lorsque vous créez des colonnes calculées, suivez les instructions ci-après :  
  
-   Plutôt que de créer une formule individuelle qui contient de nombreuses dépendances complexes, créez les formules par étapes, en enregistrant les résultats dans les colonnes, afin de pouvoir valider les résultats et évaluer les performances.  
  
-   La modification de données requiert souvent un nouveau calcul des colonnes calculées. Vous pouvez éviter cela en optant pour le mode de recalcul manuel. Toutefois, si des valeurs de la colonne calculée sont erronées, la colonne sera grisée tant que vous n'aurez pas actualisé et recalculé les données.  
  
-   Si vous modifiez ou supprimez des relations entre des tables, les formules qui utilisent des colonnes de ces tables deviennent non valides.  
  
-   Si vous créez une formule qui contient une dépendance circulaire ou faisant référence à elle-même, une erreur se produit.  
  
##  <a name="bkmk_rel_tasks"></a> Related tasks  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer une colonne calculée](../../analysis-services/tabular-models/ssas-calculated-columns-create-a-calculated-column.md)|Les tâches de cette rubrique décrivent comment ajouter une nouvelle colonne calculée dans une table.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et colonnes](../../analysis-services/tabular-models/tables-and-columns-ssas-tabular.md)   
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)   
 [Calculs](../../analysis-services/tabular-models/calculations-ssas-tabular.md)  
  
  
