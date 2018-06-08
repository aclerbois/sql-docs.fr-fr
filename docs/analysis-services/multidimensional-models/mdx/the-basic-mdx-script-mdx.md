---
title: Le Script MDX de base (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2bebee1057180259c9813d7a650594c0c6d4736d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="the-basic-mdx-script-mdx"></a>Script MDX de base (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Un script MDX (Multidimensional Expressions) définit le processus de calcul pour un cube dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Il existe deux types de scripts MDX :  
  
 **Script MDX par défaut**  
 Au moment de la création d’un cube, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] crée un script MDX par défaut pour celui-ci. Ce script définit un test de calcul pour l'intégralité du cube.  
  
 **Script MDX défini par l'utilisateur**  
 Lorsque vous avez créé un cube, vous pouvez ajouter des scripts MDX définis par l'utilisateur qui étendent les possibilités de calcul du cube.  
  
## <a name="the-default-mdx-script"></a>Script MDX par défaut  
 Le script MDX par défaut créé par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] au moment de la définition d’un cube contient une instruction CALCULATE unique. Cette dernière se situe au début du script MDX par défaut et indique que l'intégralité du cube doit être calculée pendant le premier test de calcul.  
  
 Le script MDX par défaut contient également les commandes de script qui créent des jeux nommés, des assignations et des membres calculés dans le Concepteur de cube :  
  
-   [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ajoute directement les commandes de script au script MDX par défaut.  
  
-   Pour chaque jeu nommé défini dans le cube, il existe une instruction CREATE SET correspondante dans le script MDX par défaut.  
  
-   Pour chaque membre calculé défini dans le cube, il existe une instruction CREATE MEMBER correspondante dans le script MDX par défaut.  
  
 Vous pouvez contrôler l’ordre des commandes de script, des jeux nommés et des membres calculés dans le script MDX par défaut à l’aide de l’onglet **Calculs** du Concepteur de cube. Pour plus d’informations sur la définition des calculs stockés dans le script MDX par défaut, consultez [Calculs dans les modèles multidimensionnels](../../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md).  
  
 Si aucun script MDX n'est associé à un cube, ce dernier utilise le script MDX par défaut. Un cube doit être au moins associé à un script MDX, car il se base sur celui-ci pour déterminer le comportement de calcul. En d'autres termes, un cube qui n'est associé à aucun script MDX ou associé à un script MDX vide ne peut pas calculer de cellules. Si vous créez des cubes par programmation, à l'aide de commandes ASSL (Analysis Services Scripting Language) ou de l'objet AMO (Analysis Management Object), il est recommandé de créer un script MDX par défaut contenant une instruction CALCULATE unique pour le cube.  
  
## <a name="mdx-script-content"></a>Contenu du script MDX  
 Un script MDX peut contenir les instructions et expressions suivantes :  
  
 Toutes les instructions de script MDX  
 Dans les scripts MDX, les instructions de script MDX contrôlent le contexte et la portée des calculs et gèrent le comportement des autres instructions dans le script MDX. Cette catégorie comprend les instructions suivantes :  
  
-   [CALCULER](../../../mdx/mdx-scripting-calculate.md)  
  
-   [FIGER](../../../mdx/mdx-scripting-freeze.md)  
  
-   [ÉTENDUE](../../../mdx/mdx-scripting-scope.md)  
  
 Pour plus d’informations sur les instructions de script MDX, consultez [Instructions de script MDX &#40;MDX&#41;](../../../mdx/mdx-scripting-statements-mdx.md).  
  
 [CRÉER DES MEMBRES](../../../mdx/mdx-data-definition-create-member.md)  
 L'instruction CREATE MEMBER crée des membres calculés. Pour plus d’informations sur la création de membres calculés, consultez [Création de membres calculés dans MDX &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-calculated-members-building-calculated-members.md).  
  
 [CRÉER LE JEU](../../../mdx/mdx-data-definition-create-set.md)  
 L'instruction CREATE SET crée des jeux nommés. Pour plus d’informations sur la création de jeux nommés, consultez [Création de jeux nommés à l’aide d’expressions MDX &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-named-sets-building-named-sets.md).  
  
 Instructions conditionnelles  
 Les instructions conditionnelles ajoutent une logique conditionnelle aux scripts MDX. Cette catégorie comprend les instructions [CASE](../../../mdx/case-statement-mdx.md) et [IF](../../../mdx/mdx-scripting-if.md) .  
  
 Expressions d'assignation  
 Une expression d'assignation affecte une expression, telle qu'une valeur, à un sous-cube contraint. Une expression de sous-cube contraint est une collection d'expressions de jeux contraints qui définit les « côtés » d'un sous-cube au sein d'un script MDX. Les codes suivants représentent la syntaxe d'une expression de sous-cube contraint :  
  
```  
<Constrained subcube> ::= (   
    ( <Constrained set> [<Crossjoin operator> <Constrained set>...] |  
    <ROOT function> |  
    <TREE function> |  
    LEAVES() |  
    * ) [, <Constrained subcube>...]  
<Constrained set> ::=   
    <Natural hierarchy>.MEMBERS |   
    <Natural hierarchy>.LEVEL(<numeric expression>).MEMBERS |   
    { <Natural hierarchy member> } |   
    DESCENDANTS( <Natural hierarchy member>, <Level expression>, ( SELF | AFTER | SELF_AND_AFTER ) ) |   
    DESCENDANTS( <Natural hierarchy member>, , LEAVES )  
<Natural hierarchy> ::= <Hierarchy identifier>  
<Natural hierarchy member> ::= <Natural hierarchy>.<identifier>[.<identifier>...]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence du langage MDX & #40 ; MDX & #41 ;](../../../mdx/mdx-language-reference-mdx.md)   
 [Principes de base de script MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-scripting-fundamentals-analysis-services.md)  
  
  
