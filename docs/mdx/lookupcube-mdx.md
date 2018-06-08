---
title: LookupCube (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- LOOKUPCUBE
dev_langs:
- kbMDX
helpviewer_keywords:
- LookupCube function
ms.assetid: 243fa101-328a-4016-86e0-d8b5977e15a9
caps.latest.revision: 32
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: b21de8eae8df0d591ab99038065a24ed85453ed1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lookupcube-mdx"></a>LookupCube (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la valeur d'une expression MDX (Multidimensional Expressions) évaluée sur un autre cube spécifié dans la même base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Numeric expression syntax  
LookupCube(Cube_Name, Numeric_Expression )  
  
String expression syntax  
LookupCube(Cube_Name, String_Expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *Cube_Name*  
 Une expression de chaîne valide qui spécifie le nom d’un cube.  
  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
 *String_Expression*  
 Expression de chaîne valide qui correspond généralement à une expression MDX (Multidimensional Expressions) valide des coordonnées des cellules qui retourne une chaîne.  
  
## <a name="remarks"></a>Notes  
 Si une expression numérique est spécifiée, la **LookupCube** fonction évalue l’expression numérique spécifiée dans le cube spécifié et retourne la valeur numérique qui en résulte.  
  
 Si une expression de chaîne est spécifiée, le **LookupCube** fonction évalue l’expression de chaîne spécifiée dans le cube spécifié et retourne la valeur de chaîne qui en résulte.  
  
 Le **LookupCube** fonctionne sur des cubes dans la même base de données que le cube source sur lequel la requête MDX qui contient le **LookupCube** fonction est en cours d’exécution.  
  
> [!IMPORTANT]  
>  Vous devez fournir tous les membres actuels nécessaires dans l'expression numérique ou de chaîne puisque le contexte de la requête actuelle n'est pas reporté dans le cube interrogé.  
  
 Tout calcul à l’aide de la **LookupCube** fonction est susceptible de souffrir de performances médiocres. Au lieu d'utiliser cette fonction, envisagez de modifier la conception votre solution afin que toutes les données dont vous avez besoin soient présentes dans un cube.  
  
## <a name="examples"></a>Exemples  
 La requête suivante illustre l'utilisation de LookupCube :  
  
 `WITH MEMBER MEASURES.LOOKUPCUBEDEMO AS`  
  
 `LOOKUPCUBE("Adventure Works", "[Measures].[In" + "ternet Sales Amount]")`  
  
 `SELECT MEASURES.LOOKUPCUBEDEMO  ON 0`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
