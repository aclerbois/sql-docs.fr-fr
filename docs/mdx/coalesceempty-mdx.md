---
title: CoalesceEmpty (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cbd5928e859436b90b986e9e0ea0d09e91ccd664
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577311"
---
# <a name="coalesceempty-mdx"></a>CoalesceEmpty (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Convertit une valeur de cellule vide en valeur de cellule non vide spécifiée. Il peut s'agir d'un nombre ou d'une chaîne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Numeric syntax  
CoalesceEmpty( Numeric_Expression1 [ ,Numeric_Expression2,...n] )  
  
String syntax  
CoalesceEmpty(String_Expression1 [ ,String_Expression2,...n] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Numeric_Expression1*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
 *Numeric_Expression2*  
 Expression numérique valide qui correspond généralement à une valeur numérique spécifique.  
  
 *String_Expression1*  
 Expression de chaîne valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent une chaîne.  
  
 *String_Expression2*  
 Expression de chaîne valide qui correspond généralement à une valeur de chaîne spécifique substituée à une valeur NULL retournée par la première expression de chaîne.  
  
## <a name="remarks"></a>Notes  
 Si une ou plusieurs expressions numériques sont spécifiées, le **CoalesceEmpty** fonction retourne la valeur numérique de la première expression (de gauche à droite) numérique qui peut être résolue en une valeur non vide. Si aucune des expressions numériques spécifiées ne peut être résolue en une valeur non vide, la fonction retourne la valeur de cellule vide. En règle générale, la valeur de la deuxième expression numérique correspond à la valeur numérique substituée à une valeur NULL retournée par la première expression numérique.  
  
 Si une ou plusieurs expressions de chaîne sont spécifiées, la fonction retourne la valeur de chaîne de la première expression de chaîne (de gauche à droite) qui peut être résolue à une valeur non vide. Si aucune des expressions de chaîne spécifiées ne peut être résolue en une valeur non vide, la fonction retourne la valeur de cellule vide. En règle générale, la valeur de la deuxième expression de chaîne correspond à la valeur de chaîne substituée à une valeur NULL retournée par la première expression de chaîne.  
  
 Le **CoalesceEmpty** fonction accepte uniquement les valeurs du même type. Autrement dit, toutes les expressions de valeur spécifiées ne doivent prendre la valeur que de types de données numériques ou de cellule vide, ou toutes les expressions de valeur spécifiées doivent prendre la valeur de types de données chaîne ou de cellule vide. Un seul appel à cette fonction ne peut pas contenir à la fois des expressions numériques et des expressions de chaîne.  
  
 Pour plus d'informations sur les cellules vides, consultez la documentation OLE DB.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant interroge la **Adventure Works** cube. Cet exemple retourne la quantité commandée de chaque produit et le pourcentage des quantités commandées par catégorie. Le **CoalesceEmpty** fonction garantit que les valeurs null sont représentées en tant que zéro (0) lors de la mise en forme des membres calculés.  
  
```  
WITH   
   MEMBER [Measures].[Order Percent by Category] AS  
   CoalesceEmpty(   
      ([Product].[Product Categories].CurrentMember,  
        Measures.[Order Quantity]) /   
          (  
           Ancestor  
           ( [Product].[Product Categories].CurrentMember,   
             [Product].[Product Categories].[Category]  
           ), Measures.[Order Quantity]  
       ), 0  
   ), FORMAT_STRING='Percent'  
SELECT   
   {Measures.[Order Quantity],  
      [Measures].[Order Percent by Category]} ON COLUMNS,  
{[Product].[Product].Members} ON ROWS  
FROM [Adventure Works]  
WHERE {[Date].[Calendar Year].[Calendar Year].&[2003]}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
