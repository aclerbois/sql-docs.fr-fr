---
title: 'Exemples : utilisation du mode AUTO | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, examples
ms.assetid: 11e8d0e4-df8a-46f8-aa21-9602d4f26cad
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ac05fefc6222da075f7d02bdeb027ada6c598ed7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="examples-using-auto-mode"></a>Exemples : utilisation du mode AUTO
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les exemples suivants illustrent l'utilisation du mode AUTO. Un grand nombre de ces requêtes sont spécifiées sur les documents XML d'instructions de fabrication de bicyclettes, stockés dans la colonne Instructions de la table ProductModel dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
## <a name="example-retrieving-customer-order-and-order-detail-information"></a>Exemple : extraction des informations sur les clients, les commandes et les détails des commandes  
 Cette requête extrait les informations de client, de commande et de détail de commande relatives à un client spécifique.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       Detail.SalesOrderID, Detail.LineTotal, Detail.ProductID,   
       Product.Name,  
       Detail.OrderQty  
FROM Sales.Customer AS Cust  
INNER JOIN Sales.SalesOrderHeader AS OrderHeader   
    ON Cust.CustomerID = OrderHeader.CustomerID  
INNER JOIN Sales.SalesOrderDetail AS Detail  
    ON OrderHeader.SalesOrderID = Detail.SalesOrderID  
INNER JOIN Production.Product AS Product  
    ON Product.ProductID = Detail.ProductID  
WHERE Cust.CustomerID IN (29672, 29734)  
ORDER BY OrderHeader.CustomerID,  
         OrderHeader.SalesOrderID  
FOR XML AUTO;  
```  
  
 Étant donné que la requête identifie des alias de table `Cust`, `OrderHeader`, `Detail`et `Product` , des éléments correspondants sont générés par le mode `AUTO` . Là encore, l'ordre dans lequel les tables sont identifiées par les colonnes spécifiées dans la clause `SELECT` détermine la hiérarchie de ces éléments.  
  
 Le résultat partiel est le suivant.  
  
 `<Cust CustomerID="29672">`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="43660">`  
  
 `<Detail SalesOrderID="43660" LineTotal="874.794000" ProductID="758" OrderQty="1">`  
  
 `<Product Name="Road-450 Red, 52" />`  
  
 `</Detail>`  
  
 `<Detail SalesOrderID="43660" LineTotal="419.458900" ProductID="762" OrderQty="1">`  
  
 `<Product Name="Road-650 Red, 44" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="47660">`  
  
 `<Detail SalesOrderID="47660" LineTotal="469.794000" ProductID="765" OrderQty="1">`  
  
 `<Product Name="Road-650 Black, 58" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="49857">`  
  
 `<Detail SalesOrderID="49857" LineTotal="44.994000" ProductID="852" OrderQty="1">`  
  
 `<Product Name="Women's Tights, S" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `...`  
  
 `</Cust>`  
  
## <a name="example-specifying-group-by-and-aggregate-functions"></a>Exemple : spécification de la clause GROUP BY et de fonctions d'agrégation  
 La requête suivante renvoie des ID de client spécifiques et le nombre de commandes passées par chaque client.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT C.CustomerID, COUNT(*) AS NoOfOrders  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
On C.CustomerID = SOH.CustomerID  
GROUP BY C.CustomerID  
FOR XML AUTO;This is the partial result:  
```  
  
 `<I CustomerID="11000" NoOfOrders="3" />`  
  
 `<I CustomerID="11001" NoOfOrders="3" />`  
  
 `...`  
  
## <a name="example-specifying-computed-columns-in-auto-mode"></a>Exemple : spécification de colonnes calculées en mode AUTO  
 Cette requête renvoie des noms concaténés de clients spécifiques et les informations de commande. Étant donné que la colonne calculée est affectée au niveau le plus profond rencontré à ce stade, l'élément <`SOH`> est utilisé dans cet exemple. Les noms concaténés des clients sont ajoutés comme attributs de l'élément <`SOH`> dans le résultat.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT P.FirstName + ' ' + P.LastName AS Name,  
       SOH.SalesOrderID  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  
INNER JOIN Person.Person AS P  
    ON P.BusinessEntityID = C.PersonID  
FOR XML AUTO;  
```  
  
 Voici le résultat partiel :  
  
```  
<SOH Name="Jon Yang" SalesOrderID="43793" />  
<SOH Name="Eugene Huang" SalesOrderID="43767" />  
```  
  
 Pour extraire les éléments <`IndividualCustomer`> dont l'attribut `Name` possède chaque information d'en-tête de commande en tant que sous-élément, la requête est réécrite à l'aide d'un argument sub select. La sélection interne crée une table `IndividualCustomer` temporaire dans laquelle figure la colonne calculée qui contient les noms des différents clients. Cette table est ensuite jointe à la table `SalesOrderHeader` afin d'obtenir le résultat.  
  
 La table `Sales.Customer` stocke des informations sur les différents clients, notamment la valeur `PersonID` de chacun d'eux. Cette valeur `PersonID` permet ensuite de rechercher le nom de contact dans la table `Person.Person` .  
  
```  
SELECT IndividualCustomer.Name, SOH.SalesOrderID  
FROM (SELECT FirstName+ ' '+LastName AS Name, C.PersonID, C.CustomerID  
      FROM Sales.Customer AS C, Person.Person AS P  
      WHERE C.PersonID = P.BusinessEntityID) AS IndividualCustomer  
LEFT OUTER JOIN  Sales.SalesOrderHeader AS SOH  
   ON IndividualCustomer.CustomerID = SOH.CustomerID  
ORDER BY IndividualCustomer.CustomerID, SOH.CustomerIDFOR XML AUTO;  
```  
  
 Voici le résultat partiel :  
  
 `<IndividualCustomer Name="Jon Yang">`  
  
 `<SOH SalesOrderID="43793" />`  
  
 `<SOH SalesOrderID="51522" />`  
  
 `<SOH SalesOrderID="57418" />`  
  
 `</IndividualCustomer>`  
  
 `...`  
  
 `...`  
  
## <a name="example-returning-binary-data"></a>Exemple : renvoi de données binaires  
 Cette requête retourne une photo du produit de la table `ProductPhoto` . `ThumbNailPhoto` est une colonne **varbinary (max)** dans la table `ProductPhoto` . Par défaut, le mode `AUTO` renvoie vers les données binaires une référence, en l'occurrence une URL relative pointant vers la racine virtuelle de la base de données dans laquelle la requête est exécutée. L'attribut de clé `ProductPhotoID` doit être spécifié pour identifier l'image. Lors de l'extraction d'une référence d'image telle qu'elle apparaît dans cet exemple, la clé primaire de la table doit aussi être spécifiée dans la clause `SELECT` pour identifier une ligne de façon univoque.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Voici le résultat obtenu :  
  
 `-- result`  
  
 `<Production.ProductPhoto`  
  
 `ProductPhotoID="70"`  
  
 `ThumbNailPhoto= "dbobject/Production.ProductPhoto[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 La même requête est exécutée avec l'option `BINARY BASE64` . Elle renvoie les données binaires dans un format encodé en base 64.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO, BINARY BASE64;  
```  
  
 Voici le résultat obtenu :  
  
 `-- result`  
  
 `<Production.ProductPhoto ProductPhotoID="70" ThumbNailPhoto="Base64 encoded photo" />`  
  
 Par défaut, lorsque vous utilisez le mode AUTO pour extraire des données binaires, une référence à une URL relative pointant vers la racine virtuelle de la base de données dans laquelle la requête a été exécutée est renvoyée à la place des données binaires. Cela se produit si l'option BINARY BASE64 n'est pas spécifiée.  
  
 Lorsque le mode AUTO renvoie une référence URL aux données binaires des bases de données ne respectant pas la casse et dont un nom de table ou de colonne spécifié dans la requête ne correspond pas au nom de table ou de colonne défini dans la base de données, la requête s'exécute. Toutefois, la casse renvoyée dans la référence n'est pas cohérente. Exemple :  
  
```  
SELECT ProductPhotoID, ThumbnailPhoto  
FROM   Production.ProductPhoto   
WHERE  ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Voici le résultat obtenu :  
  
 `<Production.PRODUCTPHOTO`  
  
 `PRODUCTPHOTOID="70"`  
  
 `THUMBNAILPHOTO= "dbobject/Production.PRODUCTPHOTO[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 Cela peut poser un problème, notamment lorsque des requêtes dbobject sont exécutées sur une base de données respectant la casse. Pour l'éviter, la casse du nom de table ou de colonne spécifié dans les requêtes doit correspondre à celle du nom de table ou de colonne défini dans la base de données.  
  
## <a name="example-understanding-the-encoding"></a>Exemple : présentation de l'encodage  
 Cet exemple montre les différents encodages qui se produisent dans les résultats.  
  
 Créez cette table :  
  
```  
CREATE TABLE [Special Chars] (Col1 char(1) primary key, [Col#&2] varbinary(50));  
```  
  
 Ajoutez les données suivantes à la table :  
  
```  
INSERT INTO [Special Chars] VALUES ('&', 0x20), ('#', 0x20);  
```  
  
 Cette requête renvoie les données de la table. Le mode FOR XML AUTO est spécifié. Les données binaires sont renvoyées sous forme de référence.  
  
```  
SELECT * FROM [Special Chars] FOR XML AUTO;  
```  
  
 Voici le résultat obtenu :  
  
 `<Special_x0020_Chars`  
  
 `Col1="#"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='#']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 `<Special_x0020_Chars`  
  
 `Col1="&"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='&']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 Voici le processus pour encoder des caractères spéciaux dans le résultat :  
  
-   Dans le résultat de la requête, les caractères spéciaux XML et URL qui se trouvent dans les noms d'éléments et d'attributs renvoyés sont encodés avec la valeur hexadécimale du caractère Unicode correspondant. Dans le résultat précédent, le nom d'élément <`Special Chars`> est renvoyé comme <`Special_x0020_Chars`>. Le nom de l’attribut <`Col#&2`> est retourné en tant que <`Col_x0023__x0026_2`>. Les caractères spéciaux XML et URL sont encodés.  
  
-   Si les valeurs des éléments ou des attributs contiennent une des cinq entités de caractères XML standard (', "", \<, > et &), ces caractères XML spéciaux sont toujours encodés selon l’encodage de caractère XML. Dans le résultat précédent, la valeur `&` contenue dans la valeur de l'attribut <`Col1`> est encodée sous la forme `&`. Cependant, le caractère « # » reste sous la forme « # » parce qu'il s'agit d'un caractère XML valide et non d'un caractère XML spécial.  
  
-   Si les valeurs des éléments ou des attributs contiennent un des caractères URL spéciaux ayant donc une signification particulière dans l'URL, ils sont encodés seulement dans la valeur URL DBOBJECT et uniquement lorsque le caractère spécial fait partie d'un nom de table ou de colonne. Dans le résultat, le caractère « `#` » qui fait partie du nom de table `Col#&2` est encodé sous la forme `_x0023_ in the DBOJBECT URL`.  
  
## <a name="see-also"></a> Voir aussi  
 [UTiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  
