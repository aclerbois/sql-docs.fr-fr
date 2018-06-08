---
title: Quantification des Expressions (XQuery) | Documents Microsoft
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
- universal quantifiers
- effective Boolean value [XQuery]
- quantified expressions [XQuery]
- XQuery, quantified expressions
- every expression [XQuery]
- existential quantifiers [XQuery]
- some expression [XQuery]
- EBV
- expressions [XQuery], quantifiers
ms.assetid: a3a75a6c-8f67-4923-8406-1ada546c817f
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 9fa6c22aafdd0279c9205f36902bac1ef18c77df
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="quantified-expressions-xquery"></a>Expressions quantifiées (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Les quantificateurs existentiels et universels spécifient différentes sémantiques pour les opérateurs booléens appliqués à deux séquences. Cela est illustré par le tableau suivant.  
  
 *Quantificateur existentiel*  
 Soient deux séquences ; si un élément de la première séquence a une correspondance dans la seconde séquence, en fonction de l'opérateur de comparaison utilisé, la valeur renvoyée est True.  
  
 *Quantificateur universel*  
 Soient deux séquences ; si chaque élément de la première séquence a une correspondance dans la seconde séquence, la valeur renvoyée est True.  
  
 XQuery prend en charge les expressions quantifiées de la forme suivante :  
  
```  
( some | every ) <variable> in <Expression> (,…) satisfies <Expression>  
```  
  
 Vous pouvez utiliser ces expressions dans une requête pour appliquer explicitement une quantification existentielle ou universelle à une expression sur une ou plusieurs séquences. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l'expression de la clause `satisfies` doit aboutir à l'un des résultats suivants : une séquence de nœuds, une séquence vide ou une valeur booléenne. La valeur booléenne effective du résultat de cette expression est utilisée dans la quantification. La quantification existentielle qui utilise **certains** renvoie la valeur True si au moins une des valeurs liées par le quantificateur possède un résultat True dans l’expression de satisfaction. La quantification universelle qui utilise **chaque** doit avoir la valeur True pour toutes les valeurs liées par le quantificateur.  
  
 Par exemple, la requête suivante vérifie chaque \<emplacement > élément pour voir s’il dispose d’un attribut LocationID.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        if (every $WC in //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID)  
        then  
             <Result>All work centers have workcenterLocation ID</Result>  
         else  
             <Result>Not all work centers have workcenterLocation ID</Result>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Étant donné que LocationID est un attribut obligatoire de le \<emplacement > élément, vous obtenez le résultat attendu :  
  
```  
<Result>All work centers have Location ID</Result>   
```  
  
 Au lieu d’utiliser le [méthode query()](../t-sql/xml/query-method-xml-data-type.md), vous pouvez utiliser la [méthode value()](../t-sql/xml/value-method-xml-data-type.md) pour renvoyer les résultats à l’univers relationnel, comme indiqué dans la requête suivante. La requête renvoie True si tous les sites de production possèdent l'attribut LocationID. Sinon, elle renvoie False.  
  
```  
SELECT Instructions.value('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        every $WC in  //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID',   
  'nvarchar(10)') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 La requête suivante vérifie si l'une des images de produit est petite. Dans le document XML du catalogue des produits, différents angles sont stockés pour chaque image de produit de taille différente. Vous pouvez faire en sorte que chaque document XML de catalogue de produits comprenne au moins une image de petite taille. La requête suivante effectue cette opération :  
  
```  
SELECT ProductModelID, CatalogDescription.value('  
     declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     some $F in /PD:ProductDescription/PD:Picture  
        satisfies $F/PD:Size="small"', 'nvarchar(20)') as SmallPicturesStored  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Voici un extrait du résultat :  
  
```  
ProductModelID SmallPicturesStored   
-------------- --------------------  
19             true        
```  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   L'assertion de type n'est pas prise en charge pour la liaison de la variable dans les expressions quantifiées.  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
