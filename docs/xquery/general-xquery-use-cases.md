---
title: Cas d’usage de XQuery général | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
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
- XQuery, general usage cases
ms.assetid: 5187c97b-6866-474d-8bdb-a082634039cc
caps.latest.revision: 34
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 9a28080c682d40d1e08aaa96e594c96496d79026
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="general-xquery-use-cases"></a>Cas d'emploi généraux des requêtes XQuery
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique donne des exemples généraux d'emploi de requêtes XQuery.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-query-catalog-descriptions-to-find-products-and-weights"></a>A. Interrogation des descriptions d'un catalogue pour rechercher des produits et des poids  
 La requête suivante renvoie les ID de modèle de produit et les poids, le cas échéant, de la description du catalogue de produits. La requête construit du code XML se présentant sous la forme suivante :  
  
```  
<Product ProductModelID="…">  
  <Weight>…</Weight>  
</Product>  
```  
  
 Voici la requête :  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  <Product  ProductModelID="{ (/p1:ProductDescription/@ProductModelID)[1] }">  
     {   
       /p1:ProductDescription/p1:Specifications/Weight   
     }   
  </Product>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le **espace de noms** mot clé dans le prologue XQuery définit un préfixe d’espace de noms qui est utilisé dans le corps de la requête.  
  
-   Le corps de la requête construit le code XML requis.  
  
-   Dans la clause WHERE, le **exist()** méthode est utilisée pour rechercher uniquement les lignes qui contiennent les descriptions du catalogue de produits. Autrement dit, le code XML qui contient l'élément <`ProductDescription`>.  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductModelID="19"/>  
<Product ProductModelID="23"/>   
<Product ProductModelID="25"/>   
<Product ProductModelID="28"><Weight>Varies with size.</Weight></Product>  
<Product ProductModelID="34"/>  
<Product ProductModelID="35"/>  
```  
  
 La requête suivante récupère les mêmes informations, mais uniquement pour les modèles de produit dont la description du catalogue mentionne le poids, l'élément <`Weight`>, dans les caractéristiques techniques, l'élément <`Specifications`>. Cet exemple utilise WITH XMLNAMESPACES pour déclarer le préfixe pd et sa liaison d'espace de noms. De cette façon, la liaison n’est pas décrite à la fois dans le **query()** (méthode) et dans le **exist()** (méthode).  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
          <Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
                 {   
                      /pd:ProductDescription/pd:Specifications/Weight   
                 }   
          </Product>  
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Specifications//Weight ') = 1  
```  
  
 Dans la requête précédente, la **exist()** méthode de la **xml** de type de données dans la clause WHERE vérifie s’il existe un <`Weight`> élément dans le <`Specifications`> élément.  
  
### <a name="b-find-product-model-ids-for-product-models-whose-catalog-descriptions-include-front-angle-and-small-size-pictures"></a>B. Recherche des ID des modèles de produit dont les descriptions englobent des illustrations de face et de petite taille  
 La description XML du catalogue de produits comporte les illustrations du produit, l'élément <`Picture`>. Chaque illustration a plusieurs propriétés dont l'angle de prise de vue, l'élément <`Angle`>, et la taille, l'élément <`Size`>.  
  
 Pour les modèles de produit dont les descriptions du catalogue englobent des illustrations de face et de petite taille, la requête construit le code XML sous la forme suivante :  
  
```  
< Product ProductModelID="…">  
  <Picture>  
    <Angle>front</Angle>  
    <Size>small</Size>  
  </Picture>  
</Product>  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
   <pd:Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
      <Picture>  
         {  /pd:ProductDescription/pd:Picture/pd:Angle }   
         {  /pd:ProductDescription/pd:Picture/pd:Size }   
      </Picture>  
   </pd:Product>  
') as Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Picture') = 1  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Angle)[1]', 'varchar(20)')  = 'front'  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Size)[1]', 'varchar(20)')  = 'small'  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Dans la clause WHERE, le **exist()** méthode est utilisée pour récupérer uniquement les lignes qui ont produit des descriptions de catalogue avec le <`Picture`> élément.  
  
-   La clause WHERE utilise le **value()** méthode deux fois pour comparer les valeurs de la <`Size`> et <`Angle`> éléments.  
  
 Voici un extrait du résultat :  
  
```  
<p1:Product   
  xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
  ProductModelID="19">  
  <Picture>  
    <p1:Angle>front</p1:Angle>  
    <p1:Size>small</p1:Size>  
  </Picture>  
</p1:Product>  
...  
```  
  
### <a name="c-create-a-flat-list-of-the-product-model-name-and-feature-pairs-with-each-pair-enclosed-in-the-features-element"></a>C. Créer une liste plate de produit paires nom et caractéristique du modèle, chaque paire étant comprise dans la \<fonctionnalités > élément  
 Dans la description du catalogue de produits, le code XML englobe plusieurs caractéristiques du produit. Toutes ces caractéristiques sont incluses dans l'élément <`Features`>. La requête utilise [Construction XML (XQuery)](../xquery/xml-construction-xquery.md) pour construire le document XML. L'expression entre accolades est remplacée par le résultat.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  for $pd in /p1:ProductDescription,  
   $f in $pd/p1:Features/*  
  return  
   <Feature>  
     <ProductModelName> { data($pd/@ProductModelName) } </ProductModelName>  
     { $f }  
  </Feature>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   $pd/p1:Features/* renvoie uniquement les enfants du nœud d'élément <`Features`>, mais $pd/p1:Features/node() renvoie tous les nœuds, notamment les nœuds d'élément et de texte, les instructions de traitement et les commentaires.  
  
-   Les deux boucles FOR génèrent un produit cartésien à partir duquel le nom du produit et ses caractéristiques sont renvoyés.  
  
-   Le **ProductName** est un attribut. La construction XML de cette requête le renvoie sous forme d'élément.  
  
 Voici un extrait du résultat :  
  
```  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p1:Warranty   
   xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
</Feature>  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p2:Maintenance xmlns:p2="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer   
           or any AdventureWorks retail store.</p2:Description>  
    </p2:Maintenance>  
</Feature>  
...  
...      
```  
  
### <a name="d-from-the-catalog-description-of-a-product-model-list-the-product-model-name-model-id-and-features-grouped-inside-a-product-element"></a>D. À partir de la description du catalogue d’un modèle de produit, les ID de nom, le modèle de modèle de liste le produit et les fonctionnalités regroupement à l’intérieur d’un \<produit > élément  
 À l’aide des informations stockées dans la description du catalogue du modèle de produit, la requête suivante répertorie le nom du modèle de produit, ID de modèle, et les fonctionnalités sont regroupées à l’intérieur d’un \<produit > élément.  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>  
         <ProductModelName>   
           { data(/pd:ProductDescription/@ProductModelName) }   
         </ProductModelName>  
         <ProductModelID>   
           { data(/pd:ProductDescription/@ProductModelID) }   
         </ProductModelID>  
         { /pd:ProductDescription/pd:Features/* }  
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Voici un extrait du résultat :  
  
```  
<Product>  
  <ProductModelName>Mountain 100</ProductModelName>  
  <ProductModelID>19</ProductModelID>  
  <p1:Warranty>... </p1:Warranty>  
  <p2:Maintenance>...  </p2:Maintenance>  
  <p3:wheel xmlns:p3="http://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p3:wheel>  
  <p4:saddle xmlns:p4="http://www.adventure-works.com/schemas/OtherFeatures">  
    <p5:i xmlns:p5="http://www.w3.org/1999/xhtml">Anatomic design</p5:i> and made from durable leather for a full-day of riding in comfort.</p4:saddle>  
  <p6:pedal xmlns:p6="http://www.adventure-works.com/schemas/OtherFeatures">  
    <p7:b xmlns:p7="http://www.w3.org/1999/xhtml">Top-of-the-line</p7:b> clipless pedals with adjustable tension.</p6:pedal>  
   ...  
```  
  
### <a name="e-retrieve-product-model-feature-descriptions"></a>E. Récupération de la description des caractéristiques d'un modèle de produit  
 La requête suivante construit un document XML qui inclut un <`Product`> élément qui a **ProducModelID**, **ProductModelName** attributs et les premier deux caractéristiques du produit. Plus précisément, les deux premières caractéristiques du produit sont les deux premiers éléments enfants de l'élément <`Features`>. En présence d'un nombre supérieur de caractéristiques, la requête renvoie un élément vide <`There-is-more/`>.  
  
```  
SELECT CatalogDescription.query('  
declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /pd:ProductDescription/@ProductModelID }  
          { /pd:ProductDescription/@ProductModelName }   
          {  
            for $f in /pd:ProductDescription/pd:Features/*[position()<=2]  
            return  
            $f   
          }  
          {  
            if (count(/pd:ProductDescription/pd:Features/*) > 2)  
            then <there-is-more/>  
            else ()  
          }   
     </Product>          
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La structure de bouclage FOR ... La boucle RETURN récupère les deux premières caractéristiques du produit. Le **position()** fonction est utilisée pour rechercher la position des éléments dans la séquence.  
  
### <a name="f-find-element-names-from-the-product-catalog-description-that-end-with-ons"></a>F. Recherche de noms d'élément se terminant par « ons », d'après les descriptions du catalogue de produits  
 La requête suivante parcourt les descriptions du catalogue et renvoie tous les éléments de l'élément <`ProductDescription`> dont le nom se termine par « ons ».  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in /p1:ProductDescription/*[substring(local-name(.),string-length(local-name(.))-2,3)="ons"]  
      return   
          <Root>  
             { $pd }  
          </Root>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 Voici un extrait du résultat :  
  
```  
ProductModelID   Result  
-----------------------------------------  
         19        <Root>         
                     <p1:Specifications xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">          
                          ...         
                     </p1:Specifications>         
                   </Root>          
```  
  
### <a name="g-find-summary-descriptions-that-contain-the-word-aerodynamic"></a>G. Recherche des descriptions résumées qui contiennent le mot « Aerodynamic »  
 La requête suivante récupère les modèles de produit dont les descriptions du catalogue contiennent le mot « Aerodynamic » dans la description résumée :  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
          <Prod >  
             { /pd:ProductDescription/@ProductModelID }  
             { /pd:ProductDescription/pd:Summary }  
          </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('  
     contains( string( (/pd:ProductDescription/pd:Summary)[1] ),"Aerodynamic")','bit') = 1  
```  
  
 Notez que la requête SELECT spécifie **query()** et **value()** méthodes de la **xml** type de données. Par conséquent, au lieu de répéter deux fois la déclaration des espaces de noms dans deux prologues de requête différents, le préfixe pd est utilisé dans la requête, puis défini une seule fois à l'aide de WITH XMLNAMESPACES.  
  
 Notez les points suivants dans la requête précédente :  
  
-   La clause WHERE sert à récupérer uniquement les lignes où la description du catalogue contient le mot « Aerodynamic » dans l'élément <`Summary`>.  
  
-   Le **contains()** fonction permet de voir si le mot est inclus dans le texte.  
  
-   Le **value()** méthode de la **xml** type de données compare la valeur retournée par **contains()** à 1.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID Result        
-------------- ------------------------------------------  
28     <Prod ProductModelID="28">  
        <pd:Summary xmlns:pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
       <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         A TRUE multi-sport bike that offers streamlined riding and a  
         revolutionary design. Aerodynamic design lets you ride with the   
         pros, and the gearing will conquer hilly roads.</p1:p>  
       </pd:Summary>  
      </Prod>    
```  
  
### <a name="h-find-product-models-whose-catalog-descriptions-do-not-include-product-model-pictures"></a>H. Recherche de modèles de produit dont les descriptions du catalogue ne s'accompagnent pas d'illustrations  
 La requête suivante récupère les ID des modèles de produit dont les descriptions du catalogue ne s'accompagnent pas d'un élément <`Picture`>.  
  
```  
SELECT  ProductModelID  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not NULL  
AND     CatalogDescription.exist('declare namespace p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /p1:ProductDescription/p1:Picture  
') = 0  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Si le **exist()** méthode dans la clause WHERE renvoie la valeur False (0), l’ID de modèle de produit est retournée. Dans le cas contraire, aucune donnée n'est renvoyée.  
  
-   Dans la mesure où toutes les descriptions des produits comportent un élément <`Picture`>, le jeu de résultats est vide dans ce cas.  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes XQuery impliquant la hiérarchie](../xquery/xqueries-involving-hierarchy.md)   
 [Requêtes XQuery impliquant un ordre](../xquery/xqueries-involving-order.md)   
 [Gestion des requêtes XQuery des données relationnelles](../xquery/xqueries-handling-relational-data.md)   
 [Recherche de chaînes dans XQuery](../xquery/string-search-in-xquery.md)   
 [La gestion des espaces de noms dans XQuery](../xquery/handling-namespaces-in-xquery.md)   
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
