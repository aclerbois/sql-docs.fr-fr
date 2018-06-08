---
title: Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ELEMENTS XSINIL directive
- adding namespaces
- XSINIL directive
- default namespaces
- queries [XML in SQL Server], WITH XMLNAMESPACES clause
- predefined namespaces [XML in SQL Server]
- FOR XML clause, WITH XMLNAMESPACES clause
- namespaces [XML in SQL Server]
- xml data type [SQL Server], WITH XMLNAMESPACES clause
- WITH XMLNAMESPACES clause
ms.assetid: 2189cb5e-4460-46c5-a254-20c833ebbfec
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: da7122abb272847f5951d3f8d0ed5157fde82088
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-namespaces-to-queries-with-with-xmlnamespaces"></a>Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  [WITH XMLNAMESPACES (Transact-SQL)](../../t-sql/xml/with-xmlnamespaces.md) fournit une prise en charge des URI d’espace de noms de la manière suivante :  
  
-   Il rend le préfixe de l’espace de noms associé au mappage d’URI disponible lors de la [Construction de données XML à l’aide de requêtes FOR XML](../../relational-databases/xml/for-xml-sql-server.md) .  
  
-   Il met l’espace de noms associé au mappage d’URI à la disposition du contexte d’espace de noms statique des [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md).  
  
## <a name="using-with-xmlnamespaces-in-the-for-xml-queries"></a>Utilisation de WITH XMLNAMESPACES dans les requêtes FOR XML  
 WITH XMLNAMESPACES vous permet d'inclure des espaces de noms XML dans des requêtes FOR XML. Examinons, par exemple, la requête FOR XML suivante :  
  
```  
SELECT ProductID, Name, Color  
FROM   Production.Product  
WHERE  ProductID=316 or ProductID=317  
FOR XML RAW  
```  
  
 Voici le résultat obtenu :  
  
```  
<row ProductID="316" Name="Blade" />  
<row ProductID="317" Name="LL Crankarm" Color="Black" />  
  
```  
  
 Pour ajouter des espaces de noms aux données XML générées par la requête FOR XML, commencez par spécifier le préfixe d'espace de noms associé aux mappages d'URI à l'aide de la clause WITH NAMESPACES. Ensuite, utilisez les préfixes d'espace de noms pour spécifier les noms dans la requête, comme illustré dans la requête modifiée ci-dessous. Notez que la clause WITH XMLNAMESPACES spécifie le préfixe d'espace de noms (`ns1`) au mappage d'URI (`uri`). Le préfixe `ns1` est alors utilisé pour spécifier les noms d'élément et d'attribut que doit générer la requête FOR XML.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Prod'), ELEMENTS  
  
```  
  
 Le résultat XML inclut les préfixes d'espace de noms :  
  
```  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
</ns1:Prod>  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>317</ns1:ProductID>  
  <ns1:Name>LL Crankarm</ns1:Name>  
  <ns1:Color>Black</ns1:Color>  
</ns1:Prod>  
  
```  
  
 Ce qui suit s'applique à la clause WITH XMLNAMESPACES :  
  
-   Cela est pris en charge uniquement sur les modes RAW, AUTO et PATH des requêtes FOR XML. Le mode EXPLICIT n'est pas pris en charge.  
  
-   Il affecte uniquement les préfixes d’espace de noms des requêtes FOR XML et les méthodes de type de données **xml** , mais pas l’analyseur XML. Par exemple, la requête ci-dessous retourne une erreur, car le document XML ne possède pas de déclaration d'espace de noms pour le préfixe myNS.  
  
-   Les directives FOR XML, XMLSCHEMA et XMLDATA ne peuvent pas être utilisées lorsqu'une clause WITH XMLNAMESPACES est utilisée.  
  
    ```  
    CREATE TABLE T (x xml)  
    go  
    WITH XMLNAMESPACES ('http://abc' as myNS )  
    INSERT INTO T VALUES('<myNS:root/>')  
    ```  
  
## <a name="using-the-xsinil-directive"></a>Utilisation de la directive XSINIL  
 Vous ne pouvez pas définir le préfixe xsi dans la clause WITH XMLNAMESPACES si vous utilisez la directive ELEMENTS XSINIL. À la place, il est ajouté automatiquement lorsque vous utilisez ELEMENTS XSINIL. La requête ci-dessous utilise ELEMENTS XSINIL qui génère des données XML centrées sur les éléments, dans lesquelles les valeurs Null sont mappées sur les éléments dont l’attribut **xsi:nil** a la valeur True.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316   
FOR XML RAW, ELEMENTS XSINIL  
```  
  
 Voici le résultat obtenu :  
  
```  
<row xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
  <ns1:Color xsi:nil="true" />  
</row>  
```  
  
## <a name="specifying-default-namespaces"></a>Spécification d'espaces de noms par défaut  
 Au lieu de déclarer un préfixe d'espace de noms, vous pouvez déclarer un espace de noms par défaut en utilisant un mot clé DEFAULT. Dans la requête FOR XML, il liera l'espace de noms par défaut aux nœuds XML dans les données XML résultantes. Dans l'exemple ci-dessous, WITH XMLNAMESPACES définit deux préfixes d'espaces de noms qui sont définis ensemble à l'aide d'un espace de noms par défaut.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,   
                    'uri2' as ns2,  
                    DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product   
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Product'), ROOT('ns2:root'), ELEMENTS  
```  
  
 La requête FOR XML génère des données XML centrées sur les éléments. Notez que la requête utilise les deux préfixes d'espaces de noms dans l'affectation de noms aux nœuds. Dans la clause SELECT, ProductID, Name et Color ne spécifient pas de nom avec un préfixe quelconque. Par conséquent, les éléments correspondants dans les données XML résultantes appartiennent à l'espace de noms par défaut.  
  
```  
<ns2:root xmlns="uri2" xmlns:ns2="uri2" xmlns:ns1="uri1">  
  <ns1:Product>  
    <ProductID>316</ProductID>  
    <Name>Blade</Name>  
  </ns1:Product>  
  <ns1:Product>  
    <ProductID>317</ProductID>  
    <Name>LL Crankarm</Name>  
    <Color>Black</Color>  
  </ns1:Product>  
</ns2:root>  
```  
  
 La requête ci-dessous est similaire à la précédente, mais le mode FOR XML AUTO est spécifié.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,  'uri2' as ns2,DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product as "ns1:Product"  
WHERE ProductID=316 or ProductID=317  
FOR XML AUTO, ROOT('ns2:root'), ELEMENTS  
```  
  
## <a name="using-predefined-namespaces"></a>Utilisation d'espaces de noms prédéfinis  
 Lorsque vous utilisez des espaces de noms prédéfinis, à l'exception des espaces de noms xml et xsi lorsque ELEMENTS XSINIL est utilisé, vous devez spécifier explicitement la liaison avec les espaces de noms à l'aide de WITH XMLNAMESPACES. La requête ci-dessous définit explicitement le préfixe d'espace de noms associé à la liaison d'URI pour l'espace de noms prédéfini (`urn:schemas-microsoft-com:xml-sql`).  
  
```  
WITH XMLNAMESPACES ('urn:schemas-microsoft-com:xml-sql' as sql)  
SELECT 'SELECT * FROM Customers FOR XML AUTO, ROOT("a")' AS "sql:query"  
FOR XML PATH('sql:root')  
```  
  
 Voici l'ensemble de résultats. Les utilisateurs SQLXML sont familiarisés avec ce modèle XML. Pour plus d’informations, consultez [Concepts de la programmation SQLXML 4.0](../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md).  
  
```  
<sql:root xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>SELECT * FROM Customers FOR XML AUTO, ROOT("a")</sql:query>  
</sql:root>  
```  
  
 Seul le préfixe d'espace de noms xml peut être utilisé sans qu'il soit défini explicitement dans WITH XMLNAMESPACES, comme l'illustre la requête en mode PATH ci-dessous. En outre, si le préfixe est déclaré, il doit être lié à l’espace de noms http://www.w3.org/XML/1998/namespace. Les noms spécifiés dans la clause SELECT font référence au préfixe d'espace de noms xml qui n'est pas défini explicitement à l'aide de WITH XMLNAMESPACES.  
  
```  
SELECT 'en'    as "English/@xml:lang",  
       'food'  as "English",  
       'ger'   as "German/@xml:lang",  
       'Essen' as "German"  
FOR XML PATH ('Translation')  
go  
```  
  
 Les attributs @xml:lang utilisent l’espace de noms xml prédéfini. Comme XML version 1.0 ne requiert pas la déclaration explicite de la liaison d'espace de noms xml, le résultat n'inclut pas de déclaration explicite de la liaison d'espace de noms.  
  
 Voici le résultat obtenu :  
  
```  
<Translation>  
  <English xml:lang="en">food</English>  
  <German xml:lang="ger">Essen</German>  
</Translation>  
```  
  
## <a name="using-with-xmlnamespaces-with-the-xml-data-type-methods"></a>Utilisation de WITH XMLNAMESPACES avec les méthodes de type de données xml  
 Les [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md) spécifiées dans une requête SELECT, ou dans UPDATE quand il s’agit de la méthode **modify()** , doivent toutes répéter la déclaration d’espace de noms dans leur prologue. Ceci peut prendre du temps. Par exemple, la requête ci-dessous récupère les identificateurs des modèles de produits dont les descriptions de catalogue incluent une spécification. À savoir que l'élément <`Specifications`> existe.  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
```  
  
 Dans la requête précédente, les deux méthodes **query()** et **exist()** déclarent le même espace de noms dans leur prologue. Exemple :  
  
```  
declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
```  
  
 Une autre méthode consiste à déclarer WITH XMLNAMESPACES au préalable et à utiliser les préfixes d'espace de noms dans la requête. Dans ce cas, il n’est pas nécessaire que les méthodes **query()** et **exist()** incluent les déclarations d’espace de noms dans leurs prologues.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
Go  
```  
  
 Notez qu'une déclaration explicite dans le prologue XQuery remplace le préfixe d'espace de noms et l'espace de noms d'élément par défaut qui sont définis dans la clause WITH.  
  
## <a name="see-also"></a> Voir aussi  
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)   
 [WITH XMLNAMESPACES &#40;Transact-SQL&#41;](../../t-sql/xml/with-xmlnamespaces.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
