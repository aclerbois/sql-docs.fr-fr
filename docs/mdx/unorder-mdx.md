---
title: Unorder (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9fe8d86b322aaf753f1ce45be2332095e2b85af9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581331"
---
# <a name="unorder-mdx"></a>Unorder (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Supprime tout classement appliqué d'un jeu spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Unorder(Set_Expression)   
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
## <a name="remarks"></a>Notes  
 Le **Unorder** fonction supprime tout classement appliqué aux tuples contenus dans le jeu par toute autre fonction ou instruction, telles que la [ordre](../mdx/order-mdx.md) (fonction). L’ordre des tuples dans le jeu retourné par le **Unorder** fonction est indéterminée.  
  
 Le **Unorder** fonction est utilisée comme indicateur de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour l’optimisation des requêtes pour le traitement de la série. À l’aide de l’ordre des tuples dans un jeu est sans importance pour un calcul ou une requête, le **Unorder** fonction peut améliorer les performances dans de tels cas. Par exemple, le [NonEmpty (MDX)](../mdx/nonempty-mdx.md) fonction il fonctionnera mieux lorsque le jeu fourni pour cette fonction n’est pas ordonné que lorsque [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] doit préserver l’ordre, bien qu’avec [!INCLUDE[ssASCurrent](../includes/ssascurrent-md.md)], le processeur de requêtes tente d’appliquer cette fonction automatiquement pour de nombreuses fonctions, telles que **somme** et **d’agrégation**. L’avantage de l’utilisation de **Unorder** n’est plus susceptible d’être notable sur les très grands jeux de comprenant des millions de tuples.  
  
## <a name="example"></a>Exemple  
 Le pseudo-code suivant présente la syntaxe employée pour cette fonction.  
  
```  
NonEmpty (UnOrder (<set_expression>))  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
