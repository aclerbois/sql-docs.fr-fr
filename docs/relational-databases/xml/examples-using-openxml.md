---
title: 'Exemples : Utilisation de OPENXML | Microsoft Docs'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ColPattern [XML in SQL Server]
- XML [SQL Server], mapping data
- OPENXML statement, about OPENXML statement
- overflow in XML document [SQL Server]
- mapping XML data [SQL Server]
- combining attribute-centric and element centric mapping
- unconsumed data
- attribute-centric mapping
- column patterns [XML in SQL Server]
- XML [SQL Server], overflow handling
- row patterns [XML in SQL Server]
- rowpattern [XML in SQL Server]
- flags parameter
- element-centric mapping [SQL Server]
- edge tables
ms.assetid: 689297f3-adb0-4d8d-bf62-cfda26210164
caps.latest.revision: 36
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 33f794f164a1fbd63ce65289c36b30391a87587b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="examples-using-openxml"></a>Exemples : Utilisation de OPENXML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les exemples de cette rubrique montrent comment utiliser OPENXML pour créer une vue d'un ensemble de lignes d'un document XML. Pour plus d’informations sur la syntaxe d’OPENXML, consultez [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md). Les exemples montrent tous les aspects de OPENXML, sauf la spécification des métapropriétés. Pour plus d’informations sur la spécification de métapropriétés dans OPENXML, consultez [Spécifier des métapropriétés dans OPENXML](../../relational-databases/xml/specify-metaproperties-in-openxml.md).  
  
## <a name="examples"></a>Exemples  
 Lors de l’extraction de données, *rowpattern* sert à identifier les nœuds du document XML qui déterminent les lignes. De plus, *rowpattern* est exprimé dans le langage du modèle XPath utilisé dans la mise en œuvre XPath de MSXML. Par exemple, si le modèle s’achève au niveau d’un élément ou d’un attribut, une ligne est créée pour chaque nœud d’élément ou d’attribut sélectionné par *rowpattern*.  
  
 La valeur *flags* fournit le mappage par défaut. Si aucun *ColPattern* n’est spécifié dans *SchemaDeclaration*, le mappage défini par la valeur *flags* est appliqué. La valeur *flags* est ignorée si *ColPattern* est spécifié dans *SchemaDeclaration*. Le paramètre *ColPattern* spécifié détermine le mappage (centré sur l’attribut ou sur l’élément), ainsi que la façon dont les données en excès ou non consommées sont traitées.  
  
### <a name="a-executing-a-simple-select-statement-with-openxml"></a>A. Exécution d'une instruction SELECT simple avec OPENXML  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`>, <`Order`> et <`OrderDetail`>. L’instruction OPENXML extrait des informations sur les clients dans un ensemble de lignes à deux colonnes (**CustomerID** et **ContactName**) à partir du document XML.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/ROOT/Customer) identifie les nœuds <`Customer`> à traiter.  
  
-   Le paramètre *flags* prend la valeur **1** pour indiquer qu’il s’agit d’un mappage centré sur l’attribut. Ainsi, les attributs XML sont mappés sur les colonnes de l’ensemble de lignes défini dans *SchemaDeclaration*.  
  
-   Dans *SchemaDeclaration*(dans la clause WITH), les valeurs *ColName* spécifiées correspondent aux noms d’attributs XML. Par conséquent, le paramètre *ColPattern* n’est pas spécifié dans *SchemaDeclaration*.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans l'ensemble de lignes fourni par OPENXML.  
  
```  
DECLARE @DocHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @DocHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@DocHandle, '/ROOT/Customer',1)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @DocHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Dans la mesure où les éléments <`Customer`> n’ont pas de sous-éléments, si la même instruction SELECT s’exécute avec une valeur *flags* de **2**, pour indiquer qu’il s’agit d’un mappage centré sur l’élément, les valeurs de **CustomerID** et de **ContactName** pour les deux clients sont renvoyées comme NULL.  
  
 @xmlDocument peut également être de type **xml** ou de type **(n)varchar(max)**.  
  
 Si, dans le document XML, les éléments <`CustomerID`> et <`ContactName`> sont des sous-éléments, le mappage centré sur l'élément extrait les valeurs.  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer>  
   <CustomerID>VINET</CustomerID>  
   <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer>     
   <CustomerID>LILAS</CustomerID>  
   <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT    *  
FROM      OPENXML (@XmlDocumentHandle, '/ROOT/Customer',2)  
           WITH (CustomerID  varchar(10),  
                 ContactName varchar(20))  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Remarquez que le descripteur de document renvoyé par **sp_xml_preparedocument** n’est valide que pendant la durée du traitement et non pendant l’ensemble de la session.  
  
### <a name="b-specifying-colpattern-for-mapping-between-rowset-columns-and-the-xml-attributes-and-elements"></a>B. Spécification de ColPattern pour effectuer un mappage entre les colonnes d'un ensemble de lignes et les attributs/éléments XML  
 Cet exemple montre comment le modèle XPath est défini dans le paramètre facultatif *ColPattern* pour fournir un mappage entre les colonnes d’un ensemble de lignes et les attributs/éléments XML.  
  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`>, <`Order`> et <`OrderDetail`>. L’instruction OPENXML extrait des informations sur les clients et les commandes sous la forme d’un ensemble de lignes (**CustomerID**, **OrderDate**, **ProdID** et **Qty**) à partir du document XML.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/ROOT/Customer/Order/OrderDetail) identifie les nœuds <`OrderDetail`> à traiter.  
  
 À titre d’illustration, le paramètre *flags* prend la valeur **2** pour indiquer qu’il s’agit d’un mappage centré sur l’élément. Toutefois, le mappage spécifié dans *ColPattern* remplace ce dernier. Autrement dit, le modèle XPath spécifié dans *ColPattern* mappe les colonnes de l’ensemble de lignes sur des attributs. Il en résulte un mappage centré sur l'attribut.  
  
 Dans *SchemaDeclaration*(dans la clause WITH), le paramètre *ColPattern* est également spécifié avec les paramètres *ColName* et *ColType* . Le paramètre facultatif *ColPattern* correspond au modèle XPath spécifié et définit les éléments suivants :  
  
-   Les colonnes **OrderID**, **CustomerID** et **OrderDate** de l’ensemble de lignes sont mappées sur les attributs du parent des nœuds identifiés par *rowpattern* et *rowpattern* identifie les nœuds <`OrderDetail`>. Par conséquent, les colonnes **CustomerID** et **OrderDate** sont mappées sur les attributs **CustomerID** et **OrderDate** de l’élément <`Order`>.  
  
-   Les colonnes **ProdID** et **Qty** de l’ensemble de lignes sont mappées sur les attributs **ProductID** et **Quantity** des nœuds identifiés par *rowpattern*.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans l'ensemble de lignes fourni par OPENXML.  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT stmt using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@XmlDocumentHandle, '/ROOT/Customer/Order/OrderDetail',2)  
WITH (OrderID     int         '../@OrderID',  
      CustomerID  varchar(10) '../@CustomerID',  
      OrderDate   datetime    '../@OrderDate',  
      ProdID      int         '@ProductID',  
      Qty         int         '@Quantity')  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
OrderID CustomerID        OrderDate          ProdID    Qty  
-------------------------------------------------------------  
10248    VINET     1996-07-04 00:00:00.000     11       12  
10248    VINET     1996-07-04 00:00:00.000     42       10  
10283    LILAS     1996-08-16 00:00:00.000     72        3  
```  
  
 Le modèle XPath défini comme *ColPattern* peut également être spécifié pour mapper les éléments XML sur les colonnes de l’ensemble de lignes (aboutissant ainsi à un mappage centré sur l'élément). Dans l'exemple suivant, <`CustomerID`> et <`OrderDate`> du document XML sont des sous-éléments de l'élément <`Orders`>. Dans la mesure où *ColPattern* se substitue au mappage spécifié dans le paramètre *flags*, il n’y a pas lieu de spécifier le paramètre *flags* dans OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order EmployeeID="5" >  
      <OrderID>10248</OrderID>  
      <CustomerID>VINET</CustomerID>  
      <OrderDate>1996-07-04T00:00:00</OrderDate>  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order  EmployeeID="3" >  
      <OrderID>10283</OrderID>  
      <CustomerID>LILAS</CustomerID>  
      <OrderDate>1996-08-16T00:00:00</OrderDate>  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail')  
WITH (CustomerID  varchar(10)   '../CustomerID',  
      OrderDate   datetime      '../OrderDate',  
      ProdID      int           '@ProductID',  
      Qty         int           '@Quantity')  
EXEC sp_xml_removedocument @docHandle  
```  
  
### <a name="c-combining-attribute-centric-and-element-centric-mapping"></a>C. Combinaison du mappage centré sur les attributs et du mappage centré sur les éléments  
 Dans cet exemple, le paramètre *flags* a la valeur **3** pour indiquer qu’il s’agit d’un mappage centré à la fois sur l’attribut et sur l’élément. Dans ce cas, le mappage centré sur l'attribut est appliqué en premier, puis le mappage centré sur l'élément est appliqué ensuite pour toutes les colonnes non encore traitées.  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument =N'<ROOT>  
<Customer CustomerID="VINET"  >  
     <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" >   
     <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer',3)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Voici le résultat :  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Le mappage centré sur l’attribut est appliqué à **CustomerID**. Il n’y a pas d’attribut **ContactName** dans l’élément <`Customer`>. Par conséquent, le mappage centré sur l'élément est appliqué.  
  
### <a name="d-specifying-the-text-xpath-function-as-colpattern"></a>D. Spécification de la fonction XPath text() en tant que ColPattern  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`> et <`Order`>. L’instruction OPENXML extrait un ensemble de lignes constitué de l’attribut **oid** de l’élément <`Order`>, de l’ID du parent du nœud identifié par *rowpattern* et de la chaîne de valeur de feuille du contenu de l’élément.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/root/Customer/Order) identifie les nœuds <`Order`> à traiter.  
  
-   Le paramètre *flags* prend la valeur **1** pour indiquer qu’il s’agit d’un mappage centré sur l’attribut. Ainsi, les attributs XML sont mappés sur les colonnes de l’ensemble de lignes définies dans *SchemaDeclaration*.  
  
-   Dans *SchemaDeclaration* (dans la clause WITH), les noms de colonne de l’ensemble de lignes **oid** et **amount** correspondent aux noms des attributs XML équivalents. Par conséquent, le paramètre *ColPattern* n’est pas spécifié. Pour la colonne **comment** de l’ensemble de lignes, la fonction XPath **text()** est spécifiée en tant que *ColPattern*. Ceci remplace le mappage centré sur l’attribut défini dans le paramètre *flags*; la colonne contient la chaîne de valeur de feuille du contenu de l’élément.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans l'ensemble de lignes fourni par OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied  
      </Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
            <Urgency>Important</Urgency>  
            Happy Customer.  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH (oid     char(5),   
           amount  float,   
           comment ntext 'text()')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
oid   amount        comment  
----- -----------   -----------------------------  
O1    3.5           NULL  
O2    13.4          Customer was very satisfied  
O3    100.0         Happy Customer.  
O4    10000.0       NULL  
```  
  
### <a name="e-specifying-tablename-in-the-with-clause"></a>E. Spécification de TableName dans la clause WITH  
 Dans cet exemple, *TableName* est spécifié dans la clause WITH au lieu de *SchemaDeclaration*. Cela est utile si vous disposez d’une table dont la structure vous convient et qu’aucun modèle de colonne (paramètre *ColPattern*) n’est requis.  
  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`> et <`Order`>. L’instruction OPENXML extrait les informations sur les commandes dans un ensemble de lignes à trois colonnes (**oid**, **date** et **amount**) à partir du document XML.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/root/Customer/Order) identifie les nœuds <`Order`> à traiter.  
  
-   Il n’y a pas de paramètre *SchemaDeclaration* dans la clause WITH. Un nom de table est spécifié à la place. Par conséquent, le schéma de la table est utilisé en guise de schéma de l'ensemble de lignes.  
  
-   Le paramètre *flags* prend la valeur **1** pour indiquer qu’il s’agit d’un mappage centré sur l’attribut. Par conséquent, les attributs des éléments (identifiés par *rowpattern*) sont mappés sur les colonnes de l’ensemble de lignes portant le même nom.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans l'ensemble de lignes fourni par OPENXML.  
  
```  
-- Create a test table. This table schema is used by OPENXML as the  
-- rowset schema.  
CREATE TABLE T1(oid char(5), date datetime, amount float)  
GO  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
-- Sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH T1  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
oid   date                        amount  
----- --------------------------- ----------  
O1    1996-01-20 00:00:00.000     3.5  
O2    1997-04-30 00:00:00.000     13.4  
O3    1999-07-14 00:00:00.000     100.0  
O4    1996-01-20 00:00:00.000     10000.0  
```  
  
### <a name="f-obtaining-the-result-in-an-edge-table-format"></a>F. Obtention du résultat dans un format de table edge  
 Dans cet exemple, la clause WITH n'est pas spécifiée dans l'instruction OPENXML. Par conséquent, l'ensemble de lignes généré par OPENXML possède un format de table edge. L'instruction SELECT renvoie toutes les colonnes dans la table edge.  
  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`>, <`Order`> et <`OrderDetail`>.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/ROOT/Customer) identifie les nœuds <`Customer`> à traiter.  
  
-   La clause WITH n'est pas fournie. Par conséquent, OPENXML renvoie l'ensemble de lignes dans un format de table edge.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans la table edge.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
SET @xmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer')  
  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Le résultat est renvoyé sous la forme de table edge. Vous pouvez écrire des requêtes sur la table edge afin d'obtenir des informations. Exemple :  
  
-   La requête suivante renvoie le nombre de nœuds **Customer** dans le document. Comme la clause WITH n'est pas spécifiée, OPENXML renvoie une table edge. L'instruction SELECT interroge la table edge.  
  
    ```  
    SELECT count(*)  
    FROM OPENXML(@docHandle, '/')  
    WHERE localname = 'Customer'  
    ```  
  
-   La requête suivante renvoie les noms locaux des nœuds XML de type element.  
  
    ```  
    SELECT distinct localname   
    FROM OPENXML(@docHandle, '/')   
    WHERE nodetype = 1   
    ORDER BY localname  
    ```  
  
### <a name="g-specifying-rowpattern-ending-with-an-attribute"></a>G. Spécification du paramètre rowpattern, terminé par un attribut  
 Dans cet exemple, le document XML est constitué des éléments <`Customer`>, <`Order`> et <`OrderDetail`>. L’instruction OPENXML extrait les informations sur le détail des commandes dans un ensemble de lignes à trois colonnes (**ProductID**, **Quantity** et **OrderID**) à partir du document XML.  
  
 Tout d’abord, la procédure stockée **sp_xml_preparedocument** est appelée pour obtenir un descripteur de document. Ce descripteur est transmis à OPENXML.  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   *rowpattern* (/ROOT/Customer/Order/OrderDetail/@ProductID) se termine par un attribut XML, **ProductID**. Dans l'ensemble de lignes obtenu, une ligne est créée pour chaque nœud d'attribut sélectionné dans le document XML.  
  
-   Dans cet exemple, le paramètre *flags* n’est pas spécifié. En revanche, les mappages sont définis par le paramètre *ColPattern* .  
  
 Dans *SchemaDeclaration* (dans la clause WITH), le paramètre *ColPattern* est également spécifié avec les paramètres *ColName* et *ColType* . Le paramètre facultatif *ColPattern* correspond au modèle XPath spécifié pour définir les éléments suivants :  
  
-   Le modèle XPath (**.**) spécifié pour le paramètre *ColPattern* de la colonne **ProdID** de l’ensemble de lignes identifie le nœud de contexte (nœud actuel). Comme pour le paramètre *rowpattern* spécifié, il s’agit de l’attribut **ProductID** de l’élément <`OrderDetail`>.  
  
-   Le paramètre *ColPattern*, **../@Quantity**, spécifié pour la colonne **Qty** de l’ensemble de lignes, identifie l’attribut **Quantity** du parent <`OrderDetail`>, nœud du nœud de contexte \<ProductID>.  
  
-   De même, le paramètre *ColPattern*, **../../@OrderID**, spécifié pour la colonne **OID** de l’ensemble de lignes, identifie l’attribut **OrderID** du parent, <`Order`>, du nœud parent du nœud de contexte. Le nœud parent est <`OrderDetail`> et le nœud de contexte est <`ProductID`>.  
  
 Ensuite, l'instruction SELECT extrait toutes les colonnes dans l'ensemble de lignes fourni par OPENXML.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--Sample XML document  
SET @xmlDocument =N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail/@ProductID')  
       WITH ( ProdID  int '.',  
              Qty     int '../@Quantity',  
              OID     int '../../@OrderID')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Voici le résultat obtenu :  
  
```  
ProdID      Qty         OID  
----------- ----------- -------   
11          12          10248  
42          10          10248  
72          3           10283  
```  
  
### <a name="h-specifying-an-xml-document-that-has-multiple-text-nodes"></a>H. Spécification d'un document XML comprenant plusieurs nœuds de texte  
 Si vous disposez d’un document XML comprenant plusieurs nœuds de texte, une instruction SELECT avec un paramètre *ColPattern* **text()** renvoie uniquement le premier nœud de texte, et non la totalité. Exemple :  
  
```  
DECLARE @h int  
EXEC sp_xml_preparedocument @h OUTPUT,  
         N'<root xmlns:a="urn:1">  
           <a:Elem abar="asdf">  
             T<a>a</a>U  
           </a:Elem>  
         </root>',  
         '<ns xmlns:b="urn:1" />'  
  
SELECT * FROM openxml(@h, '/root/b:Elem')  
      WITH (Col1 varchar(20) 'text()')  
EXEC sp_xml_removedocument @h  
```  
  
 L’instruction SELECT renvoie **T** comme résultat (et non **TaU**).  
  
### <a name="i-specifying-the-xml-data-type-in-the-with-clause"></a>I. Spécification du type de données xml dans la clause WITH  
 Dans la clause WITH, un modèle de colonne mappé sur la colonne de type **xml** (typé ou non typé) doit renvoyer une séquence vide ou une séquence d’éléments, des instructions de traitement, des nœuds de texte et des commentaires. Les données sont converties en type de données **xml** .  
  
 Dans l’exemple suivant, la déclaration de schéma de la table de la clause WITH inclut des colonnes de type **xml** .  
  
```  
DECLARE @h int  
DECLARE @x xml  
set @x = '<Root>  
  <row id="1"><lname>Duffy</lname>  
   <Address>  
            <Street>111 Maple</Street>  
            <City>Seattle</City>  
   </Address>  
  </row>  
  <row id="2"><lname>Wang</lname>  
   <Address>  
            <Street>222 Pine</Street>  
            <City>Bothell</City>  
   </Address>  
  </row>  
</Root>'  
  
EXEC sp_xml_preparedocument @h output, @x  
SELECT *  
FROM   OPENXML (@h, '/Root/row', 10)  
      WITH (id int '@id',  
  
            lname    varchar(30),  
            xmlname  xml 'lname',  
            OverFlow xml '@mp:xmltext')  
EXEC sp_xml_removedocument @h  
```  
  
 Il s’agit plus précisément de transmettre une variable de type **xml** (@x) à la fonction **sp_xml_preparedocument()**.  
  
 Voici le résultat obtenu :  
  
```  
id  lname   xmlname                   OverFlow  
--- ------- ------------------------------ -------------------------------  
1   Duffy   <lname>Duffy</lname>  <row><Address>  
                                   <Street>111 Maple</Street>  
                                   <City>Seattle</City>  
                                  </Address></row>  
2   Wang    <lname>Wang</lname>   <row><Address>  
                                    <Street>222 Pine</Street>  
                                    <City>Bothell</City>  
                                   </Address></row>  
```  
  
 Notez les points suivants par rapport au résultat obtenu :  
  
-   Pour la colonne **lname** de type **varchar(30)**, la valeur est récupérée à partir de l’élément <`lname`> correspondant.  
  
-   Pour la colonne **xmlname** de type **xml**, le même élément name est renvoyé en tant que valeur.  
  
-   L'indicateur prend la valeur 10, soit 2 + 8, où 2 indique qu'il s'agit d'un mappage centré sur l'élément et 8 indique que seules les données XML non consommées doivent être ajoutées à la colonne OverFlow définie dans la clause WITH. Si vous définissez l'indicateur à 2, l'ensemble du document XML est copié dans la colonne OverFlow spécifiée dans la clause WITH.  
  
-   Si la colonne de la clause WITH est une colonne en XML typé et que l'instance XML ne respecte pas le schéma, une erreur est générée.  
  
### <a name="j-retrieving-individual-values-from-multivalued-attributes"></a>J. Extraction de valeurs individuelles à partir d'attributs à plusieurs valeurs  
 Un document XML peut avoir des attributs qui ont plusieurs valeurs. Par exemple, l’attribut **IDREFS** peut avoir plusieurs valeurs. Dans un document XML, les valeurs des attributs à plusieurs valeurs sont spécifiées sous la forme d'une chaîne qui contient les valeurs séparées par un espace. Dans le document XML suivant, les attributs **attends** de l’élément \<Student> et **attendedBy** de l’élément \<Class> ont plusieurs valeurs. L'extraction de valeurs individuelles d'un attribut XML à plusieurs valeurs et le stockage de chacune d'entre elles dans une ligne distincte de la base de données demandent un travail supplémentaire. Cet exemple illustre le processus.  
  
 Cet exemple de document XML est constitué des éléments suivants :  
  
-   \<Student>  
  
     Attributs **id** (ID étudiant), **name**et **attends** . L’attribut **attends** est un attribut à plusieurs valeurs.  
  
-   \<Class>  
  
     Attributs **id** (ID cours), **name**et **attendedBy** . L’attribut **attendedBy** est un attribut à plusieurs valeurs.  
  
 L’attribut **attends** de \<Student> et l’attribut **attendedBy** de \<Class> représentent une relation **m:n** entre les tables Student et Class. Un étudiant peut faire partie de plusieurs cours et un cours peut avoir plusieurs étudiants.  
  
 Supposons que vous vouliez fragmenter ce document et l'enregistrer dans la base de données comme suit :  
  
-   Enregistrez les données \<Student> dans la table Students.  
  
-   Enregistrez les données \<Class> dans la table Courses.  
  
-   Enregistrez les données de la relation **m:n** (entre Student et Class) dans la table CourseAttendence. L'extraction des valeurs demande davantage de travail. Pour extraire ces informations et les stocker dans la table, utilisez ces procédures stockées :  
  
    -   **Insert_Idrefs_Values**  
  
         Insère les valeurs de l'identificateur du cours et de l'identificateur de l'étudiant dans la table CourseAttendence.  
  
    -   **Extract_idrefs_values**  
  
         Extrait les ID individuels des étudiants de chaque élément \<Course>. Une table edge est utilisée pour extraire ces valeurs.  
  
 Voici les étapes de l'opération :  
  
```  
-- Create these tables:  
DROP TABLE CourseAttendance  
DROP TABLE Students  
DROP TABLE Courses  
GO  
CREATE TABLE Students(  
                id   varchar(5) primary key,  
                name varchar(30)  
                )  
GO  
CREATE TABLE Courses(  
               id       varchar(5) primary key,  
               name     varchar(30),  
               taughtBy varchar(5)  
)  
GO  
CREATE TABLE CourseAttendance(  
             id         varchar(5) references Courses(id),  
             attendedBy varchar(5) references Students(id),  
             constraint CourseAttendance_PK primary key (id, attendedBy)  
)  
go  
-- Create these stored procedures:  
DROP PROCEDURE f_idrefs  
GO  
CREATE PROCEDURE f_idrefs  
    @t      varchar(500),  
    @idtab  varchar(50),  
    @id     varchar(5)  
AS  
DECLARE @sp int  
DECLARE @att varchar(5)  
SET @sp = 0  
WHILE (LEN(@t) > 0)  
BEGIN   
    SET @sp = CHARINDEX(' ', @t+ ' ')  
    SET @att = LEFT(@t, @sp-1)  
    EXEC('INSERT INTO '+@idtab+' VALUES ('''+@id+''', '''+@att+''')')  
    SET @t = SUBSTRING(@t+ ' ', @sp+1, LEN(@t)+1-@sp)  
END  
Go  
  
DROP PROCEDURE fill_idrefs  
GO  
CREATE PROCEDURE fill_idrefs   
    @xmldoc     int,  
    @xpath      varchar(100),  
    @from       varchar(50),  
    @to         varchar(50),  
    @idtable    varchar(100)  
AS  
DECLARE @t varchar(500)  
DECLARE @id varchar(5)  
  
/* Temporary Edge table */  
SELECT *   
INTO #TempEdge   
FROM OPENXML(@xmldoc, @xpath)  
  
DECLARE fillidrefs_cursor CURSOR FOR  
    SELECT CAST(iv.text AS nvarchar(200)) AS id,  
           CAST(av.text AS nvarchar(4000)) AS refs  
    FROM   #TempEdge c, #TempEdge i,  
           #TempEdge iv, #TempEdge a, #TempEdge av  
    WHERE  c.id = i.parentid  
    AND    UPPER(i.localname) = UPPER(@from)  
    AND    i.id = iv.parentid  
    AND    c.id = a.parentid  
    AND    UPPER(a.localname) = UPPER(@to)  
    AND    a.id = av.parentid  
  
OPEN fillidrefs_cursor  
FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    IF (@@FETCH_STATUS <> -2)  
    BEGIN  
        execute f_idrefs @t, @idtable, @id  
    END  
    FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
END  
CLOSE fillidrefs_cursor  
DEALLOCATE fillidrefs_cursor  
Go  
-- This is the sample document that is shredded and the data is stored in the preceding tables.  
DECLARE @h int  
EXECUTE sp_xml_preparedocument @h OUTPUT, N'<Data>  
  <Student id = "s1" name = "Student1"  attends = "c1 c3 c6"  />  
  <Student id = "s2" name = "Student2"  attends = "c2 c4" />  
  <Student id = "s3" name = "Student3"  attends = "c2 c4 c6" />  
  <Student id = "s4" name = "Student4"  attends = "c1 c3 c5" />  
  <Student id = "s5" name = "Student5"  attends = "c1 c3 c5 c6" />  
  <Student id = "s6" name = "Student6" />  
  
  <Class id = "c1" name = "Intro to Programming"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c2" name = "Databases"   
         attendedBy = "s2 s3" />  
  <Class id = "c3" name = "Operating Systems"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c4" name = "Networks" attendedBy = "s2 s3" />  
  <Class id = "c5" name = "Algorithms and Graphs"   
         attendedBy =  "s4 s5"/>  
  <Class id = "c6" name = "Power and Pragmatism"   
         attendedBy = "s1 s3 s5" />  
</Data>'  
  
INSERT INTO Students SELECT * FROM OPENXML(@h, '//Student') WITH Students  
  
INSERT INTO Courses SELECT * FROM OPENXML(@h, '//Class') WITH Courses  
/* Using the edge table */  
EXECUTE fill_idrefs @h, '//Class', 'id', 'attendedby', 'CourseAttendance'  
  
SELECT * FROM Students  
SELECT * FROM Courses  
SELECT * FROM CourseAttendance  
  
EXECUTE sp_xml_removedocument @h  
```  
  
### <a name="k-retrieving-binary-from-base64-encoded-data-in-xml"></a>K. Extraction de données binaires à partir de données codées en mode Base64 dans XML  
 Des données binaires sont souvent incluses dans XML à l'aide de l'encodage base64. Lorsque vous fragmentez ce code XML à l'aide d'une instruction OPENXML, vous recevez les données encodées en base64. Cet exemple montre comment reconvertir en binaire les données encodées en base64.  
  
-   Créez une table avec un exemple de données binaires.  
  
-   Utilisez une requête FOR XML et l'option BINARY BASE64 pour construire du code XML où les données binaires sont encodées en base64.  
  
-   Fragmentez le code XML à l'aide de OPENXML. Les données renvoyées par OPENXML seront des données encodées en base64. Ensuite, appelez la fonction .value pour reconvertir ces dernières en binaire.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 varbinary(100))  
go  
-- Insert sample binary data  
INSERT T VALUES(1, 0x1234567890)   
go  
 -- Create test XML document that has base64 encoded binary data (use FOR XML query and specify BINARY BASE64 option)  
SELECT * FROM T  
FOR XML AUTO, BINARY BASE64  
go  
-- result  
-- <T Col1="1" Col2="EjRWeJA="/>  
  
-- Now shredd the sample XML using OPENXML.   
-- Call the .value function to convert   
-- the base64 encoded data returned by OPENXML to binary.  
DECLARE @h int ;  
EXEC sp_xml_preparedocument @h OUTPUT, '<T Col1="1" Col2="EjRWeJA="/>' ;  
SELECT Col1,   
CAST('<binary>' + Col2 + '</binary>' AS XML).value('.', 'varbinary(max)') AS BinaryCol   
FROM openxml(@h, '/T')   
WITH (Col1 integer, Col2 varchar(max)) ;  
EXEC sp_xml_removedocument @h ;  
GO  
```  
  
 Voici l'ensemble de résultats. Les données binaires renvoyées sont les données binaires d'origine de la table T.  
  
```  
Col1        BinaryCol  
----------- ---------------------  
1           0x1234567890  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_xml_preparedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)   
 [sp_xml_removedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)   
 [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md)  
  
  
