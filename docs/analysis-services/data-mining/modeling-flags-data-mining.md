---
title: Indicateurs de modélisation (exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 32a7241fcea41af44e3e336d02c857f51d133208
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="modeling-flags-data-mining"></a>Indicateurs de modélisation (Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez utiliser des indicateurs de modélisation dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour fournir à un algorithme d’exploration de données des informations supplémentaires portant sur les données définies dans une table de cas. Ces informations permettent à l'algorithme de construire un modèle d'exploration de données plus précis.  
  
 Certains indicateurs de modélisation sont définis au niveau de la structure d'exploration de données, tandis que d'autres sont définis au niveau de la colonne du modèle d'exploration de données. Par exemple, l’indicateur de modélisation **NOT NULL** est utilisé avec les colonnes de structure d’exploration de données. Vous pouvez définir des indicateurs de modélisation supplémentaires sur les colonnes du modèle d'exploration de données, en fonction de l'algorithme que vous utilisez pour créer le modèle.  
  
> [!NOTE]  
>  Les plug-ins tiers peuvent posséder d'autres indicateurs de modélisation, en plus de ceux prédéfinis par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="list-of-modeling-flags"></a>Liste des indicateurs de modélisation  
 La liste suivante décrit les indicateurs de modélisation pris en charge dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d'informations sur les indicateurs de modélisation pris en charge par des algorithmes spécifiques, consultez la rubrique de référence technique relative à l'algorithme qui a été utilisé pour créer le modèle.  
  
 **NOT NULL**  
 Indique que les valeurs de la colonne d'attribut ne doivent jamais contenir de valeur NULL. Une erreur est générée si [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] rencontre une valeur NULL pour la colonne d’attribut au cours du processus d’apprentissage du modèle.  
  
 **MODEL_EXISTENCE_ONLY**  
 Indique que la colonne sera considérée comme ayant deux états : **Missing** (manquant) et **Existing**(existant). Si la valeur est **NULL**, elle est considérée comme manquante (Missing). L'indicateur MODEL_EXISTENCE_ONLY est appliqué à l'attribut prédictible et est pris en charge par la plupart des algorithmes.  
  
 En effet, l’affectation de la valeur **True** à l’indicateur MODEL_EXISTENCE_ONLY modifie la représentation des valeurs pour qu’il n’y ait que deux états : **Missing** et **Existing**. Tous les états autres que Missing sont combinés dans une valeur **Existing** unique.  
  
 En règle générale, cet indicateur de modélisation est utilisé dans des attributs pour lesquels l’état **NULL** a une signification implicite. En d’autres termes, le fait que la colonne ait une valeur peut revêtir plus d’importance que la valeur explicite de l’état **NOT NULL** . Par exemple, une colonne [DateContractSigned] peut être **NULL** si un contrat n’a jamais été signé et **NOT NULL** si le contrat a été signé. Ainsi, si le but du modèle est de prédire si un contrat doit être signé, vous pouvez utiliser l’indicateur MODEL_EXISTENCE_ONLY pour ignorer la valeur de date exacte dans les cas **NOT NULL** et faire uniquement la distinction entre les cas où un contrat est **Missing** et ceux où la valeur est **Existing**.  
  
> [!NOTE]  
>  L'état Missing est un état spécial utilisé par l'algorithme qui diffère de la valeur de texte « Manquant » dans une colonne. Pour plus d’informations, consultez [Valeurs manquantes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md).  
  
 **RÉGRESSEUR**  
 Indique que la colonne est candidate pour être utilisée comme régresseur lors du traitement. Cet indicateur est défini sur une colonne de modèle d'exploration de données et ne peut être appliqué qu'à des colonnes dont le type de données numériques continues. Pour plus d’informations sur l’utilisation de cet indicateur, consultez la section [Utilisations de l’indicateur de modélisation REGRESSOR](#bkmk_UseRegressors)plus loin dans cette rubrique.  
  
## <a name="viewing-and-changing-modeling-flags"></a>Affichage et modification d'indicateurs de modélisation  
 Vous pouvez afficher les indicateurs de modélisation associés à une colonne de la structure d'exploration de données ou à une colonne du modèle en affichant les propriétés de la structure ou du modèle.  
  
 Pour déterminer les indicateurs de modélisation ont été appliqués à la structure d'exploration de données actuelle, vous pouvez créer une requête sur l'ensemble de lignes de schéma d'exploration de données qui retourne les indicateurs de modélisation uniquement pour les colonnes de la structure, en utilisant une requête semblable à la suivante :  
  
```  
SELECT COLUMN_NAME, MODELING_FLAG  
FROM $system.DMSCHEMA_MINING_STRUCTURE_COLUMNS  
WHERE STRUCTURE_NAME = '<structure name>'  
```  
  
 Vous pouvez ajouter ou modifier les indicateurs de modélisation utilisés dans un modèle en utilisant le Concepteur d'exploration de données et en modifiant les propriétés des colonnes associées. De telles modifications requièrent que la structure ou le modèle soient à nouveau traités.  
  
 Vous pouvez spécifier des indicateurs de modélisation dans une nouvelle structure d'exploration de données ou un nouveau modèle d'exploration de données à l'aide de DMX, ou à l'aide de scripts AMO ou XMLA. Cependant, vous ne pouvez pas utiliser DMX pour modifier les indicateurs de modélisation utilisés dans un modèle et une structure d'exploration de données existants. Vous devez créer un modèle d’exploration de données en utilisant la syntaxe `ALTER MINING STRUCTURE….ADD MINING MODEL`.  
  
##  <a name="bkmk_UseRegressors"></a> Utilisations de l’indicateur de modélisation REGRESSOR  
 Lorsque vous définissez l'indicateur de modélisation REGRESSOR sur une colonne, vous indiquez à l'algorithme que la colonne contient des régresseurs potentiels. Les régresseurs actuellement utilisés dans le modèle sont déterminés par l'algorithme. Un régresseur potentiel peut être ignoré s'il ne modélise pas l'attribut prédictible.  
  
 Lorsque vous générez un modèle à l'aide de l'Assistant Exploration de données, toutes les colonnes d'entrée continues sont marquées comme des régresseurs possibles. Par conséquent, même si vous ne définissez pas explicitement l'indicateur REGRESSOR sur une colonne, la colonne peut être utilisée comme régresseur dans le modèle.  
  
 Vous pouvez déterminer les régresseurs utilisés dans le modèle traité en exécutant une requête sur l'ensemble de lignes de schéma pour le modèle d'exploration de données, comme le montre l'exemple suivant :  
  
```  
SELECT COLUMN_NAME, MODELING_FLAG  
FROM $system.DMSCHEMA_MINING_COLUMNS  
WHERE MODEL_NAME = '<model name>'  
```  
  
 **Remarque** Si vous modifiez un modèle d’exploration de données et que vous remplacez le type de contenu continu d’une colonne par un type de contenu discret, vous devez modifier manuellement l’indicateur sur la colonne d’exploration de données, puis retraiter le modèle.  
  
### <a name="regressors-in-linear-regression-models"></a>Régresseurs dans les modèles de régression linéaire  
 Les modèles de régression linéaire sont basés sur l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees). Même si vous n’utilisez pas l’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression), tout modèle d’arbre de décision peut contenir un arbre ou des nœuds qui représentent une régression sur un attribut continu.  
  
 Par conséquent, dans ces modèles, il est inutile de spécifier qu'une colonne continue représente un régresseur. L’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) partitionne le dataset en régions avec des séquences explicites même si vous ne définissez pas l’indicateur REGRESSOR sur la colonne. La différence réside dans le fait que lorsque vous définissez l'indicateur de modélisation, l'algorithme essaie de rechercher des équations de régression se présentant sous la forme suivante pour faire tenir les séquences dans les nœuds de l'arbre.  
  
 a*C1 + b\*C2 + ...  
  
 La somme des résiduels est alors calculée et, si l'écart est trop grand, l'arbre est fractionné.  
  
 Par exemple, si vous prédisez le comportement d’achat de vos clients en utilisant **Income** comme attribut et que vous définissez l’indicateur de modélisation REGRESSOR sur la colonne, l’algorithme essaie tout d’abord de faire tenir les valeurs **Income** en utilisant une formule de régression standard. Si l'écart est trop grand, la formule de régression est abandonnée et l'arbre est fractionné sur un autre attribut. L'algorithme MDT essaie ensuite de faire tenir un régresseur pour le revenu dans chacune des branches après le fractionnement.  
  
 Vous pouvez utiliser le paramètre FORCE_REGRESSOR pour faire en sorte que l'algorithme utilise un régresseur particulier. Ce paramètre peut être utilisé avec l’algorithme d’arbres de décision et l’algorithme de régression linéaire.  
  
## <a name="related-tasks"></a>Tâches associées  
 Utilisez les liens suivants pour en savoir plus sur l'utilisation des indicateurs de modélisation.  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Modifier les indicateurs de modélisation à l'aide du Concepteur d'exploration de données|[Afficher ou modifier la modélisation des indicateurs & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/view-or-change-modeling-flags-data-mining.md)|  
|Fournir une indication à l'algorithme pour recommander des régresseurs potentiels|[Spécifier une colonne à utiliser comme régresseur dans un modèle](../../analysis-services/data-mining/specify-a-column-to-use-as-regressor-in-a-model.md)|  
|Voir les indicateurs de modélisation pris en charge par des algorithmes spécifiques (dans la section Indicateurs de modélisation de la rubrique de référence correspondant à chaque algorithme)|[Algorithmes d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)|  
|En savoir plus sur les colonnes de structure d'exploration de données et les propriétés que vous pouvez définir dessus|[Colonnes de structure d'exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)|  
|En savoir plus sur les colonnes du modèle d'exploration de données et les indicateurs de modélisation qui peuvent être appliqués au niveau du modèle|[Colonnes du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md)|  
|Voir la syntaxe à utiliser pour travailler avec des indicateurs de modélisation dans des instructions DMX|[Indicateurs de modélisation &#40;DMX&#41;](../../dmx/modeling-flags-dmx.md)|  
|Comprendre les valeurs manquantes et la façon de les utiliser|[Les valeurs manquantes & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md)|  
|En savoir plus sur la gestion des modèles et des structures, et sur la définition de propriétés d'utilisation|[Déplacement d'objets d'exploration de données](../../analysis-services/data-mining/moving-data-mining-objects.md)|  
  
  
