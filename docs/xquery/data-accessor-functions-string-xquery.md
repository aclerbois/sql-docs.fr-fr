---
title: Fonction String (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
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
- string function
- fn:string function
ms.assetid: 7baa2959-9340-429b-ad53-3df03d8e13fc
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: c20973cdaa3b3d80124a9713a104d7294d6c20f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-accessor-functions---string-xquery"></a>Fonctions d’accesseur de données - string (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la valeur de *$arg* représenté sous forme de chaîne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:string() as xs:string  
fn:string($arg as item()?) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nœud ou valeur atomique.  
  
## <a name="remarks"></a>Notes  
  
-   Si *$arg* est la séquence vide, la chaîne de longueur nulle est retournée.  
  
-   Si *$arg* est un nœud, la fonction retourne la valeur de chaîne du nœud qui est obtenue à l’aide de l’accesseur de la valeur de chaîne. Celui-ci est défini dans les spécifications W3C XQuery 1.0 et XPath 2.0 Data Model.  
  
-   Si *$arg* est une valeur atomique, la fonction retourne la même chaîne est retournée par l’expression de cast en tant que **xs : String**, *$arg*, sauf si spécifié autrement.  
  
-   Si le type de *$arg* est **xs : anyURI**, l’URI est converti en une chaîne sans séquence d’échappement des caractères spéciaux.  
  
-   Dans cette implémentation, **fn :String()** sans argument peut uniquement être utilisé dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets ([ ]).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-string-function"></a>A. Utilisation de la fonction string  
 La requête suivante extrait le nœud d'élément enfant <`Features`> de l'élément <`ProductDescription`>.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 /PD:ProductDescription/PD:Features  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Voici le résultat partiel :  
  
```  
<PD:Features xmlns:PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   These are the product highlights.   
   <p1:Warranty xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 years</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
   </p1:Warranty>  
       ...  
</PD:Features>  
```  
  
 Si vous spécifiez la **string()** (fonction), vous recevez la valeur de chaîne du nœud spécifié.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 string(/PD:ProductDescription[1]/PD:Features[1])  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Le résultat partiel est le suivant.  
  
```  
These are the product highlights.   
3 yearsparts and labor...    
```  
  
### <a name="b-using-the-string-function-on-various-nodes"></a>B. Utilisation de la fonction string sur différents nœuds  
 Dans l'exemple suivant, une instance XML est affectée à une variable de type xml. Les requêtes sont spécifiées pour illustrer le résultat de l’application **string()** à différents nœuds.  
  
```  
declare @x xml  
set @x = '<?xml version="1.0" encoding="UTF-8" ?>  
<!--  This is a comment -->  
<root>  
  <a>10</a>  
just text  
  <b attr="x">20</b>  
</root>  
'  
```  
  
 La requête suivante extrait la valeur de chaîne du nœud de document. Cette valeur est le fruit de la concaténation de la valeur de chaîne de tous ses nœuds de texte descendants.  
  
```  
select @x.query('string(/)')  
```  
  
 Voici le résultat obtenu :  
  
```  
This is a comment 10  
just text  
 20  
```  
  
 La requête suivante essaie d'extraire la valeur de chaîne d'un nœud d'instruction de traitement. Le résultat est une séquence vide, car il ne contient pas de nœud de texte.  
  
```  
select @x.query('string(/processing-instruction()[1])')  
```  
  
 La requête suivante extrait la valeur de chaîne du nœud de commentaire et renvoie le nœud de texte.  
  
```  
select @x.query('string(/comment()[1])')  
```  
  
 Voici le résultat obtenu :  
  
```  
This is a comment   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
