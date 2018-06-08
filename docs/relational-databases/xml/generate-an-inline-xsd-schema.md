---
title: Générer un schéma XSD en ligne | Microsoft Docs
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
- XSD schemas [SQL Server]
- XMLSCHEMA option
- schemas [SQL Server], XML
- XDR schemas
- FOR XML clause, inline XSD schema generation
- inline XSD schema generation [SQL Server]
- XMLDATA option
ms.assetid: 04b35145-1cca-45f4-9eb7-990abf2e647d
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e2f360663435f0f9388fa4aca5c0391e540d08e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-an-inline-xsd-schema"></a>Générer un schéma XSD en ligne
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Dans une clause FOR XML, vous pouvez demander que votre requête retourne un schéma en ligne avec les résultats de la requête. Pour obtenir un schéma XDR, utilisez le mot clé XMLDATA dans la clause FOR XML. Pour obtenir un schéma XSD, utilisez le mot clé XMLSCHEMA.  
  
 Cette rubrique décrit le mot clé XMLSCHEMA et explique la structure du schéma XSD en ligne résultant. Les limites suivantes sont à respecter lorsque vous demandez des schémas en ligne :  
  
-   Vous pouvez spécifier XMLSCHEMA seulement en mode RAW et AUTO, pas en mode EXPLICIT.  
  
-   Si une requête FOR XML imbriquée spécifie la directive TYPE, le résultat de la requête est de type **xml** et ce résultat est traité comme une instance de données XML non typées. Pour plus d’informations, consultez [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md).  
  
 Lorsque vous spécifiez XMLSCHEMA dans une requête FOR XML, vous recevez à la fois un schéma et des données XML, le résultat de la requête. Chaque élément de niveau supérieur des données fait référence au schéma précédent en utilisant une déclaration d'espace de noms qui, à son tour, fait référence à l'espace de noms cible du schéma en ligne.  
  
 Exemple :  
  
```  
<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">  
  <xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="http://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="Production.ProductModel">  
    <xsd:complexType>  
      <xsd:attribute name="ProductModelID" type="sqltypes:int" use="required" />  
      <xsd:attribute name="Name" use="required">  
        <xsd:simpleType sqltypes:sqlTypeAlias="[AdventureWorks2012].[dbo].[Name]">  
          <xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033" sqltypes:sqlCompareOptions="IgnoreCase IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">  
            <xsd:maxLength value="50" />  
          </xsd:restriction>  
        </xsd:simpleType>  
      </xsd:attribute>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
<Production.ProductModel xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" ProductModelID="1" Name="Classic Vest" />  
```  
  
 Le résultat inclut un schéma XML et le résultat XML. L'élément de niveau supérieur <`ProductModel`> dans le résultat fait référence au schéma en utilisant la déclaration d'espace de noms par défaut, xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" .  
  
 La partie schéma du résultat peut contenir plusieurs documents de schéma qui décrivent plusieurs espaces de noms. Au minimum, le deux documents de schéma ci-dessous sont retournés :  
  
-   Un document de schéma pour l’espace de noms **Sqltypes** et pour lequel les types SQL de base sont retournés.  
  
-   Un autre document de schéma qui décrit la forme du résultat de la requête FOR XML.  
  
 En outre, si des types de données **xml** typées sont inclus dans le résultat de la requête, les schémas associés avec ces types de données **xml** typés sont inclus.  
  
 L'espace de noms cible du document de schéma qui décrit la forme du résultat FOR XML contient une partie fixe et une partie numérique incrémentée automatiquement. Le format de cet espace de noms est illustré dans ce qui suit, où *n* est un entier positif. Par exemple, dans la requête précédente, urn:schemas-microsoft-com:sql:SqlRowSet1 est l'espace de noms cible.  
  
```  
urn:schemas-microsoft-com:sql:SqlRowSetn  
```  
  
 La modification dans les espaces de noms cibles du résultat qui s'est produite d'une exécution à l'autre peut ne pas être souhaitée. Par exemple, si vous interrogez les données XML résultantes, la modification dans l'espace de noms cible requiert que vous mettiez à jour votre requête. Vous pouvez éventuellement spécifier un espace de noms cible lorsque l'option XMLSCHEMA est ajoutée dans la clause FOR XML. Les données XML résultantes incluront l'espace de noms spécifié et resteront les mêmes, quel que soit le nombre de fois que vous exécutez la requête.  
  
```  
SELECT ProductModelID, Name  
FROM   Production.ProductModel  
WHERE ProductModelID=1  
FOR XML AUTO, XMLSCHEMA ('MyURI')  
```  
  
## <a name="entity-elements"></a>Éléments d'entité  
 Pour pouvoir analyser les détails de la structure de schéma XSD générée pour le résultat de la requête, l'élément d'entité doit être décrit au préalable  
  
 Un élément d'entité dans les données XML retournées par une requête FOR XML est un élément généré à partir d'une table et non pas à partir d'une colonne. Par exemple, la requête FOR XML ci-dessous retourne des informations de contact à partir de la table `Person` dans la base de données `AdventureWorks2012` .  
  
```  
SELECT BusinessEntityID, FirstName  
FROM Person.Person  
WHERE BusinessEntityID = 1  
FOR XML AUTO, ELEMENTS  
```  
  
 Voici le résultat obtenu :  
  
 `<Person>`  
  
 `<BusinessEntityID>1</BusinessEntityID>`  
  
 `<FirstName>Ken</FirstName>`  
  
 `</Person>`  
  
 Dans ce résultat, <`Person`> est l'élément d'entité. Plusieurs éléments d'entité peuvent exister dans le résultat XML et chacun de ces éléments possède une déclaration globale dans le schéma XSD en ligne. Par exemple, la requête ci-dessous récupère l'en-tête de commande et les informations détaillées d'une commande spécifique.  
  
```  
SELECT  SalesOrderHeader.SalesOrderID, ProductID, OrderQty  
FROM    Sales.SalesOrderHeader, Sales.SalesOrderDetail  
WHERE   SalesOrderHeader.SalesOrderID = SalesOrderDetail.SalesOrderID  
AND     SalesOrderHeader.SalesOrderID=5001  
FOR XML AUTO, ELEMENTS, XMLSCHEMA  
```  
  
 Comme la requête spécifie la directive ELEMENTS, les données XML résultantes sont centrées sur les éléments. La requête spécifie également la directive XMLSCHEMA. Par conséquent, un schéma XSD en ligne est retourné. Voici le résultat obtenu :  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="http://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="Sales.SalesOrderHeader">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="SalesOrderID" type="sqltypes:int" />`  
  
 `<xsd:element ref="schema:Sales.SalesOrderDetail" minOccurs="0" maxOccurs="unbounded" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `<xsd:element name="Sales.SalesOrderDetail">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="OrderQty" type="sqltypes:smallint" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 Notez les points suivants dans la requête précédente :  
  
-   Dans le résultat, <`SalesOrderHeader`> et <`SalesOrderDetail`> sont des éléments d'entité. Pour cette raison, ils sont déclarés globalement dans le schéma. Ainsi, la déclaration apparaît au plus haut niveau au sein de l'élément <`Schema`>.  
  
-   <`SalesOrderID`>, <`ProductID`> et <`OrderQty`> ne sont pas des éléments d'entité, car ils sont mappés avec des colonnes. Les données de colonne sont retournées en tant qu'éléments dans les données XML, en raison de la directive ELEMENTS. Elles sont mappées avec les éléments locaux du type complexe de l'élément d'entité. Notez que si la directive ELEMENTS n'est pas spécifiée, les valeurs `SalesOrderID`, `ProductID` et `OrderQty` sont mappées avec les attributs locaux du type complexe de l'élément d'entité correspondant.  
  
## <a name="attribute-name-clashes"></a>Conflits de noms d'attributs  
 La discussion suivante est basée sur les tables `CustOrder` et `CustOrderDetail` . Pour tester les exemples suivants, créez ces tables et ajoutez vos propres données d'exemple :  
  
```  
CREATE TABLE CustOrder (OrderID int primary key, CustomerID int)  
GO  
CREATE TABLE CustOrderDetail (OrderID int, ProductID int, Qty int)  
GO  
```  
  
 Dans FOR XML, le même nom est parfois utilisé pour indiquer des propriétés ou des attributs différents. Par exemple, la requête ci-dessous en mode RAW centré sur les attributs génère deux attributs du même nom, OrderID. Cela génère une erreur.  
  
```  
SELECT CustOrder.OrderID,   
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
FROM   dbo.CustOrder, dbo.CustOrderDetail  
WHERE  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA  
```  
  
 Toutefois, comme il est possible de disposer de deux éléments du même nom, vous pouvez éliminer le problème en ajoutant la directive ELEMENTS :  
  
```  
SELECT CustOrder.OrderID,  
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
from   dbo.CustOrder, dbo.CustOrderDetail  
where  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA, ELEMENTS  
```  
  
 Voici l'ensemble de résultats. Notez dans le schéma XSD en ligne que l'élément OrderID est défini deux fois. Dans une des déclarations, minOccurs a la valeur 0, ce qui correspond à l'OrderID de la table CustOrderDetail, et l'autre déclaration est mappée avec la colonne clé primaire OrderID de la table `CustOrder` où minOccurs a la valeur 1 par défaut.  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="http://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" />`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" minOccurs="0" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
## <a name="element-name-clashes"></a>Conflits de noms d'éléments  
 Dans FOR XML, le même nom peut être utilisé pour indiquer deux sous-éléments. Par exemple, la requête ci-dessous récupère les valeurs ListPrice et DealerPrice des produits, mais la requête spécifie le même alias, Price, pour ces deux colonnes. Par conséquent, l'ensemble de lignes résultant possèdera deux colonnes du même nom.  
  
### <a name="case-1-both-subelements-are-nonkey-columns-of-the-same-type-and-can-be-null"></a>Cas 1 : les deux sous-éléments sont des colonnes non-clés du même type et ils acceptent la valeur NULL  
 Dans la requête ci-dessous, les deux sous-éléments sont des colonnes non-clés du même type et ils acceptent la valeur NULL.  
  
```  
DROP TABLE T  
go  
CREATE TABLE T (ProductID int primary key, ListPrice money, DealerPrice money)  
go  
INSERT INTO T values (1, 1.25, null)  
go  
  
SELECT ProductID, ListPrice Price, DealerPrice Price  
FROM   T  
for    XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Ceci représente le schéma XML correspondant généré. Seule une fraction du schéma XSD en ligne est indiquée :  
  
 `…`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" minOccurs="0" maxOccurs="2" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `</row>`  
  
 Notez les éléments suivants dans le schéma XSD en ligne :  
  
-   ListPrice et DealerPrice sont du même type, `money`, et les deux acceptent la valeur NULL dans la table. Par conséquent, comme ils peuvent ne pas être retournés dans les données XML résultantes, il existe un seul élément enfant <`Price`> dans la déclaration de type complexe de l'élément <`row`> pour lequel minOccurs=0 et maxOccurs=2.  
  
-   Dans le résultat, comme `DealerPrice` a une valeur NULL dans la table, seul `ListPrice` est retourné comme élément <`Price`>. Si vous ajoutez le paramètre `XSINIL` à la directive ELEMENTS, vous recevrez les deux éléments pour lesquels `xsi:nil` a pour valeur TRUE pour l’élément <`Price`> qui correspond à DealerPrice. Vous recevrez aussi deux éléments enfants <`Price`> dans la définition de type complexe <`row`> dans le schéma XSD en ligne avec l'attribut `nillable` ayant la valeur TRUE pour les deux. Ce fragment est un résultat partiel :  
  
 `…`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `<Price xsi:nil="true" />`  
  
 `</row>`  
  
### <a name="case-2-one-key-and-one-nonkey-column-of-the-same-type"></a>Cas 2 : une colonne clé et une colonne non-clé du même type  
 La requête ci-dessous illustre une colonne clé et une colonne non-clé du même type.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 int, Col3 nvarchar(20))  
go  
INSERT INTO T VALUES (1, 1, 'test')  
go   
```  
  
 La requête ci-dessous exécutée sur la table **T** spécifie le même alias pour Col1 et Col2, où Col1 est une clé primaire et ne peut pas avoir la valeur NULL, alors que Col2 accepte la valeur NULL. Cela génère deux éléments frères qui sont des enfants de l'élément <`row`>.  
  
```  
SELECT Col1 as Col, Col2 as Col, Col3  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Voici l'ensemble de résultats. Seul un fragment du schéma XSD en ligne est indiqué :  
  
 `…`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="Col3" minOccurs="0">`  
  
 `<xsd:simpleType>`  
  
 `<xsd:restriction base="sqltypes:nvarchar"`  
  
 `sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth"`  
  
 `sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `</xsd:element>`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col>1</Col>`  
  
 `<Col>1</Col>`  
  
 `<Col3>test</Col3>`  
  
 `</row>`  
  
 Notez dans le schéma XSD en ligne que l'élément <`Col`> correspondant à Col2 a une valeur minOccurs égale à 0.  
  
### <a name="case-3-both-elements-of-different-types-and-corresponding-columns-can-be-null"></a>Cas 3 : les deux éléments sont de types différents et les colonnes correspondantes acceptent la valeur NULL  
 La requête ci-dessous est spécifiée sur l'exemple de table défini dans le cas 2 :  
  
```  
SELECT Col1, Col2 as Col, Col3 as Col  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Dans la requête ci-dessous, Col2 et Col3 obtiennent les mêmes alias. Cela génère deux éléments frères du même nom, qui sont tous les deux des enfants de l'élément <`raw`> dans le résultat. Ces deux colonnes sont de types différents et acceptent la valeur NULL. Voici l'ensemble de résultats. Seul le schéma XSD en ligne partielle est indiqué.  
  
 `…`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="http://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:simpleType name="Col1">`  
  
 `<xsd:restriction base="sqltypes:int" />`  
  
 `</xsd:simpleType>`  
  
 `<xsd:simpleType name="Col2">`  
  
 `<xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col1" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" minOccurs="0" maxOccurs="2" type="xsd:anySimpleType" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col1>1</Col1>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col1">1</Col>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col2">test</Col>`  
  
 `</row>`  
  
 Notez les éléments suivants dans le schéma XSD en ligne :  
  
-   Comme Col2 et Col3 acceptent la valeur NULL, la déclaration d'élément <`Col`> spécifie une valeur minOccurs égale à 0 et une valeur maxOccurs égale à 2.  
  
-   Comme les deux éléments <`Col`> sont frères, le schéma contient une seule déclaration d'élément. En outre, comme les deux éléments sont également de types différents, bien qu'ils soient tous les deux d'un type simple, le type de l'élément dans le schéma est `xsd:anySimpleType`. Dans le résultat, chaque type d'instance est identifié par l'attribut `xsi:type`.  
  
-   Dans le résultat, chaque instance de l'élément <`Col`> fait référence à son type d'instance en utilisant l'attribut `xsi:type`.  
  
  
