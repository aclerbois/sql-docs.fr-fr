---
title: Créer et gérer des mesures | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f6547cca48cbb846901e12cd1ab38822de9133d3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-measures"></a>Créer et gérer des mesures 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Une mesure est une formule créée pour être utilisée dans un rapport ou un tableau croisé dynamique (ou un graphique croisé dynamique) Excel. Les mesures peuvent reposer sur des fonctions d'agrégation standard, comme COUNT ou SUM, ou vous pouvez définir votre propre formule à l'aide de DAX. Les tâches de cette rubrique décrivent comment créer et gérer des mesures à l'aide de la grille de mesures d'une table.  
  
## <a name="tasks"></a>Tâches  
 Pour créer et gérer des mesures, vous utiliserez la grille de mesures d'une table. Vous pouvez afficher la grille de mesures pour une table dans le générateur de modèles dans la vue de données uniquement. Vous ne pouvez pas créer de mesures ni afficher la grille de mesures dans la vue de diagramme ; toutefois, vous pouvez afficher des mesures existantes dans la vue de diagramme. Pour afficher la grille de mesures d'une table, cliquez sur le menu **Table** , puis sur **Afficher la grille de mesures**.  
  
###  <a name="bkmk_create_stand"></a> Pour créer une mesure qui utilise une formule d'agrégation standard  
  
-   Cliquez sur la colonne pour laquelle vous souhaitez créer la mesure, puis dans le menu **Colonne** , pointez sur **Somme automatique**, puis cliquez sur un type d'agrégation.  
  
     La mesure est créée automatiquement avec un nom par défaut, suivi de la formule dans la première cellule de la grille de mesures directement sous la colonne.  
  
###  <a name="bkmk_create_custom"></a> Pour créer une mesure qui utilise une formule personnalisée  
  
-   Dans la grille de mesures, sous la colonne pour laquelle vous souhaitez créer la mesure, cliquez sur une cellule, puis dans la barre de formule, tapez un nom, suivi de deux-points (:), d’un signe égal (=) et de la formule. Appuyez sur Entrée pour accepter la formule.  
  
###  <a name="bkmk_edit"></a> Pour modifier les propriétés de mesure  
  
-   Dans la grille de mesures, cliquez sur une mesure, puis dans la fenêtre **Propriétés** , tapez ou sélectionnez une valeur de propriété différente.  
  
###  <a name="bkmk_rename"></a> Pour renommer une mesure  
  
-   Dans la grille de mesures, cliquez sur une mesure, puis dans la fenêtre **Propriétés** , dans **Nom de la mesure**, tapez un nouveau nom et cliquez sur ENTRÉE.  
  
     Vous pouvez également renommer une mesure dans la barre de formule. Le nom de la mesure précède la formule, suivi de deux-points.  
  
###  <a name="bkmk_delete"></a> Pour supprimer une mesure  
  
-   Dans la grille de mesures, cliquez avec le bouton droit sur une mesure, puis sélectionnez **Supprimer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)   
 [Indicateurs de performance clés](../../analysis-services/tabular-models/kpis-ssas-tabular.md)   
 [Colonnes calculées](../../analysis-services/tabular-models/ssas-calculated-columns.md)  
  
  
