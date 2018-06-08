---
title: Valeur booléenne effective (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- effective Boolean value [XQuery]
- Boolean values
- XQuery, effective Boolean values
- EBV
ms.assetid: 506682b1-b6c9-45e2-aa54-7abd5844c3f1
caps.latest.revision: 17
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4ab470f5643335cd1ed26edd07aa93284bab1d47
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="effective-boolean-value-xquery"></a>Valeur booléenne effective (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Voici les valeurs booléennes effectives :  
  
-   False si l'opérande est une séquence vide ou une valeur booléenne fausse.  
  
-   Sinon, la valeur est true.  
  
 La valeur booléenne effective peut être calculée pour les expressions qui renvoient une seule valeur booléenne, une séquence de nœuds ou une séquence vide. Notez que la valeur booléenne est calculée de manière implicite lors du traitement des types d'expressions suivants :  
  
-   Expressions logiques  
  
-   Le [ne fonctionne pas](../xquery/functions-on-boolean-values-not-function.md)  
  
-   Clause WHERE d'une expression FLWOR  
  
-   [Expressions conditionnelles](../xquery/conditional-expressions-xquery.md)  
  
-   [Expressions quantifiées](../xquery/quantified-expressions-xquery.md)  
  
 Voici un exemple de valeur booléenne effective. Lorsque le **si** expression est traitée, la valeur booléenne effective de la condition est déterminée. Puisque l'expression `/a[1]` renvoie une séquence vide, la valeur booléenne effective est false. Le résultat est renvoyé au format XML avec un nœud de texte (false).  
  
```  
value is false  
DECLARE @x XML  
SET @x = '<b/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Dans l'exemple suivant, la valeur booléenne effective est true puisque l'expression renvoie une séquence non vide.  
  
```  
DECLARE @x XML  
SET @x = '<a/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Lorsque la requête réalisée sur des **xml** colonnes ou des variables, vous pouvez avoir des nœuds de type booléen. Le **data()** dans ce cas retourne une valeur booléenne. Si l'expression de la requête renvoie une valeur booléenne true, la valeur booléenne effective est true, comme le montre l'exemple qui suit. Les points suivants sont également illustrés dans l'exemple suivant :  
  
-   Une collection de schémas XML est créée. L’élément \<b > dans la collection est de type booléen.  
  
-   Typé **xml** variable est créée et interrogée.  
  
-   L'expression `data(/b[1])` renvoie une valeur booléenne true. Par conséquent, la valeur booléenne effective est true dans ce cas.  
  
-   L’expression `data(/b[2])` retourne une valeur booléenne false. Par conséquent, la valeur booléenne effective est false dans ce cas.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="s" type="string"/>  
      <element name="b" type="boolean"/>  
</schema>'  
go  
DECLARE @x XML(SC)  
SET @x = '<b>true</b><b>false</b>'  
SELECT @x.query('if (data(/b[1])) then "true" else "false"')  
SELECT @x.query('if (data(/b[2])) then "true" else "false"')  
go  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Principes fondamentaux de XQuery](../xquery/xquery-basics.md)   
 [Instruction et itération FLWOR &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)  
  
  
