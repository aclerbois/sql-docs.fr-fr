---
title: Expressions logiques (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- logical operators [SQL Server], XQuery
- effective Boolean value [XQuery]
- logical expressions [XQuery]
- EBV
- expressions [XQuery], logical
ms.assetid: de94cd2e-2d48-49fb-9ebd-a2d90c79bf62
caps.latest.revision: 26
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 20579cbbc8fc16fad2ab33c033fa8c73fbeadf1d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="logical-expressions-xquery"></a>Expressions logiques (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery prend en charge la logique **et** et **ou** opérateurs.  
  
```  
expression1 and expression2  
expression1 or expression2  
```  
  
 Les expressions de test, `expression1,``expression2`, dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut entraîner une séquence vide, une séquence d’un ou plusieurs nœuds ou une valeur booléenne unique. D'après le résultat, la valeur booléenne effective est déterminée de la façon suivante :  
  
-   Si les résultats d'une expression de test est une séquence vide, le résultat de l'expression est False.  
  
-   Si les résultats de l'expression de test est une seule valeur booléenne, cette valeur est le résultat de l'expression.  
  
-   Si l'expression de test génère une séquence d'un ou plusieurs nœuds, le résultat de l'expression est True.  
  
-   Sinon, une erreur statique est générée.  
  
 La logique **et** et **ou** opérateur est ensuite appliqué aux valeurs booléennes qui en résulte des expressions avec la sémantique de la logique standard.  
  
 La requête suivante récupère, à partir du catalogue de produits, les illustrations de face et de petit format, soit l'élément <`Picture`>, pour un modèle de produit spécifique. Notez que pour chaque document de description de produit, le catalogue peut stocker une ou plusieurs illustrations avec différents attributs tels que taille et angle de prise de vue.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $F in /PD:ProductDescription/PD:Picture[PD:Size="small"   
                                                 and PD:Angle="front"]  
     return   
         $F   
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Voici le résultat obtenu :  
  
```  
<PD:Picture   
  xmlns:PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  <PD:Angle>front</PD:Angle>  
  <PD:Size>small</PD:Size>  
  <PD:ProductPhotoID>31</PD:ProductPhotoID>  
</PD:Picture>  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
