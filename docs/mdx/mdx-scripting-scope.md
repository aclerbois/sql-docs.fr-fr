---
title: L’instruction SCOPE (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4c390d13d2f916dc287646f85def1513f910b635
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579961"
---
# <a name="mdx-scripting---scope"></a>Écriture de scripts MDX - étendue
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Limite l'étendue des instructions MDX (Multidimensional Expressions) spécifiées à un sous-cube spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SCOPE(Subcube_Expression)   
   [ MDX_Statement ]  
END SCOPE  
  
Subcube_Expression ::=(Auxiliary_Subcube [, Auxiliary_Subcube,...n])  
  
Auxiliary_Subcube ::=   
        Limited_Set   
    | Root([dimension_name])   
    | Leaves([dimension_name])  
  
Limited_Set ::=   
        single_tuple   
    | member   
    | Common_Grain_Members   
    | hierarchy.members   
    | level.members   
    | {}   
    | Descendants  
            (  
                  Member  
         , [level  
         [  
            , SELF   
             | AFTER   
                          | BEFORE   
                          | SELF_AND_AFTER   
                          | SELF_AND_BEFORE   
                          | SELF_BEFORE_AFTER   
                          | LEAVES  
                  ]  
            )   
[* <limited set>]  
```  
  
## <a name="arguments"></a>Arguments  
 *Subcube_Expression*  
 Expression de sous-cube MDX valide.  
  
 *MDX_Statement*  
 Instruction MDX valide.  
  
 *Common_Grain_Members*  
 Instruction MDX valide qui évalue les membres qui ont la même granularité.  
  
 *single_tuple*  
 Tuple unique.  
  
## <a name="remarks"></a>Notes  
 L'instruction SCOPE détermine le sous-cube qui sera affecté par l'exécution d'une ou plusieurs instructions MDX. Sauf si une instruction MDX est insérée dans une instruction SCOPE, l'étendue implicite d'une instruction MDX est le cube tout entier.  
  
> [!NOTE]  
>  Les membres cachés sont exposés dans les instructions SCOPE.  
  
 Instructions SCOPE créent des sous-cubes qui exposent les « trous » quel que soit le **MDX Compatibility** paramètre. Par exemple, l'instruction `Scope( Customer.State.members )` peut inclure les états de pays ou de régions qui n'ont pas d'états, mais dans lesquels des membres d'espace réservés autrement invisibles ont été insérés.  
  
 Les membres calculés et les jeux nommés créés dans le cadre d'une instruction SCOPE ne sont pas affectés par l'instruction SCOPE.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant, à partir du script de calcul MDX dans la solution exemple Adventure Works, définit l’étendue actuelle en tant que trimestre fiscal dans l’année fiscale 2005 et de la mesure de quota du montant des ventes, puis assigne une valeur aux cellules dans l’étendue actuelle à l’aide de la **ParallelPeriod** (fonction). L’exemple modifie ensuite l’étendue à l’aide d’une autre instruction SCOPE et qu’il exécute ensuite une autre assignation à l’aide du [This (MDX)](../mdx/this-mdx.md) (fonction).  
  
```  
Scope   
 (   
    [Date].[Fiscal Year].&[2005],  
    [Date].[Fiscal].[Fiscal Quarter].Members,  
    [Measures].[Sales Amount Quota]  
 ) ;     
  
   This = ParallelPeriod                               
          (   
             [Date].[Fiscal].[Fiscal Year], 1,  
             [Date].[Fiscal].CurrentMember   
          ) * 1.35 ;  
  
/*-- Allocate equally to months in FY 2002 -----------------------------*/  
  
  Scope   
  (   
     [Date].[Fiscal Year].&[2002],  
     [Date].[Fiscal].[Month].Members   
  ) ;     
  
    This = [Date].[Fiscal].CurrentMember.Parent / 3 ;     
  
  End Scope ;     
End Scope ;     
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions de script MDX &#40;MDX&#41;](../mdx/mdx-scripting-statements-mdx.md)  
  
  
