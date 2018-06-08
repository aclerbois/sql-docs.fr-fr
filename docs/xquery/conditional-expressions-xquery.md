---
title: Expressions conditionnelles (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, conditional expressions
- expressions [XQuery], conditional
- effective Boolean value [XQuery]
- if-then-else statement [XQuery]
- conditional expressions [XQuery]
- EBV
ms.assetid: b280dd96-c80f-4c51-bc06-a88d42174acb
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 3634414fb0353c9152d317c718707c3ce26ec812
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="conditional-expressions-xquery"></a>Expressions conditionnelles (XQuery)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  XQuery prend en charge l’attribut conditional suivant **if-then-else** instruction :  
  
```  
if (<expression1>)  
then  
  <expression2>  
else  
  <expression3>  
```  
  
 Suivant la valeur booléenne effective de `expression1`, `expression2` ou `expression3` est évaluée. Par exemple :  
  
-   Si l'expression de test, `expression1`, aboutit à une séquence vide, le résultat est False.  
  
-   Si l'expression de test, `expression1`, aboutit à une valeur booléenne simple, cette valeur est le résultat de l'expression.  
  
-   Si l'expression de test, `expression1`, aboutit à une séquence d'un ou plusieurs nœuds, le résultat de l'expression est True.  
  
-   Sinon, une erreur statique est générée.  
  
 En outre, notez les points suivants :  
  
-   L'expression de test doit figurer entre parenthèses.  
  
-   Le **else** expression est requise. Si vous n'en avez pas besoin, vous pouvez renvoyer « ( ) », comme le montrent les exemples de cette rubrique.  
  
 Par exemple, la requête suivante porte sur la **xml** variable de type. Le **si** condition teste la valeur de la variable SQL (@v) à l’intérieur de l’expression XQuery à l’aide de la [:variable()](../xquery/xquery-extension-functions-sql-variable.md) fonction d’extension. Si la valeur de la variable est « FirstName », elle renvoie l'élément <`FirstName`>. Sinon, elle renvoie l'élément <`LastName`>.  
  
```  
declare @x xml  
declare @v varchar(20)  
set @v='FirstName'  
set @x='  
<ROOT rootID="2">  
  <FirstName>fname</FirstName>  
  <LastName>lname</LastName>  
</ROOT>'  
SELECT @x.query('  
if ( sql:variable("@v")="FirstName" ) then  
  /ROOT/FirstName  
 else  
   /ROOT/LastName  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<FirstName>fname</FirstName>  
```  
  
 La requête suivante extrait les deux premières descriptions de fonctionnalités de la description de catalogue de produit d'un modèle de produit spécifique. Si le document comporte davantage de fonctionnalités, elle ajoute un élément <`there-is-more`> avec un contenu vide.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /p1:ProductDescription/@ProductModelID }  
          { /p1:ProductDescription/@ProductModelName }   
          {  
            for $f in /p1:ProductDescription/p1:Features/*[position()\<=2]  
            return  
            $f   
          }  
          {  
            if (count(/p1:ProductDescription/p1:Features/*) > 2)  
            then \<there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Dans la requête précédente, la condition dans la **si** expression vérifie s’il existe plus de deux éléments enfants <`Features`>. Si tel est le cas, elle renvoie l'élément `\<there-is-more/>` dans le résultat.  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  \<p1:Warranty xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p1:WarrantyPeriod>3 years\</p1:WarrantyPeriod>  
    \<p1:Description>parts and labor\</p1:Description>  
  \</p1:Warranty>  
  \<p2:Maintenance xmlns:p2="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p2:NoOfYears>10 years\</p2:NoOfYears>  
    \<p2:Description>maintenance contract available through your dealer or any AdventureWorks retail store.\</p2:Description>  
  \</p2:Maintenance>  
  \<there-is-more />  
</Product>  
```  
  
 Dans la requête suivante, un élément <`Location`> doté d'un attribut LocationID est renvoyé si le site de production ne spécifie pas les heures de préparation (@SetupHours).  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in //AWMI:root/AWMI:Location  
        return  
        if ( $WC[not(@SetupHours)] )  
        then  
          <WorkCenterLocation>  
             { $WC/@LocationID }   
          </WorkCenterLocation>  
         else  
           ()  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Voici le résultat obtenu :  
  
```  
<WorkCenterLocation LocationID="30" />  
<WorkCenterLocation LocationID="45" />  
<WorkCenterLocation LocationID="60" />  
```  
  
 Cette requête peut être écrite sans la **si** clause, comme indiqué dans l’exemple suivant :  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in //AWMI:root/AWMI:Location[not(@SetupHours)]   
        return  
          <Location>  
             { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
