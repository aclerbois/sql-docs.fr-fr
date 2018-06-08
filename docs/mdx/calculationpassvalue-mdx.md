---
title: CalculationPassValue (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 88d7d8c52105feb14c982586f9a819977fc81ed0
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577231"
---
# <a name="calculationpassvalue-mdx"></a>CalculationPassValue (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la valeur numérique ou la valeur de chaîne d'une expression MDX (Multidimensional Expressions) évaluée pendant le test de calcul spécifié d'un cube.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
      Numeric syntax  
CalculationPassValue(Numeric_Expression,Pass_Value [, ABSOLUTE | RELATIVE [,ALL]])  
String syntax  
CalculationPassValue(String_Expression ,Pass_Value [, ABSOLUTE | RELATIVE [,ALL]])  
```  
  
## <a name="arguments"></a>Arguments  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
 *String_Expression*  
 Expression de chaîne valide qui correspond généralement à une expression MDX (Multidimensional Expressions) valide des coordonnées des cellules qui retournent un nombre exprimé sous forme de chaîne.  
  
 *Pass_Value*  
 Expression numérique valide qui précise le numéro du test de calcul.  
  
 ABSOLUTE  
 Une valeur d’indicateur accès qui spécifie que la *Pass_Value* paramètre contient l’index de base zéro de la passe de calcul. ABSOLUTE est la valeur d'indicateur d'accès par défaut si aucune valeur de ce type n'est spécifiée.  
  
 RELATIVE  
 Une valeur d’indicateur accès qui spécifie que la *Pass_Value* paramètre contient un décalage relatif au test de calcul du calcul déclencheur. Si le décalage se résout à un index de test de calcul inférieur à 0, le test de calcul 0 est utilisé et aucune erreur ne survient.  
  
 ALL  
 Lorsque vous définissez cet indicateur, toutes les valeurs sont NULL, sauf les valeurs chargées par le moteur de stockage. Si l'indicateur n'est pas défini, les valeurs sont agrégées sans qu'aucun calcul ne soit appliqué.  
  
## <a name="remarks"></a>Notes  
 Si une expression numérique est spécifiée, la fonction retourne une valeur numérique en évaluant l'expression numérique MDX précisée dans le test de calcul spécifié, et éventuellement modifiée par un indicateur d'accès et un modificateur d'indicateur d'accès.  
  
 Si une expression de chaîne est fournie, la fonction retourne une valeur de chaîne en évaluant l’expression de chaîne MDX précisée dans le test de calcul spécifié et éventuellement modifiée par un indicateur d’accès et un modificateur d’indicateur d’accès *.*  
  
 Avec une résolution automatique de la récursivité dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], cette fonction a peu utilisation pratique.  
  
> [!NOTE]  
>  Seuls les administrateurs peuvent utiliser le **CalculationPassValue** fonction dans un script MDX. Une erreur se produit si un script MDX contenant cette fonction est exécuté dans le contexte d'un rôle qui ne dispose pas des droits d'administrateur.  
  
## <a name="see-also"></a>Voir aussi  
 [CalculationCurrentPass &#40;MDX&#41;](../mdx/calculationcurrentpass-mdx.md)   
 [IIf &#40;MDX&#41;](../mdx/iif-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
