---
title: Colonnes avec nom | Microsoft Docs
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
- names [SQL Server], columns with
ms.assetid: c994e089-4cfc-4e9b-b7fc-e74f6014b51a
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 46b121595162e60bedecfb4338fa058bb7cb8c01
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="columns-with-a-name"></a>Colonnes avec nom
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les conditions suivantes sont celles dans lesquelles les colonnes de l'ensemble de lignes avec nom sont mappées, avec respect de la casse, au document XML obtenu :  
  
-   Le nom de colonne commence par un arobase (@).  
  
-   Le nom de colonne ne commence pas par un arobase (@).  
  
-   Le nom de colonne ne commence pas par un arobase (@) et contient une barre oblique (/).  
  
-   Plusieurs colonnes partagent le même préfixe.  
  
-   Une colonne porte un nom différent.  
  
## <a name="column-name-starts-with-an-at-sign-"></a>Le nom de colonne commence par un arobase (@)  
 Si le nom de colonne commence par un arobase (@) et qu’il ne contient pas de barre oblique (/), un attribut de l’élément `row` possédant la valeur de colonne correspondante est créé. Par exemple, la requête suivante renvoie un ensemble de lignes de deux colonnes (@PmId, Name). Dans le document XML obtenu, un attribut **PmId** est ajouté à l’élément `row` correspondant et une valeur de ProductModelID lui est affectée.  
  
```  
  
SELECT ProductModelID as "@PmId",  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH   
go  
  
```  
  
 Voici le résultat obtenu :  
  
```  
<row PmId="7">  
  <Name>HL Touring Frame</Name>  
</row>  
```  
  
 À un niveau donné, les attributs doivent précéder tous les autres types de nœuds, tels que les nœuds d'élément et les nœuds de texte. La requête suivante renvoie une erreur :  
  
```  
SELECT Name,  
       ProductModelID as "@PmId"  
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH   
go  
```  
  
## <a name="column-name-does-not-start-with-an-at-sign-"></a>Le nom de colonne ne commence pas par un arobase (@)  
 Si le nom de colonne ne commence pas par un arobase (@), qu’il n’est pas l’un des tests de nœud XPath et qu’il ne contient pas de barre oblique (/), un élément XML sous-élément de l’élément de ligne, par défaut `row`, est créé.  
  
 La requête suivante spécifie le nom de colonne, qui est le résultat. Un élément enfant `result` est donc ajouté à l’élément `row`.  
  
```  
SELECT 2+2 as result  
for xml PATH  
```  
  
 Voici le résultat obtenu :  
  
```  
<row>  
  <result>4</result>  
</row>  
```  
  
 La requête suivante spécifie le nom de colonne ManuWorkCenterInformation pour le document XML retourné par la requête XQuery portant sur la colonne Instruction de type **xml**. Un élément `ManuWorkCenterInformation` est donc ajouté en tant qu’enfant de l’élément `row`.  
  
```  
SELECT   
       ProductModelID,  
       Name,  
       Instructions.query('declare namespace MI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
                /MI:root/MI:Location   
              ') as ManuWorkCenterInformation  
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH   
go  
```  
  
 Voici le résultat obtenu :  
  
```  
<row>  
  <ProductModelID>7</ProductModelID>  
  <Name>HL Touring Frame</Name>  
  <ManuWorkCenterInformation>  
    <MI:Location ...LocationID="10" ...></MI:Location>  
    <MI:Location ...LocationID="20" ...></MI:Location>  
     ...  
  </ManuWorkCenterInformation>  
</row>  
```  
  
## <a name="column-name-does-not-start-with-an-at-sign--and-contains-a-slash-mark-"></a>Le nom de colonne ne commence pas par un arobase (@) et contient une barre oblique (/)  
 Si le nom de colonne ne commence pas par un arobase (@) mais qu'il contient une barre oblique (/), il indique une hiérarchie XML. Par exemple, si le nom de colonne est « Name1/Name2/Name3.../Name***n*** », chaque partie Name***i*** représente un nom d’élément imbriqué dans l’élément de ligne actuel (avec i égal à 1) ou situé sous l’élément nommé Name***i-1***. Si la partie Name***n*** commence par « @ », elle est mappée à un attribut de Name***n-1*** .  
  
 Par exemple, la requête suivante renvoie un ID et un nom d'employé représentés sous la forme d'un élément complexe EmpName qui contient un prénom (First), un deuxième prénom (Middle) et un nom de famille (Last).  
  
```  
SELECT EmployeeID "@EmpID",   
       FirstName  "EmpName/First",   
       MiddleName "EmpName/Middle",   
       LastName   "EmpName/Last"  
FROM   HumanResources.Employee E, Person.Contact C  
WHERE  E.EmployeeID = C.ContactID  
AND    E.EmployeeID=1  
FOR XML PATH  
```  
  
 Les noms de colonnes sont utilisés comme chemin d'accès dans la construction du document XML en mode PATH. Le nom de colonne qui contient les valeurs d’ID d’employé commence par « \@ ». Ainsi, un attribut, **EmpID**, est ajouté à l’élément `row`. Le nom de toutes les autres colonnes contient une barre oblique (/) qui indique la hiérarchie. Le document XML obtenu possède l’enfant `EmpName` sous l’élément `row`, et l’enfant `EmpName` possède les éléments enfants `First`, `Middle` et `Last`.  
  
```  
<row EmpID="1">  
  <EmpName>  
    <First>Gustavo</First>  
    <Last>Achong</Last>  
  </EmpName>  
</row>  
```  
  
 Le deuxième prénom de l'employé est NULL et, par défaut, la valeur NULL correspond à l'absence de l'élément ou de l'attribut. Si vous souhaitez que des éléments soient générés pour les valeurs NULL, vous pouvez spécifier la directive ELEMENTS avec XSINIL, comme le montre la requête ci-après.  
  
```  
SELECT EmployeeID "@EmpID",   
       FirstName  "EmpName/First",   
       MiddleName "EmpName/Middle",   
       LastName   "EmpName/Last"  
FROM   HumanResources.Employee E, Person.Contact C  
WHERE  E.EmployeeID = C.ContactID  
AND    E.EmployeeID=1  
FOR XML PATH, ELEMENTS XSINIL  
```  
  
 Voici le résultat obtenu :  
  
```  
<row xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
      EmpID="1">  
  <EmpName>  
    <First>Gustavo</First>  
    <Middle xsi:nil="true" />  
    <Last>Achong</Last>  
  </EmpName>  
</row>  
```  
  
 Par défaut, le mode PATH génère des données XML centrées sur l'attribut. Par conséquent, la spécification de la directive ELEMENTS dans une requête en mode PATH est sans effet. Toutefois, comme le montre l'exemple précédent, la directive ELEMENTS, associée à XSINIL, permet de générer des éléments pour les valeurs NULL.  
  
 Outre l'ID et le nom, la requête suivante extrait l'adresse d'un employé. Conformément au chemin d’accès indiqué dans les noms des colonnes d’adresses, un élément enfant `Address` est ajouté à l’élément `row` et les détails de l’adresse sont ajoutés en tant qu’éléments enfants de l’élément `Address`.  
  
```  
SELECT EmployeeID   "@EmpID",   
       FirstName    "EmpName/First",   
       MiddleName   "EmpName/Middle",   
       LastName     "EmpName/Last",  
       AddressLine1 "Address/AddrLine1",  
       AddressLine2 "Address/AddrLIne2",  
       City         "Address/City"  
FROM   HumanResources.Employee E, Person.Contact C, Person.Address A  
WHERE  E.EmployeeID = C.ContactID  
AND    E.AddressID = A.AddressID  
AND    E.EmployeeID=1  
FOR XML PATH  
```  
  
 Voici le résultat obtenu :  
  
```  
<row EmpID="1">  
  <EmpName>  
    <First>Gustavo</First>  
    <Last>Achong</Last>  
  </EmpName>  
  <Address>  
    <AddrLine1>7726 Driftwood Drive</AddrLine1>  
    <City>Monroe</City>  
  </Address>  
</row>  
```  
  
## <a name="several-columns-share-the-same-path-prefix"></a>Plusieurs colonnes partagent le même préfixe de chemin d'accès  
 Si plusieurs colonnes partagent le même préfixe de chemin d'accès, elles sont regroupées sous le même nom. Si différents préfixes d'espace de noms sont utilisés alors qu'ils sont liés au même espace de noms, un chemin d'accès est considéré comme différent. Dans la requête précédente, les colonnes FirstName, MiddleName et LastName partagent le même préfixe EmpName. Elles sont donc ajoutées en tant qu’enfants de l’élément `EmpName`. Cela était également le cas au moment de la création de l’élément `Address` dans l’exemple précédent.  
  
## <a name="one-column-has-a-different-name"></a>Une colonne porte un nom différent  
 Si une colonne intermédiaire et portant un nom différent apparaît, elle rompt le regroupement, comme le montre la requête modifiée suivante. La requête rompt le regroupement de FirstName, MiddleName et LastName, tel que spécifié dans la requête précédente, en ajoutant des colonnes d'adresse entre les colonnes FirstName et MiddleName.  
  
```  
SELECT EmployeeID "@EmpID",   
       FirstName "EmpName/First",   
       AddressLine1 "Address/AddrLine1",  
       AddressLine2 "Address/AddrLIne2",  
       City "Address/City",  
       MiddleName "EmpName/Middle",   
       LastName "EmpName/Last"  
FROM   HumanResources.EmployeeAddress E, Person.Contact C, Person.Address A  
WHERE  E.EmployeeID = C.ContactID  
AND    E.AddressID = A.AddressID  
AND    E.EmployeeID=1  
FOR XML PATH  
```  
  
 La requête crée donc deux éléments `EmpName`. Le premier élément `EmpName` possède l’élément enfant `FirstName` et le second élément `EmpName` possède les éléments enfants `MiddleName` et `LastName`.  
  
 Voici le résultat obtenu :  
  
```  
<row EmpID="1">  
  <EmpName>  
    <First>Gustavo</First>  
  </EmpName>  
  <Address>  
    <AddrLine1>7726 Driftwood Drive</AddrLine1>  
    <City>Monroe</City>  
  </Address>  
  <EmpName>  
    <Last>Achong</Last>  
  </EmpName>  
</row>  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  
