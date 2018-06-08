---
title: Séquence d’Expressions (XQuery) | Documents Microsoft
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
- sequence [XQuery]
- expressions [XQuery], sequence
- filtering sequences [XQuery]
ms.assetid: 41e18b20-526b-45d2-9bd9-e3b7d7fbce4e
caps.latest.revision: 22
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 34c26b529aeaee5e9f80ecc0a1a07d3cb8cedbf4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sequence-expressions-xquery"></a>Expressions de séquence (XQuery)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge les opérateurs XQuery qui servent à construire, filtrer et combiner une séquence d'éléments. Un élément peut être une valeur atomique ou un nœud.  
  
## <a name="constructing-sequences"></a>Construction des séquences  
 Vous pouvez utiliser l'opérateur comma (virgule) pour construire une séquence qui concatène les éléments en une seule séquence.  
  
 Une séquence peut contenir des doublons. Les séquences imbriquées (une séquence dans une autre séquence) sont réduites. Par exemple, la séquence (1, 2, (3, 4, (5))) devient (1, 2, 3, 4, 5). Voici des exemples de construction de séquences.  
  
### <a name="example-a"></a>Exemple A  
 La requête suivante renvoie une séquence de cinq valeurs atomiques :  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3,4,5)')  
go  
-- result 1 2 3 4 5  
```  
  
 La requête suivante renvoie une séquence de deux nœuds :  
  
```  
-- sequence of 2 nodes  
declare @x xml  
set @x=''  
select @x.query('(<a/>, <b/>)')  
go  
-- result  
<a />  
<b />  
```  
  
 La requête suivante renvoie une erreur car vous construisez une séquence de valeurs atomiques et de nœuds. Il s'agit alors d'une séquence hétérogène qui n'est pas prise en charge.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1, 2, <a/>, <b/>)')  
go  
```  
  
### <a name="example-b"></a>Exemple B  
 La requête suivante construit une séquence de valeurs atomiques en combinant quatre séquences de longueur différente en une seule.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2),10,(),(4, 5, 6)')  
go  
-- result = 1 2 10 4 5 6  
```  
  
 Vous pouvez trier la séquence à l'aide de FLOWR et de ORDER BY :  
  
```  
declare @x xml  
set @x=''  
select @x.query('for $i in ((1,2),10,(),(4, 5, 6))  
                  order by $i  
                  return $i')  
go  
```  
  
 Vous pouvez compter les éléments dans la séquence à l’aide de la **fn :Count()** (fonction).  
  
```  
declare @x xml  
set @x=''  
select @x.query('count( (1,2,3,(),4) )')  
go  
-- result = 4  
```  
  
### <a name="example-c"></a>Exemple C  
 La requête suivante porte sur la colonne AdditionalContactInfo de la **xml** type dans la table Contact. Cette colonne stocke les informations supplémentaires des contacts tels qu'un ou plusieurs numéros de téléphone, numéros de pager et adresses supplémentaires. Le \<numéro de téléphone >, \<pager >, et d’autres nœuds peuvent apparaître n’importe où dans le document. La requête construit une séquence qui contient tous les le \<telephoneNumber > enfants du nœud de contexte, suivi par le \<radiomessagerie > enfants. Notez l'utilisation de l'opérateur de séquence comma (virgule) dans l'expression de renvoi, `($a//act:telephoneNumber, $a//act:pager)`.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
 'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo   
   return ($a//act:telephoneNumber, $a//act:pager)  
') As Result  
FROM Person.Contact  
WHERE ContactID=3  
```  
  
 Voici le résultat obtenu :  
  
```  
<act:telephoneNumber xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:pager xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>999-555-1244</act:number>  
  <act:SpecialInstructions>  
Page only in case of emergencies.  
</act:SpecialInstructions>  
</act:pager>  
```  
  
## <a name="filtering-sequences"></a>Filtrage des séquences  
 Vous pouvez filtrer la séquence renvoyée par une expression en ajoutant un prédicat à l'expression. Pour plus d’informations, consultez [Expressions de chemin d’accès &#40;XQuery&#41;](../xquery/path-expressions-xquery.md). Par exemple, la requête suivante renvoie une séquence de trois nœuds d'élément <`a`> :  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a')  
```  
  
 Voici le résultat obtenu :  
  
```  
<a attrA="1">111</a>  
<a />  
<a />  
```  
  
 Pour récupérer uniquement les éléments <`a`> dotés de l'attribut attrA, vous pouvez définir un filtre dans le prédicat. La séquence qui en résulte ne comportera qu'un seul élément <`a`>.  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a[@attrA]')  
```  
  
 Voici le résultat obtenu :  
  
```  
<a attrA="1">111</a>  
```  
  
 Pour plus d’informations sur la spécification de prédicats dans une expression de chemin d’accès, consultez [spécification de prédicats dans une étape d’Expression de chemin d’accès](../xquery/path-expressions-specifying-predicates.md).  
  
 L'exemple suivant construit une expression de séquence de sous-arborescences, puis applique un filtre à la séquence.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
<c>top level c</c>  
<d></d>  
'  
```  
  
 L'expression de `(/a, /b)` construit une séquence composée des sous-arborescences `/a` et `/b`, et filtre l'élément `<c>` à partir de la séquence résultante.  
  
```  
SELECT @x.query('  
  (/a, /b)/c  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<c>C under a</c>  
<c>C under b</c>  
```  
  
 L'exemple suivant applique un filtre de prédicat. L'expression cherche les éléments <`a`> et <`b`> contenant l'élément <`c`>.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
  
<c>top level c</c>  
<d></d>  
'  
SELECT @x.query('  
  (/a, /b)[c]  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<a>  
  <c>C under a</c>  
</a>  
<b>  
  <c>C under b</c>  
</b>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   L'expression de plage XQuery n'est pas prise en charge.  
  
-   Les séquences doivent être homogènes. Plus précisément, tous les éléments d'une séquence doivent être soit des nœuds, soit des valeurs atomiques. Le contrôle s'effectue de façon statique.  
  
-   La combinaison de séquences de nœuds à l'aide des opérateurs UNION, INTERSECT ou EXCEPT n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
