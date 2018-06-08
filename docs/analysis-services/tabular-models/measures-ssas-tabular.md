---
title: Mesures | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cbd53d9d49c937967e88cefa95750dca41631876
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="measures"></a>Mesures
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Dans les modèles tabulaires, une mesure est un calcul créé à l'aide d'une formule DAX pour une utilisation dans un client de création de rapport. Les mesures sont évaluées en fonction des champs, des filtres, et des segments que les utilisateurs choisissent dans l'application cliente de création de rapports.  
  
##  <a name="bkmk_understanding"></a> Avantages  
 Les mesures peuvent reposer sur des fonctions d'agrégation standard, comme AVERAGE, COUNT, ou SUM, ou vous pouvez définir votre propre formule à l'aide de DAX. Outre la formule, chaque mesure a des propriétés, définies par le type de données de mesure, tel que le nom, le détail de table, le format et les décimales.  
  
 Lorsque les mesures ont été définies dans un modèle, les utilisateurs peuvent ensuite les ajouter à un rapport ou un tableau croisé dynamique. En fonction des perspectives et des rôles, les mesures s'affichent dans la liste de champs avec la table associée, et elles sont disponibles pour tous les utilisateurs du modèle. Les mesures sont généralement créées dans des tables de faits ; toutefois, elles peuvent être indépendantes de la table à laquelle elles sont associées.  
  
 Il est important de comprendre les différences fondamentales entre une colonne calculée et une mesure. Dans une colonne calculée, une formule prend une valeur pour chaque ligne de la colonne. Par exemple, dans une table FactSales, une colonne calculée nommée TotalProfit avec la formule suivante calcule une valeur pour le bénéfice total de chaque ligne (une ligne par vente) dans la table FactSales :  
  
```  
=[SalesAmount] - [TotalCost] - [ReturnAmount]  
```  
  
 La colonne calculée TotalProfit peut ensuite être utilisée dans un client de création de rapports comme n'importe quelle autre colonne.  
  
 Une mesure, en revanche, prend une valeur selon une sélection de l'utilisateur ; un contexte de filtre défini dans un tableau croisé dynamique ou un rapport. Par exemple, une mesure dans la table FactSales est créée avec la formule suivante :  
  
```  
Sum of TotalProfit: =SUM([TotalProfit])  
```  
  
 Un analyste des ventes souhaite, à l'aide d'Excel, connaître le bénéfice total pour une catégorie de produits. Chaque catégorie de produits est composée de plusieurs produits. L'analyste des ventes sélectionne la colonne ProductCategoryName et l'ajoute à la fenêtre de filtre d'étiquettes de ligne dans un tableau croisé dynamique ; une ligne pour chaque catégorie de produits est alors affichée dans le tableau croisé dynamique. L'utilisateur sélectionne ensuite la somme de la mesure TotalProfit. Une mesure est par défaut ajoutée dans la fenêtre de filtre de valeurs. La mesure calcule la somme du bénéfice total et affiche les résultats pour chaque catégorie de produits. L'analyste des ventes peut ensuite affiner le filtrage de la somme du bénéfice total pour chaque catégorie de produits à l'aide d'un segment, par exemple, en ajoutant CalendarYear comme segment pour afficher la somme du bénéfice total pour chaque catégorie de produits par année.  
  
|ProductCategoryName|Somme de TotalProfit|  
|-------------------------|------------------------|  
|Audio|$2,731,061,308.69|  
|Appareils photos et caméscopes|$620,623,675.75|  
|Ordinateurs|$392,999,044.59|  
|TV et vidéo|$946,989,702.51|  
|**Total général**|**$4,691,673,731.53**|  
  
##  <a name="bkmk_def_mg"></a> Defining measures by using the measure grid  
 Les mesures sont créées au moment de la conception à l'aide de la grille de mesures dans le concepteur de modèles. Chaque table a une grille de mesures. Par défaut, la grille de mesures s'affiche sous chaque table dans le concepteur de modèles. Vous pouvez également choisir de ne pas afficher la grille de mesures pour une table particulière. Pour désactiver l’affichage de la grille de mesures d’une table, cliquez sur le menu **Table** , puis sur **Afficher la grille de mesures**.  
  
 Dans la grille de mesures, vous pouvez créer des mesures des manières suivantes :  
  
-   Cliquez sur une cellule vide de la grille de mesures, puis tapez une formule DAX dans la barre de formule. Lorsque vous cliquez sur Entrée pour terminer la formule, la mesure apparaît dans la cellule de la grille de mesures.  
  
-   Créez une mesure à l'aide d'une fonction d'agrégation standard en cliquant sur une colonne, puis en cliquant sur le bouton Somme automatique (∑) dans la barre d'outils, puis sur une fonction d'agrégation standard. Les agrégations standard sont : Sum, Average, Count, DistinctCount, Max, Min. Les mesures créées à l'aide du bouton Somme automatique apparaissent toujours dans la grille de mesures, directement sous la colonne.  
  
 Par défaut, lors de l'utilisation de Somme automatique, le nom de la mesure est défini par le nom de la colonne associée, suivi de deux-points et de la formule. Vous pouvez modifier le nom dans la barre de formule ou dans le paramètre de propriété **Nom de la mesure** dans la fenêtre Propriétés. Lors de la création d’une mesure à l’aide d’une formule personnalisée, vous pouvez taper un nom dans la barre de formule, suivi de deux-points, puis de la formule, ou vous pouvez taper un nom dans le paramètre de propriété **Nom de la mesure** dans la fenêtre Propriétés.  
  
 Il est important de nommer les mesures avec précaution. Le nom de la mesure apparaît avec la table associée dans la liste des champs du client de création de rapports. Un indicateur de performance clé sera également nommé selon la mesure de base. Une mesure ne peut pas avoir le même nom qu'une colonne dans une table du modèle.  
  
> [!TIP]  
>  Vous pouvez regrouper des mesures de plusieurs tables au sein d'une seule et même table. Pour cela, créez une table vite, puis déplacez des mesures vers celle-ci ou créez-en de nouvelles. Gardez à l'esprit que vous devrez peut-être inclure des noms de table dans des formules DAX lorsque vous ferez référence à des colonnes dans d'autres tables.  
  
 Si des perspectives ont été définies pour le modèle, les mesures ne sont pas ajoutées automatiquement à chacune d'entre elles. Vous devez ajouter manuellement les mesures à une perspective à l'aide de la boîte de dialogue Perspectives. Pour plus d’informations, consultez [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md).  
  
##  <a name="bkmk_properties"></a> Propriétés des mesures  
 Chaque mesure a des propriétés qui la caractérisent. Les propriétés de mesures, ainsi que les propriétés des colonnes associées, peuvent être modifiées dans la fenêtre Propriétés. Les mesures ont les propriétés suivantes :  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|**Description**|Vide|Description de la mesure. La description n'apparaîtra pas avec la mesure dans un client de création de rapports.|  
|**Format**|Déterminé automatiquement à partir du type de données de la colonne référencée dans l'expression de formule.|Format de la mesure. Par exemple, devise ou pourcentage.|  
|**Formule**|La formule est entrée dans la barre de formule lors de la création de la mesure.|Formule de la mesure.|  
|**Nom de la mesure**|Si Somme automatique est utilisé, le nom de la mesure précédera le nom de colonne suivi de deux-points. Si une formule personnalisée est entrée, tapez un nom suivi de deux-points, puis tapez la formule.|Nom de la mesure tel qu'il s'affiche dans une liste de champs de client de création de rapports.|  
  
##  <a name="bkmk_KPI"></a> Utilisation d’une mesure dans un indicateur de performance clé  
 Un indicateur de performance clé (KPI) est défini par une valeur de *base* , explicitée par une mesure, par rapport à une valeur *cible* , également définie par une mesure ou par une valeur absolue. Un indicateur de performance clé inclut également *l’état*, un calcul où la valeur de base correspond à la valeur cible entre les seuils, comme indiqué dans le format graphique. Les indicateurs sont souvent utilisés par les professionnels pour identifier les tendances des métriques commerciales critiques.  
  
 Toute mesure peut servir de mesure de base d'un indicateur de performance clé. Pour créer un KPI, dans la grille de mesures, cliquez avec le bouton droit sur une mesure, puis sélectionnez **Créer un KPI**. La boîte de dialogue Indicateur de performance clé apparaît. Vous pouvez y spécifier une valeur cible (définie par une mesure ou une valeur absolue) et définir des seuils d'état et un type graphique. Pour plus d’informations, consultez [indicateurs de performance clés](../../analysis-services/tabular-models/kpis-ssas-tabular.md).  
  
##  <a name="bkmk_rel_tasks"></a> Related tasks  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer et gérer des mesures](../../analysis-services/tabular-models/create-and-manage-measures-ssas-tabular.md)|Décrit comment créer et gérer des mesures à l'aide de la grille de mesures dans le générateur de modèles.|  
  
## <a name="see-also"></a>Voir aussi  
 [Indicateurs de performance clés](../../analysis-services/tabular-models/kpis-ssas-tabular.md)   
 [Créer et gérer des indicateurs de performance clés](../../analysis-services/tabular-models/create-and-manage-kpis-ssas-tabular.md)   
 [Colonnes calculées](../../analysis-services/tabular-models/ssas-calculated-columns.md)  
  
  
