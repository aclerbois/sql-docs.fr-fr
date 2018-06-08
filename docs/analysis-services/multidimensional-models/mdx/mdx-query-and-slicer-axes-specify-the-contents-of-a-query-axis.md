---
title: Spécification du contenu d’un axe de requête (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bceafa9fb8ddd89162deca105404c317001a86bb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-query-and-slicer-axes---specify-the-contents-of-a-query-axis"></a>Requête MDX et les Axes de segment - spécifier le contenu d’un axe de requête
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les axes de requête spécifient les bords d'un jeu de cellules retourné par une instruction SELECT MDX (Multidimensional Expressions). La spécification des bords d'un jeu de cellules vous permet de restreindre les données retournées qui sont visibles par le client.  
  
 Pour spécifier des axes de requête, utilisez la clause `<SELECT query axis clause>` pour attribuer un jeu à un axe de requête particulier. Chaque valeur `<SELECT query axis clause>` définit un axe de requête. Le nombre d'axes du dataset est égal au nombre de valeurs `<SELECT query axis clause>` de l'instruction SELECT.  
  
## <a name="query-axis-syntax"></a>Syntaxe de l'axe de requête  
 L'exemple suivant indique la syntaxe de `<SELECT query axis clause>`.  
  
```  
  
<SELECT query axis clause> ::=  
   [ NON EMPTY ] Set_Expression [ <SELECT dimension property list clause> ] [<HAVING clause>]  
   ON {  
      Integer_Expression |   
      AXIS( Integer_Expression ) |   
      {COLUMNS | ROWS | PAGES | SECTIONS | CHAPTERS}     
      }  
  
```  
  
 Chaque axe de requête possède un numéro : 0 pour l'axe des X, 1 pour l'axe des Y, 2 pour l'axe des Z, etc. Dans la syntaxe de `<SELECT query axis clause>`, la valeur `Integer_Expression` spécifie le numéro de l'axe. Une requête MDX peut prendre en charge jusqu'à 128 axes spécifiés, mais très peu de requêtes MDX en utilisent plus de 5. Pour les cinq premiers axes, les alias COLUMNS, ROWS, PAGES, SECTIONS et CHAPTERS peuvent remplacer les numéros.  
  
 Une requête MDX ne peut pas omettre les axes de requête. C'est-à-dire qu'une requête qui comporte un ou plusieurs axes de requête ne doit pas exclure les axes dotés de numéros inférieurs ou intermédiaires. Par exemple, une requête ne peut pas avoir un axe ROWS sans un axe COLUMNS, ni des axes COLUMNS et PAGES sans un axe ROWS.  
  
 Toutefois, vous pouvez spécifier une clause SELECT sans axes (c'est-à-dire une clause SELECT vide). Dans ce cas, toutes les dimensions sont des dimensions de découpage, et la requête MDX sélectionne une cellule.  
  
 Dans la syntaxe d'axe de requête ci-dessus, chaque valeur `Set_Expression` spécifie le jeu définissant le contenu de l'axe de requête. Pour plus d’informations sur les jeux, consultez [Utilisation de membres, de tuples et de jeux &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md).  
  
## <a name="examples"></a>Exemples  
 L'instruction SELECT simple suivante retourne la mesure Internet Sales Amount sur l'axe des colonnes, et utilise la fonction MDX MEMBERS pour retourner tous les membres de la hiérarchie Calendar de la dimension Date sur l'axe des lignes :  
  
```  
SELECT {[Measures].[Internet Sales Amount]} ON COLUMNS,  
{[Date].[Calendar].MEMBERS} ON ROWS  
FROM [Adventure Works]  
  
```  
  
 Les deux requêtes suivantes retournent exactement les mêmes résultats, mais illustrent l'utilisation de numéros d'axe plutôt que d'alias :  
  
```  
SELECT {[Measures].[Internet Sales Amount]} ON 0,  
{[Date].[Calendar].MEMBERS} ON 1  
FROM [Adventure Works]  
  
SELECT {[Measures].[Internet Sales Amount]} ON AXIS(0),  
{[Date].[Calendar].MEMBERS} ON AXIS(1)  
FROM [Adventure Works]  
  
```  
  
 Le mot clé NON EMPTY, utilisé avant la définition du jeu, permet de supprimer facilement tous les tuples vides d'un axe. Par exemple, dans les exemples étudiés jusqu'à présent, il n'y a pas de données dans le cube à partir d'août 2004. Pour supprimer toutes les lignes de l'ensemble de cellules qui n'ont aucune donnée dans aucune colonne, ajoutez simplement NON EMPTY avant la définition du jeu sur l'axe des lignes, comme suit :  
  
```  
SELECT {[Measures].[Internet Sales Amount]} ON COLUMNS,  
NON EMPTY  
{[Date].[Calendar].MEMBERS} ON ROWS  
FROM [Adventure Works]  
  
```  
  
 Il est possible d'utiliser NON EMPTY sur tous les axes d'une requête. Comparez les résultats des deux requêtes suivantes, la première n'utilisant pas NON EMPTY et la deuxième l'utilisant sur les deux axes :  
  
```  
SELECT {[Measures].[Internet Sales Amount]}   
* [Promotion].[Promotion].[Promotion].MEMBERS  
ON COLUMNS,  
{[Date].[Calendar].[Calendar Year].MEMBERS} ON ROWS  
FROM [Adventure Works]  
WHERE([Product].[Subcategory].&[19])  
  
SELECT NON EMPTY {[Measures].[Internet Sales Amount]}   
* [Promotion].[Promotion].[Promotion].MEMBERS  
ON COLUMNS,  
NON EMPTY  
{[Date].[Calendar].[Calendar Year].MEMBERS} ON ROWS  
FROM [Adventure Works]  
WHERE([Product].[Subcategory].&[19])  
  
```  
  
 La clause HAVING peut être utilisée pour filtrer le contenu d'un axe en fonction d'un critère spécifique ; elle est moins souple que d'autres méthodes qui permettent d'obtenir les mêmes résultats, telles que la fonction FILTER, mais elle est plus simple à utiliser. Voici un exemple qui retourne uniquement les dates auxquelles la valeur Internet Sales Amount est supérieure à 15 000 $ :  
  
```  
SELECT {[Measures].[Internet Sales Amount]}   
ON COLUMNS,  
NON EMPTY  
{[Date].[Calendar].[Date].MEMBERS}   
HAVING [Measures].[Internet Sales Amount]>15000  
ON ROWS  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Spécification du contenu d’un axe de secteur & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-specify-the-contents-of-a-slicer-axis.md)  
  
  
