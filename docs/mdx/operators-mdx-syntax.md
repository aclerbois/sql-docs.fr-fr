---
title: Opérateurs (syntaxe MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 309311bfcef0ada531e391e99091a788715c8e2d
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580661"
---
# <a name="operators-mdx-syntax"></a>Opérateurs (syntaxe MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Dans la syntaxe MDX (Multidimensional Expressions), les opérateurs permettent d'exécuter les actions suivantes :  
  
-   Modifier des données, soit définitivement, soit temporairement.  
  
-   Chercher des valeurs ou des objets selon un critère de recherche.  
  
-   Implémenter une décision entre des valeurs ou des expressions.  
  
-   Tester des conditions précises avant de commencer ou de valider une transaction, ou avant d'exécuter des instructions spécifiques.  
  
 MDX prend en charge les opérateurs répertoriés dans le tableau suivant :  
  
|Si vous devez effectuer ce type d'opération|Utiliser|  
|---------------------------------------|---------|  
|Affecter une valeur à une variable ou associer une colonne de jeu de résultats à un alias|[Opérateurs d’affectation](../mdx/assignment-operators.md)|  
|Addition, soustraction, multiplication, division.|[Opérateurs arithmétiques](../mdx/arithmetic-operators.md)|  
|Tester une condition telle que AND, OR, NOT et XOR.|[Opérateurs au niveau du bit](../mdx/bitwise-operators.md)|  
|Comparer une valeur à une autre ou à une expression|[Opérateurs de comparaison](../mdx/comparison-operators.md)|  
|Combiner deux chaînes en une de façon permanente ou temporaire.|[Opérateurs de concaténation](../mdx/concatenation-operators.md)|  
|Combiner deux expressions d'ensemble en une de façon permanente ou temporaire.|[Opérateurs de jeu](../mdx/set-operators.md)|  
|Exécute une opération sur un opérande.|[Opérateurs unaires](../mdx/unary-operators.md)|  
  
> [!NOTE]  
>  Dans les requêtes, toute personne qui peut voir les données d'un cube devant être utilisé avec un certain type d'opérateur peut effectuer des opérations. Cependant, vous avez besoin des droits adéquats pour de pouvoir modifier les données.  
  
 Si vous utilisez plusieurs opérateurs, l'ordre dans lequel la syntaxe MDX les évalue est important. De même, l'utilisateur des opérateurs peut exiger la conversion d'un type de données en un autre avant que les opérateurs puissent être évalués.  
  
## <a name="evaluating-complex-expressions"></a>Évaluation d'expressions complexes  
 Vous pouvez créer une expression en utilisant des opérateurs pour combiner plusieurs expressions plus petites. Dans ces expressions complexes, la syntaxe MDX évalue les opérateurs de commande basée sur la définition de priorité des opérateurs utilisée par [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. MDX exécute des opérateurs avec une priorité plus élevée avant d’effectuer des opérateurs avec une priorité inférieure.  
  
### <a name="understanding-operator-precedence"></a>Description de la priorité des opérateurs  
 La liste suivante présente les opérateurs par ordre de priorité décroissant. Les opérateurs contenus dans la même ligne ont une priorité équivalente et sont évalués de gauche à droite sauf contrainte due à la présence de parenthèses :  
  
-   IS  
  
-   AS  
  
-   DISTINCT  
  
-   :  
  
-   ^  
  
-   /, *  
  
-   +, -  
  
-   EXISTING  
  
-   <>, >=, =, \<=, >, <  
  
-   NOT  
  
-   AND  
  
-   XOR  
  
-   - ou -  
  
 Pour plus d’informations sur les opérateurs dans la syntaxe MDX, consultez [référence des opérateurs MDX &#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md).  
  
### <a name="determining-results"></a>Détermination de résultats  
 Lorsque vous combinez des expressions simples pour constituer une expression complexe, les règles des opérateurs combinées à celles de priorité des types de données déterminent le type de données de la valeur obtenue.  
  
 Si le résultat est un caractère ou une valeur Unicode, les règles des opérateurs combinées à celles de la priorité de classement déterminent le classement du résultat. Pour plus d’informations sur les classements, consultez [langues et classements &#40;Analysis Services&#41;](../analysis-services/languages-and-collations-analysis-services.md).  
  
 Il existe également des règles pour déterminer la précision, l’échelle et la longueur du résultat basées sur la précision, l’échelle et la longueur des expressions simples.  
  
## <a name="converting-data-types"></a>Conversion de types de données  
 La syntaxe MDX convertit un objet en un autre type lorsqu'il est utilisé dans une expression qui exige un type différent. Le tableau suivant définit les règles de conversion de chaque objet.  
  
|Type d'origine|Type exigé|Conversion|  
|-------------------|-----------------|----------------|  
|Level|Définissez|\<niveau > .members|  
|Hierarchy|Membre|\<hiérarchie > .defaultmember|  
|Membre|Tuple|(\<Membre >)|  
|Tuple|Membre|\<Tuple > .item(0)|  
|Tuple|Scalaire|\<Tuple > .value|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des opérateurs MDX &#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [Éléments de syntaxe MDX &#40;MDX&#41;](../mdx/mdx-syntax-elements-mdx.md)  
  
  
