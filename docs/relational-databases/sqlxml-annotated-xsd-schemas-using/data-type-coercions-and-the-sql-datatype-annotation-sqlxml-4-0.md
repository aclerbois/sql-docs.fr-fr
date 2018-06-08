---
title: 'Conversions de types de données et SQL : DataType Annotation (SQLXML 4.0) | Documents Microsoft'
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- type attribute
- sql:datatype
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- xsd:type
- datatype annotation
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XSD schemas [SQLXML], mapping data types
ms.assetid: db192105-e8aa-4392-b812-9d727918c005
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 25685856bbb1b088f7c2825e27973915f850dea7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-coercions-and-the-sqldatatype-annotation-sqlxml-40"></a>Forçages de type de données et annotation sql:datatype (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans un schéma XSD, les **xsd : type** attribut spécifie le type de données XSD d’un élément ou attribut. Lorsqu'un schéma XSD est utilisé pour extraire des données de la base de données, le type de données spécifié est utilisé pour formater les données.  
  
 En plus de spécifier un type XSD dans un schéma, vous pouvez également spécifier un Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données à l’aide de la **SQL : DataType** annotation. Le **xsd : type** et **SQL : DataType** attributs contrôlent le mappage entre les types de données XSD et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données.  
  
## <a name="xsdtype-attribute"></a>Attribut xsd:type  
 Vous pouvez utiliser la **xsd : type** attribut pour spécifier le type de données XML d’un attribut ou un élément qui est mappé à une colonne. Le **xsd : type** affecte le document qui est retourné à partir du serveur, ainsi que la requête XPath est exécutée. Quand une requête XPath est exécutée sur un schéma de mappage contienne **xsd : type**, XPath utilise le type de données spécifié lors du traitement de la requête. Pour plus d’informations sur l’utilisation de XPath **xsd : type**, consultez [mappage des Types de données XSD aux Types de données XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md).  
  
 Dans un document retourné, tous les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont convertis en représentations sous forme de chaîne. Certains types de données requièrent des conversions supplémentaires. Le tableau suivant répertorie les conversions qui sont utilisées pour différents **xsd : type** valeurs.  
  
|Type de données XSD|Conversion SQL Server|  
|-------------------|---------------------------|  
|Booléen|CONVERT(bit, COLUMN)|  
|Date|LEFT(CONVERT(nvarchar(4000), COLUMN, 126), 10)|  
|Décimal|CONVERT(money, COLUMN)|  
|id/idref/idrefs|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|nmtoken/nmtokens|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|Time|SUBSTRING(CONVERT(nvarchar(4000), COLUMN, 126), 1+CHARINDEX(N'T', CONVERT(nvarchar(4000), COLUMN, 126)), 24)|  
|Autres|Aucune conversion supplémentaire|  
  
> [!NOTE]  
>  Certaines des valeurs retournées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est peut-être pas compatible avec les types de données XML qui sont spécifiés à l’aide de **xsd : type**, soit parce que la conversion n’est pas possible (convertir par exemple, « XYZ » un **décimal** type de données) ou parce que la valeur dépasse la plage de ce type de données (par exemple, -100000 converti en un **UnsignedShort** type XSD). Les conversions de type incompatibles peuvent générer des documents XML non valides ou des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="mapping-from-sql-server-data-types-to-xsd-data-types"></a>Mappage des types de données SQL Server en types de données XSD  
 Le tableau ci-dessous montre un mappage évident de types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en types de données XSD. Si vous connaissez le type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce tableau fournit le type XSD correspondant que vous pouvez spécifier dans le schéma XSD.  
  
|Type de données SQL Server|Type de données XSD|  
|--------------------------|-------------------|  
|**bigint**|**Long**|  
|**binaire**|**base64Binary**|  
|**bit**|**boolean**|  
|**char**|**chaîne**|  
|**datetime**|**dateTime**|  
|**decimal**|**decimal**|  
|**float**|**double**|  
|**image**|**base64Binary**|  
|**int**|**int**|  
|**money**|**decimal**|  
|**nchar**|**chaîne**|  
|**ntext**|**chaîne**|  
|**nvarchar**|**chaîne**|  
|**numeric**|**decimal**|  
|**real**|**float**|  
|**smalldatetime**|**dateTime**|  
|**smallint**|**courte**|  
|**smallmoney**|**decimal**|  
|**sql_variant**|**chaîne**|  
|**sysname**|**chaîne**|  
|**texte**|**chaîne**|  
|**timestamp**|**dateTime**|  
|**tinyint**|**unsignedByte**|  
|**varbinary**|**base64Binary**|  
|**varchar**|**chaîne**|  
|**uniqueidentifier**|**chaîne**|  
  
## <a name="sqldatatype-annotation"></a>Annotation sql:datatype  
 Le **SQL : DataType** annotation est utilisée pour spécifier le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données ; cette annotation doit être spécifié lorsque :  
  
-   Vous effectuez un chargement en masse dans un **dateTime** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonne à partir d’un fichier XSD **dateTime**, **date**, ou **temps** type. Dans ce cas, vous devez identifier le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données de la colonne à l’aide de **SQL : DataType = « dateTime »**. Cette règle s'applique également aux codes de mise à jour.  
  
-   Vous effectuez un chargement en bloc dans une colonne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **uniqueidentifier** type et la valeur XSD est un GUID qui inclut des accolades ({et}). Lorsque vous spécifiez **SQL : DataType = « uniqueidentifier »**, les accolades sont supprimées de la valeur avant son insertion dans la colonne. Si **SQL : DataType** n’est pas spécifié, la valeur est envoyée avec les accolades et l’insertion ou la mise à jour échoue.  
  
-   Le type de données XML **base64Binary** est mappé à différents [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données (**binaire**, **image**, ou **varbinary**). Pour mapper le type de données XML **base64Binary** à un spécifique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de type de données, utilisez la **SQL : DataType** annotation. Cette annotation spécifie le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] explicite de la colonne à laquelle l'attribut est mappé. Ceci est utile lorsque les données sont stockées dans les bases de données. En spécifiant le **SQL : DataType** annotation, vous pouvez identifier explicites [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données.  
  
 Il est généralement recommandé de spécifier **SQL : DataType** dans le schéma.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l'aide des exemples suivants, vous devez répondre à certaines conditions requises. Pour plus d’informations, consultez [configuration requise pour exécuter les exemples de SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-xsdtype"></a>A. Spécification de xsd:type  
 Cet exemple montre comment un XSD **date** type qui est spécifié à l’aide de la **xsd : type** attribut dans le schéma affecte le document XML résultant. Le schéma fournit une vue XML de la table Sales.SalesOrderHeader dans la base de données AdventureWorks.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader">  
     <xsd:complexType>  
       <xsd:attribute name="SalesOrderID" type="xsd:string" />   
       <xsd:attribute name="CustomerID"   type="xsd:string" />   
       <xsd:attribute name="OrderDate"    type="xsd:date" />   
       <xsd:attribute name="DueDate"  />   
       <xsd:attribute name="ShipDate"  type="xsd:time" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Dans ce schéma XSD, trois attributs retournent une valeur de date à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque le schéma :  
  
-   Spécifie **xsd : type = date** sur la **OrderDate** d’attribut, la partie date de la valeur retournée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le **OrderDate** attribut s’affiche.  
  
-   Spécifie **xsd : type = heure** sur la **ShipDate** d’attribut, la partie heure de la valeur retournée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le **ShipDate** attribut s’affiche.  
  
-   Ne spécifiez pas **xsd : type** sur la **DueDate** d’attribut, la même valeur que celle qui est retournée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’affiche.  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom xsdType.xml.  
  
2.  Copiez le modèle suivant et collez-le dans un fichier texte. Enregistrez le fichier sous le nom xsdTypeT.xml dans le répertoire où vous avez enregistré xsdType.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="xsdType.xml">  
        /Order  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (xsdType.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\SqlXmlTest\xsdType.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le jeu de résultats partiel :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Order SalesOrderID="43659"   
         CustomerID="676"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
  <Order SalesOrderID="43660"   
         CustomerID="117"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
 ...  
</ROOT>  
```  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader">  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="CustomerID"  />  
    <AttributeType name="OrderDate" dt:type="date" />  
    <AttributeType name="DueDate" />  
    <AttributeType name="ShipDate" dt:type="time" />  
  
    <attribute type="SalesOrderID" sql:field="OrderID" />  
    <attribute type="CustomerID" sql:field="CustomerID" />  
    <attribute type="OrderDate" sql:field="OrderDate" />  
    <attribute type="DueDate" sql:field="DueDate" />  
    <attribute type="ShipDate" sql:field="ShipDate" />  
</ElementType>  
</Schema>  
```  
  
### <a name="b-specifying-sql-data-type-using-sqldatatype"></a>B. Spécification du type de données SQL à l'aide de sql:datatype  
 Pour obtenir un exemple fonctionnel, consultez l’exemple G dans [exemples de chargement en masse XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md). Dans cet exemple, une valeur GUID qui inclut "{" et "}" fait l'objet d'un chargement en masse. Le schéma dans cet exemple spécifie **SQL : DataType** pour identifier le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de type de données en tant que **uniqueidentifier**. Cet exemple montre quand **SQL : DataType** doit être spécifié dans le schéma.  
  
  
