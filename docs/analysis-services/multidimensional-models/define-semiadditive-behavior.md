---
title: Définir le comportement semi-additif | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1ab36e1835af05010fa3fa206e49feec6161c31d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-semiadditive-behavior"></a>Définir le comportement semi-additif
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les mesures semi-additives, qui n'agrègent pas uniformément toutes les dimensions, sont très fréquentes dans les scénarios d'entreprise. Chaque cube qui se base sur l'instantané de soldes dans le temps pose ce problème. Ces instantanés s'utilisent dans des applications traitant de titres de placement, de soldes de compte, de budgétisation, de ressources humaines, de polices et de déclarations d'assurance, et de nombreux autres domaines d'activité.  
  
 Ajoutez le comportement semi-additif à un cube pour définir une méthode d'agrégation de mesures ou de membres individuels de l'attribut de type de compte. Si le cube contient une dimension de comptes, vous pouvez automatiquement définir le comportement semi-additif sur la base du type de compte.  
  
 Pour ajouter un comportement semi-additif, ouvrez un cube dans le Concepteur de cube et choisissez **Ajouter Business Intelligence** dans le menu Cube. Dans l’Assistant Business Intelligence, sélectionnez l’option **Définir le comportement semi-additif** dans la page **Choisir des améliorations** . L'Assistant vous guide ensuite tout au long des étapes d'identification des mesures ayant un comportement semi-additif.  
  
 Excepté pour LastChild qui est disponible dans l'édition Standard, les comportements semi-additifs sont uniquement disponibles dans les éditions Business Intelligence ou Entreprise.  
  
## <a name="define-semiadditive-behavior"></a>Définir le comportement semi-additif  
 Dans la page **Définir le comportement semi-additif** de l’Assistant, choisissez comment définir le comportement semi-additif en sélectionnant l’une des options suivantes :  
  
 **Désactiver le comportement semi-additif**  
 Supprime le comportement semi-additif d'un cube dans lequel le comportement semi-additif était auparavant défini. Cette sélection redéfinit une mesure à la valeur **SUM** si la mesure a l’un des types de fonction d’agrégation suivants :  
  
-   By Account  
  
-   Average of Children  
  
-   First Child  
  
-   Last Child  
  
-   Last Nonempty Child  
  
-   First Nonempty Child  
  
-   Aucun  
  
 Cette option ne change pas les mesures ayant une fonction d’agrégation standard : **Sum**, **Min**, **Max**, **Count**ou **Distinct****Count**.  
  
 **L'Assistant a détecté une dimension de compte, « Compte », qui contient des membres semi-additifs. Le serveur agrégera les membres de cette dimension selon le comportement semi-additif spécifié pour chaque type de compte.**  
 Provoque la définition de toutes les mesures d'un groupe de mesures dimensionné par une dimension de type Compte dans la fonction d'agrégation par le système, et le serveur agrège les membres de la dimension en fonction du comportement semi-additif spécifié pour chaque type de compte.  
  
> [!NOTE]  
>  Cette option est sélectionnée par défaut si l'Assistant détecte une dimension de type Compte.  
  
 **Définir le comportement semi-additif pour les mesures individuelles**  
 Sélectionne le comportement semi-additif de chaque mesure individuellement. La valeur par défaut est **SUM** (totalement additif).  
  
> [!NOTE]  
>  Cette option est sélectionnée par défaut si l'Assistant ne détecte pas une dimension de type Compte.  
  
 Pour chaque mesure, vous pouvez sélectionner l'un des types de fonctionnalités semi-additives décrits dans le tableau suivant.  
  
|Fonction semi-additive|Description|  
|---------------------------|-----------------|  
|Average of Children|L'agrégation d'un membre est la moyenne de ses enfants.|  
|ByAccount|Le système lit le comportement semi-additif spécifié pour le type de compte.|  
|Count|L'agrégation est un nombre de membres.|  
|Distinct Count|L'agrégation est un nombre de membres uniques.|  
|First Child|La valeur de membre est évaluée comme la valeur de son premier enfant avec la dimension de temps.|  
|FirstNonEmpty|La valeur de membre est évaluée comme la valeur de son premier enfant avec la dimension de temps qui contient les données.|  
|LastChild|La valeur de membre est évaluée comme la valeur de son dernier enfant avec la dimension de temps.|  
|LastNonEmpty|La valeur de membre est évaluée comme la valeur de son dernier enfant avec la dimension de temps qui contient les données.|  
|Max|La fonction d'agrégation maximale standard est appliquée.|  
|Min|La fonction d'agrégation minimale standard est appliquée.|  
|Aucun|Aucune agrégation n'est appliquée.|  
|SUM|La fonction d'addition standard est appliquée.|  
  
 Tout comportement semi-additif existant est écrasé lorsque vous terminez l'Assistant.  
  
  
