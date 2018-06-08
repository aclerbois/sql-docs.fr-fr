---
title: Instruction UPDATE CUBE (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1569d8024e23bab1841bf7757ffd828a3bc7dd4f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579751"
---
# <a name="mdx-data-manipulation---update-cube"></a>Manipulation de données MDX - UPDATE CUBE
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  L'instruction UPDATE CUBE est utilisée pour l'écriture différée des données dans une cellule d'un cube qui agrège uniquement dans son parent à l'aide de l'agrégation SUM. Pour une explication plus détaillée et un exemple, consultez « Présentation des Allocations » dans ce billet de blog : [création d’une Application de l’écriture différée avec Analysis Services (blog)](http://go.microsoft.com/fwlink/?LinkId=394977).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
UPDATE [ CUBE ] Cube_Name   
   SET   
            <update clause>   
           [, <update clause> ...n ]  
  
<update clause> ::=   
      Tuple_Expression[.VALUE]= New_Value  
      [   
        USE_EQUAL_ALLOCATION   
        | USE_EQUAL_INCREMENT   
        | USE_WEIGHTED_ALLOCATION [ BY Weight_Expression]   
        | USE_WEIGHTED_INCREMENT [ BY Weight_Expression]  
      ]  
```  
  
## <a name="arguments"></a>Arguments  
 *Cube_Name*  
 Une chaîne valide qui fournit le nom d’un cube.  
  
 *Tuple_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un tuple.  
  
 *Nouvelle_valeur*  
 Expression numérique valide.  
  
 *Weight_Expression*  
 Expression numérique MDX (Multidimensional Expressions) valide qui retourne une valeur décimale comprise entre 0 et 1.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez mettre à jour la valeur d'une cellule feuille ou non-feuille d'un cube, et allouer inévitablement la valeur d'une cellule non-feuille spécifiée à toutes les cellules feuilles dépendantes. La cellule spécifiée par l'expression de tuple peut être n'importe quelle cellule valide dans l'espace multidimensionnel (ce qui signifie que la cellule ne dispose d'aucune cellule feuille). Toutefois, la cellule doit être agrégée avec la [somme](../mdx/sum-mdx.md) fonction d’agrégation et de ne doit pas inclure un membre calculé dans le tuple utilisé pour identifier la cellule.  
  
 Il peut être utile de penser à la **UPDATE CUBE** instruction comme une sous-routine qui générera automatiquement une série d’opérations d’écriture différée de cellule aux cellules feuille et non-feuille qui seront regroupées dans une somme spécifiée.  
  
 Voici une description des méthodes d’allocation.  
  
 **USE_EQUAL_ALLOCATION :** une valeur égale basée sur l’expression suivante sera attribuée à chaque cellule feuille qui contribue à la cellule mise à jour.  
  
```  
<leaf cell value> =   
<New Value> / Count(leaf cells that are contained in <tuple>)  
```  
  
 **USE_EQUAL_INCREMENT :** changera chaque cellule feuille qui contribue à la cellule mise à jour en fonction de l’expression suivante.  
  
```  
<leaf cell value> = <leaf cell value> +   
(<New Value > - <existing value>) /  
Count(leaf cells contained in <tuple>)  
```  
  
 **USE_WEIGHTED_ALLOCATION :** une valeur égale basée sur l’expression suivante sera attribuée à chaque cellule feuille qui contribue à la cellule mise à jour.  
  
```  
<leaf cell value> = < New Value> * Weight_Expression  
```  
  
 **USE_WEIGHTED_INCREMENT :** changera chaque cellule feuille qui contribue à la cellule mise à jour en fonction de l’expression suivante.  
  
```  
<leaf cell value> = <leaf cell value> +   
(<New Value> - <existing value>)  * Weight_Expression  
```  
  
 Si une expression de poids n’est pas spécifiée, le **UPDATE CUBE** instruction utilise implicitement l’expression suivante.  
  
```  
Weight_Expression = <leaf cell value> / <existing value>  
```  
  
 Une expression de poids doit être exprimée sous forme de valeur décimale comprise entre zéro (0) et 1. Cette valeur spécifie le ratio de la valeur allouée que vous voulez affecter aux cellules feuilles affectées par l'allocation. Il est de la responsabilité du programmeur de l'application cliente de créer des expressions dont les valeurs agrégées de cumul seront égales à la valeur allouée de l'expression.  
  
> [!CAUTION]  
>  L'application cliente doit considérer simultanément l'allocation de toutes les dimensions afin d'éviter l'éventualité de résultats imprévus, notamment des valeurs de cumul incorrectes ou des données incohérentes.  
  
 Chaque **UPDATE CUBE** allocation doit être considérée comme atomique pour les besoins des transactions. Ceci signifie que si l'une des opérations d'allocation échoue pour quelque raison que ce soit, par exemple une erreur de formule ou une violation de sécurité, l'ensemble de l'opération UPDATE CUBE échouera. Avant le calcul des opérations individuelles d'allocation, un instantané des données est réalisé pour garantir la correction des calculs résultants.  
  
> [!CAUTION]  
>  Lorsqu'elle est utilisée sur une mesure contenant des entiers, la méthode USE_WEIGHTED_ALLOCATION peut retourner des résultats imprécis causés par des arrondis successifs.  
  
> [!IMPORTANT]  
>  Quand les cellules mises à jour ne se chevauchent pas, la propriété de chaîne de connexion **Update Isolation Level** peut être utilisée pour améliorer les performances pour UPDATE CUBE.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A>   
 [Les instructions de Manipulation de données MDX &#40;MDX&#41;](../mdx/mdx-data-manipulation-statements-mdx.md)  
  
  
