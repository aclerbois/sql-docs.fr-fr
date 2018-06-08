---
title: Utilisez les fonctions d’agrégation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d4e30b5670e7a9a02fdfa9f3bbfad6889e83b562
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="use-aggregate-functions"></a>Utiliser des fonctions d'agrégation
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsqu'une dimension est utilisée pour segmenter une mesure, celle-ci est résumée en fonction des hiérarchies contenues dans la dimension. Le comportement de sommation dépend de la fonction d'agrégation spécifiée pour la mesure. Pour la plupart des mesures contenant des données numériques, la fonction d’agrégation est **Sum**. La valeur de la mesure est la somme de montants différents selon le niveau auquel la hiérarchie est active.  
  
 Dans Analysis Services, chaque mesure que vous créez s'appuie sur une fonction d'agrégation qui détermine l'opération associée à la mesure. Les types d’agrégation prédéfinis incluent les fonctions **Sum**, **Min**, **Max**, **Count**et **Distinct Count**, ainsi que plusieurs autres fonctions plus spécialisées. Si vous avez besoin d'agrégations basées sur des formules complexes ou personnalisées, vous pouvez aussi créer un calcul MDX au lieu d'utiliser une fonction d'agrégation prédéfinie. Par exemple, si vous souhaitez définir une mesure pour une valeur de pourcentage, utilisez une mesure calculée dans MDX. Consultez [Instruction CREATE MEMBER &#40;MDX&#41;](../../mdx/mdx-data-definition-create-member.md).  
  
 Les mesures créées à l'aide de l'Assistant Cube sont affectées à un type d'agrégation au moment de leur définition. Le type d’agrégation est toujours **Sum**(en supposant que la colonne source contient des données numériques). **Sum** est utilisé quel que soit le type de données de la colonne source. Par exemple, si vous avez utilisé l’Assistant Cube pour créer des mesures et que vous avez extrait toutes les colonnes d’une table de faits, vous pouvez noter que toutes les mesures obtenues ont une agrégation **Sum**, même si la colonne source est de type datetime. Examinez toujours les méthodes d'agrégation prédéfinies pour les mesures créées via l'Assistant pour vous assurer que la fonction d'agrégation est appropriée.  
  
 Vous pouvez affecter ou changer la méthode d’agrégation dans la définition du cube, via [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] ou via MDX. Pour obtenir d’autres instructions, consultez [Créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md) ou [Aggregate &#40;MDX&#41;](../../mdx/aggregate-mdx.md).  
  
##  <a name="AggFunction"></a> Fonctions d'agrégation  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit des fonctions pour agréger les mesures avec les dimensions qui sont contenues dans les groupes de mesures. *L’additivité* d’une fonction d’agrégation détermine la manière dont la mesure est agrégée à travers toutes les dimensions du cube. Les fonctions d'agrégation sont divisées en trois catégories en fonction de leur niveau d'additivité :  
  
 Additive  
 Une mesure additive, ou mesure entièrement additive, peut être agrégée avec toutes les dimensions incluses dans le groupe de mesures qui contient la mesure, sans aucune restriction.  
  
 Semi-additive  
 Une mesure semi-additive peut être agrégée avec certaines des dimensions (mais pas toutes) incluses dans le groupe de mesures qui contient la mesure. Par exemple, une mesure représentant la quantité disponible en stock peut être agrégée avec une dimension géographique pour générer la quantité totale disponible pour tous les entrepôts, mais cette mesure ne peut pas être agrégée avec une dimension de temps étant donné qu'elle représente un instantané périodique des quantités disponibles. L'agrégation de cette mesure avec une dimension de temps produirait des résultats incorrects. Pour plus d’informations, consultez [Définir le comportement semi-additif](../../analysis-services/multidimensional-models/define-semiadditive-behavior.md) .  
  
 Non additive  
 Une mesure non additive ne peut être agrégée avec aucune des dimensions incluses dans le groupe de mesures qui contient la mesure. Une mesure non additive doit être calculée séparément pour chaque cellule du cube représentant la mesure. Par exemple, une mesure calculée qui renvoie un pourcentage, comme une marge bénéficiaire, ne peut pas être agrégée à partir des valeurs de pourcentage des membres enfants d'une dimension.  
  
 Le tableau suivant répertorie les fonctions d’agrégation disponibles dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], et décrit à la fois l’additivité et la sortie attendue de chaque fonction.  
  
|Fonction d’agrégation|L’additivité|Valeur renvoyée|  
|--------------------------|----------------|--------------------|  
|**Sum**|Additive|Calcule la somme des valeurs de tous les membres enfants. Il s'agit de la fonction d'agrégation par défaut.|  
|**Count**|Additive|Renvoie le nombre total de membres enfants.|  
|**Min**|Semi-additive|Renvoie la plus petite valeur pour tous les membres enfants.|  
|**Max**|Semi-additive|Renvoie la plus grande valeur pour tous les membres enfants.|  
|**DistinctCount**|Non additive|Renvoie le nombre total de membres enfants uniques. Pour plus d’informations, consultez [Présentation des mesures de comptage de valeurs](../../analysis-services/multidimensional-models/use-aggregate-functions.md#bkmk_distinct) dans la section suivante.|  
|**Aucun**|Non additive|Aucune agrégation n'est effectuée et toutes les valeurs des membres feuille et non-feuille d'une dimension sont fournies directement à partir de la table de faits du groupe de mesures qui contient la mesure. Si aucune valeur ne peut être lue à partir de la table de faits d'un membre, la valeur de ce membre est NULL.|  
|**ByAccount**|Semi-additive|Calcule l'agrégation conformément à la fonction d'agrégation affectée au type de compte d'un membre d'une dimension de comptes. Si le groupe de mesures ne contient aucune dimension de type compte, cette fonction est traitée comme la fonction d’agrégation **None** .<br /><br /> Pour plus d’informations sur les dimensions de compte, consultez [Créer un compte Finance de la dimension de type parent-enfant](../../analysis-services/multidimensional-models/database-dimensions-finance-account-of-parent-child-type.md).|  
|**AverageOfChildren**|Semi-additive|Calcule la moyenne des valeurs de tous les membres enfants non vides.|  
|**FirstChild**|Semi-additive|Renvoie la valeur du premier membre enfant.|  
|**LastChild**|Semi-additive|Renvoie la valeur du dernier membre enfant.|  
|**FirstNonEmpty**|Semi-additive|Renvoie la valeur du premier membre enfant non vide.|  
|**LastNonEmpty**|Semi-additive|Renvoie la valeur du dernier membre enfant non vide.|  
  
##  <a name="bkmk_distinct"></a> Présentation des mesures de comptage de valeurs  
 Une mesure dont la propriété **Fonction d’agrégation** a la valeur **Comptage de valeurs** est appelée mesure de comptage de valeurs. Une mesure de comptage de valeurs peut être utilisée pour compter les occurrences des membres du niveau le plus bas d'une dimension dans la table de faits. Avec ce type de comptage, si un membre apparaît plusieurs fois, il n'est compté qu'une seule fois. Une mesure de comptage de valeurs est toujours placée dans un groupe de mesures dédié. Placer une mesure de comptage de valeurs dans son propre groupe de mesures est une recommandation qui a été intégrée dans le concepteur afin d'optimiser les performances.  
  
 Les mesures de comptage de valeurs s'utilisent généralement pour déterminer pour chaque membre d'une dimension combien de membres distincts du niveau le plus bas d'une autre dimension possèdent des lignes identiques à celles de la table de faits. Par exemple, dans un cube Sales, pour chaque client et groupe de clients, combien de produits différents ont été achetés ? C'est-à-dire, pour chaque membre de la dimension Customers, combien de membres différents du niveau le plus bas de la dimension Products ont en commun des lignes avec la table de faits ? Ou, par exemple, dans un cube de visites d'un site Internet, pour chaque visiteur et chaque groupe de visiteurs, combien de pages distinctes ont été consultées sur le site Internet ? C'est-à-dire, pour chaque membre de la dimension Site Visitors, combien de membres différents du niveau le plus bas de la dimension Pages ont en commun des lignes avec la table de faits ? Dans chacun de ces exemples, les membres du niveau le plus bas de la deuxième dimension sont comptés par une mesure de comptage de valeurs.  
  
 Ce type d'analyse ne se limite pas forcément à deux dimensions. En fait, une mesure de comptage de valeurs peut être séparée et découpée selon n'importe quelle combinaison de dimensions du cube, y compris la dimension qui contient les membres comptés.  
  
 Une mesure de comptage de valeurs qui compte les membres repose sur une colonne clé étrangère de la table de faits. (C’est-à-dire que la propriété **Colonne source** de la mesure identifie cette colonne.) Cette colonne est jointe à la colonne de table de dimensions qui identifie les membres comptés par la mesure de comptage de valeurs.  
  
## <a name="see-also"></a>Voir aussi  
 [Mesures et groupes de mesures](../../analysis-services/multidimensional-models/measures-and-measure-groups.md)   
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../../mdx/mdx-function-reference-mdx.md)   
 [Définir le comportement semi-additif](../../analysis-services/multidimensional-models/define-semiadditive-behavior.md)  
  
  
