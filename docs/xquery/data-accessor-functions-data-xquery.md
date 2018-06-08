---
title: Fonction Data (XQuery) | Documents Microsoft
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
- fn:data function
- data function [XQuery]
ms.assetid: 511b5d7d-c679-4cb2-a3dd-170cc126f49d
caps.latest.revision: 31
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 74f8e5b5df2b8a6a95766576bdf5a1c8d83a4027
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-accessor-functions---data-xquery"></a>Fonctions d’accesseur de données - données (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la valeur typée pour chaque élément spécifié par *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:data ($arg as item()*) as xdt:untypedAtomic*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence d'éléments dont les valeurs typées seront renvoyées.  
  
## <a name="remarks"></a>Notes  
 Les points suivants s'appliquent aux valeurs typées :  
  
-   La valeur typée d'une valeur atomique est la valeur atomique.  
  
-   La valeur typée d'un nœud de texte est la valeur chaîne du nœud de texte.  
  
-   La valeur typée d'un commentaire est la valeur de chaîne du commentaire.  
  
-   La valeur typée d'une instruction de traitement est le contenu de l'instruction de traitement sans le nom cible de l'instruction de traitement.  
  
-   La valeur typée d'un nœud de document est sa valeur de chaîne.  
  
 Les points suivants s'appliquent aux nœuds d'attribut et d'élément :  
  
-   Si un nœud d'attribut est typé avec un type de schéma XML, sa valeur typée est la valeur typée correspondante.  
  
-   Si le nœud d’attribut n’est pas typé, sa valeur typée est égale à sa valeur de chaîne qui est retourné comme une instance de **xdt : untypedAtomic**.  
  
-   Si le nœud d’élément n’a pas été typé, sa valeur typée est égale à sa valeur de chaîne qui est retourné comme une instance de **xdt : untypedAtomic**.  
  
 Les points suivants s'appliquent aux nœuds d'élément typés :  
  
-   Si l’élément a un type de contenu simple, **data()** retourne la valeur typée de l’élément.  
  
-   Si le nœud est de type complexe, notamment xs : anyType, **data()** renvoie une erreur statique.  
  
 Bien que l’utilisation du **data()** fonction est souvent facultative, comme indiqué dans les exemples suivants, en spécifiant le **data()** fonction explicitement augmente la lisibilité de la requête. Pour plus d’informations, consultez [principes fondamentaux de XQuery](../xquery/xquery-basics.md).  
  
 Vous ne pouvez pas spécifier **data()** sur du code XML construit, comme indiqué dans l’exemple suivant :  
  
```  
declare @x xml  
set @x = ''  
select @x.query('data(<SomeNode>value</SomeNode>)')  
```  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-data-xquery-function-to-extract-typed-value-of-a-node"></a>A. Utilisation de la fonction XQuery data() pour extraire la valeur typée d'un nœud  
 L’exemple suivant illustre comment la **data()** fonction est utilisée pour récupérer les valeurs d’attribut, d’un élément et d’un nœud de texte :  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query(N'  
 for $pd in //p1:ProductDescription  
 return   
    <Root   
      ProductID = "{ data( ($pd//@ProductModelID)[1] ) }"   
      Feature =   "{ data( ($pd/p1:Features/wm:Warranty/wm:Description)[1] ) }" >  
    </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Voici le résultat obtenu :  
  
```  
<Root ProductID="19" Feature="parts and labor"/>  
```  
  
 Comme indiqué, le **data()** (fonction) est facultative lors de la construction des attributs. Si vous ne spécifiez pas le **data()** fonction, elle est implicitement sous-entendue. La requête suivante produit les mêmes résultats que la précédente :  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for $pd in //p1:ProductDescription  
         return   
          <Root    
                ProductID = "{ ($pd/@ProductModelID)[1] }"    
                Feature =   "{ ($pd/p1:Features/wm:Warranty/wm:Description)[1] }" >  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Les exemples suivants illustrent des instances dans lesquelles le **data()** fonction est nécessaire.  
  
 Dans la requête suivante, **$pd / P1 : Specifications / Material** retourne le <`Material`> élément. En outre, **données ($pd/P1 : Specifications/Material)** retourne des données caractères typées sous xdt : untypedAtomic, car <`Material`> est non typé. Lorsque l’entrée est non typée, le résultat de **data()** est de type **xdt : untypedAtomic**.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in //p1:ProductDescription  
         return   
          <Root>  
             { $pd/p1:Specifications/Material }  
             { data($pd/p1:Specifications/Material) }  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Voici le résultat obtenu :  
  
```  
<Root>  
  <Material>Almuminum Alloy</Material>Almuminum Alloy  
</Root>  
```  
  
 Dans la requête suivante, **data($pd/p1:Features/wm:Warranty)** retourne une erreur statique car <`Warranty`> est un élément de type complexe.  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
 <Root>  
   {     /p1:ProductDescription/p1:Features/wm:Warranty }  
   { data(/p1:ProductDescription/p1:Features/wm:Warranty) }  
 </Root>  
 ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID = 23  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
