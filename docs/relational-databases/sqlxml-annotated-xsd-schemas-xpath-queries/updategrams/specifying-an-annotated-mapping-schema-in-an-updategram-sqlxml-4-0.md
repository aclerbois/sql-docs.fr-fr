---
title: Spécification d’un schéma de mappage annoté dans une mise à jour (SQLXML 4.0) | Documents Microsoft
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
- annotated XSD schemas, updategrams
- data types [SQLXML], mapping schema in updategrams
- updategrams [SQLXML], annotated mapping schemas
- annotated XDR schemas, updategrams
- inverse attribute
- parent-child relationships [SQLXML]
- mapping-schema attribute
- mapping schema [SQLXML], updategrams
- sql:inverse
ms.assetid: 2e266ed9-4cfb-434a-af55-d0839f64bb9a
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ce7e9b90c034643ba31df7f34425ff8203956b9c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-40"></a>Spécification d'un schéma de mappage annoté dans un code de mise à jour (updategram) (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique explique comment le schéma de mappage (XSD ou XDR) spécifié dans un code de mise à jour est utilisé pour traiter les mises à jour. Dans une mise à jour, vous pouvez fournir le nom d’un schéma de mappage annotés à utiliser lors du mappage des éléments et attributs dans la mise à jour aux tables et colonnes dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Lorsqu'un schéma de mappage est spécifié dans un code de mise à jour, les noms d'élément et d'attribut spécifiés dans le code de mise à jour doivent être mappés aux éléments et aux attributs dans le schéma de mappage.  
  
 Pour spécifier un schéma de mappage, vous utilisez la **schéma de mappage** attribut de la  **\<synchronisation >** élément. Les exemples suivants présentent deux codes de mise à jour : l'un utilise un schéma de mappage simple et l'autre utilise un schéma plus complexe.  
  
> [!NOTE]  
>  Cette documentation suppose une connaissance suffisante des modèles et de la prise en charge des schémas de mappage dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Introduction aux schémas de XSD annoté & #40 ; SQLXML 4.0 & #41 ; ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Pour les applications héritées qui utilisent XDR, consultez [de schémas XDR annotés &#40;déconseillé dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="dealing-with-data-types"></a>Traitement des types de données  
 Si le schéma spécifie la **image**, **binaire**, ou **varbinary** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données (à l’aide de **SQL : DataType**) et ne spécifie pas un type de données XML, le code de mise part du principe que le type de données XML est **binaire base 64**. Si vos données sont **bin.base** type, vous devez spécifier explicitement le type (**dt:type=bin.base** ou **type = « xsd:hexBinary »**).  
  
 Si le schéma spécifie la **dateTime**, **date**, ou **temps** type de données XSD, vous devez également spécifier correspondant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données à l’aide de **SQL : DataType = « dateTime »**.  
  
 Lors de la gestion des paramètres de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **money** type, vous devez spécifier explicitement **SQL : DataType = « money »** sur le nœud approprié dans le schéma de mappage.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l’aide de la procédure ci-après, vous devez respecter la configuration requise spécifiée dans [configuration requise pour exécuter les exemples de SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-creating-an-updategram-with-a-simple-mapping-schema"></a>A. Création d'un code de mise à jour avec un schéma de mappage simple  
 Le schéma XSD suivant (SampleSchema.xml) est un schéma de mappage qui mappe le  **\<client >** élément à la table Sales.Customer :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
        <xsd:attribute name="CustID"    
                       sql:field="CustomerID"   
                       type="xsd:string" />  
        <xsd:attribute name="RegionID"    
                       sql:field="TerritoryID"    
                       type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Le code de mise à jour suivant insère un enregistrement dans la table Sales.Customer et compte sur le schéma de mappage précédent pour mapper correctement ces données à la table. Notez que la mise à jour utilise le même nom d’élément,  **\<client >**, comme défini dans le schéma. C'est absolument essentiel dans la mesure où le code de mise à jour spécifie un schéma particulier.  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdateSchema.xml.  
  
2.  Copiez le modèle de code de mise à jour ci-dessous et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdategram.xml dans le répertoire où vous avez enregistré le fichier SampleUpdateSchema.xml.  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleUpdateSchema.xml">  
        <updg:before>  
          <Customer CustID="1" RegionID="1"  />  
        </updg:before>  
        <updg:after>  
          <Customer CustID="1" RegionID="2" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (SampleUpdateSchema.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
   <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
         xmlns:dt="urn:schemas-microsoft-com:datatypes"   
         xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
     <ElementType name="Customer" sql:relation="Sales.Customer" >  
       <AttributeType name="CustID" />  
       <AttributeType name="RegionID" />  
  
       <attribute type="CustID" sql:field="CustomerID" />  
       <attribute type="RegionID" sql:field="TerritoryID" />  
     </ElementType>  
   </Schema>   
```  
  
### <a name="b-inserting-a-record-by-using-the-parent-child-relationship-specified-in-the-mapping-schema"></a>B. Insertion d'un enregistrement à l'aide de la relation parent-enfant spécifiée dans le schéma de mappage  
 Les éléments du schéma peuvent être liés. Le  **\<SQL : Relationship >** élément spécifie la relation parent-enfant entre les éléments de schéma. Ces informations sont utilisées pour mettre à jour les tables correspondantes qui ont une relation clé primaire/clé étrangère.  
  
 Le schéma de mappage suivant (SampleSchema.xml) se compose de deux éléments,  **\<ordre >** et  **\<OD >**:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Sales.SalesOrderHeader"  
          parent-key="SalesOrderID"  
          child="Sales.SalesOrderDetail"  
          child-key="SalesOrderID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="OD"   
                     sql:relation="Sales.SalesOrderDetail"  
                     sql:relationship="OrderOD" >  
           <xsd:complexType>  
              <xsd:attribute name="SalesOrderID"   type="xsd:integer" />  
              <xsd:attribute name="ProductID" type="xsd:integer" />  
             <xsd:attribute name="UnitPrice"  type="xsd:decimal" />  
             <xsd:attribute name="OrderQty"   type="xsd:integer" />  
             <xsd:attribute name="UnitPriceDiscount"   type="xsd:decimal" />  
  
           </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesOrderID"  type="xsd:integer" />  
        <xsd:attribute name="OrderDate"  type="xsd:date" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 La mise à jour suivant utilise ce schéma XSD pour ajouter un nouvel enregistrement de détail de commande (un  **\<OD >** élément dans le  **\<après >** bloc) pour la commande 43860. Le **schéma de mappage** attribut est utilisé pour spécifier le schéma de mappage dans la mise à jour.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before>  
       <Order SalesOrderID="43860" />  
    </updg:before>  
    <updg:after>  
      <Order SalesOrderID="43860" >  
           <OD ProductID="753" UnitPrice="$10.00"  
               Quantity="5" Discount="0.0" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdateSchema.xml.  
  
2.  Copiez le modèle de code de mise à jour (updategram) ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdategram.xml dans le répertoire où vous avez enregistré le fichier SampleUpdateSchema.xml.  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (SampleUpdateSchema.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="OD" sql:relation="Sales.SalesOrderDetail" >  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="ProductID" />  
    <AttributeType name="UnitPrice"  dt:type="fixed.14.4" />  
    <AttributeType name="OrderQty" />  
    <AttributeType name="UnitPriceDiscount" />  
  
    <attribute type="SalesOrderID" />  
    <attribute type="ProductID" />  
    <attribute type="UnitPrice" />  
    <attribute type="OrderQty" />  
    <attribute type="UnitPriceDiscount" />  
</ElementType>  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader" >  
    <AttributeType name="CustomerID" />  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="OrderDate" />  
  
    <attribute type="CustomerID" />  
    <attribute type="SalesOrderID" />  
    <attribute type="OrderDate" />  
    <element type="OD" >  
             <sql:relationship   
                   key-relation="Sales.SalesOrderHeader"  
                   key="SalesOrderID"  
                   foreign-key="SalesOrderID"  
                   foreign-relation="Sales.SalesOrderDetail" />  
    </element>  
</ElementType>  
</Schema>  
```  
  
### <a name="c-inserting-a-record-by-using-the-parent-child-relationship-and-inverse-annotation-specified-in-the-xsd-schema"></a>C. Insertion d'un enregistrement à l'aide de la relation parent-enfant et de l'annotation inverse spécifiées dans le schéma XSD  
 Cet exemple montre comment la logique de mise à jour utilise la relation parent-enfant spécifiée dans le schéma XSD pour traiter les mises à jour et comment la **inverse** annotation est utilisée. Pour plus d’informations sur la **inverse** annotation, consultez [spécifiant l’attribut SQL : inverse sur SQL : Relationship &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md).  
  
 Cet exemple suppose que les tableaux suivants se trouvent dans le **tempdb** base de données :  
  
-   `Cust (CustomerID, CompanyName)`, où `CustomerID` est la clé primaire  
  
-   `Ord (OrderID, CustomerID)`, où `CustomerID` est une clé étrangère qui fait référence à la clé primaire `CustomerID` dans la table `Cust`.  
  
 Le code de mise à jour utilise le schéma XSD suivant pour insérer des enregistrements dans les tables Cust et Ord :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
       <sql:relationship name="OrdCust" inverse="true"  
                  parent="Ord"  
                  parent-key="CustomerID"  
                  child-key="CustomerID"  
                  child="Cust"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
<xsd:element name="Order" sql:relation="Ord">  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customer" sql:relationship="OrdCust"/>  
    </xsd:sequence>  
    <xsd:attribute name="OrderID"   type="xsd:int"/>  
    <xsd:attribute name="CustomerID" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
<xsd:element name="Customer" sql:relation="Cust">  
  <xsd:complexType>  
     <xsd:attribute name="CustomerID"  type="xsd:string"/>  
    <xsd:attribute name="CompanyName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
</xsd:schema>  
```  
  
 Le schéma XSD dans cet exemple a  **\<client >** et  **\<ordre >** éléments et spécifie une relation parent-enfant entre les deux éléments. Il identifie  **\<ordre >** en tant que l’élément parent et  **\<client >** comme élément enfant.  
  
 La logique de traitement du code de mise à jour utilise les informations relatives à la relation parent-enfant pour déterminer l'ordre dans lequel les enregistrements sont insérés dans les tables. Dans cet exemple, la logique de mise à jour tente d’abord insérer un enregistrement dans la table Ord (étant donné que  **\<ordre >** est le parent), puis tente d’insérer un enregistrement dans la table Cust (étant donné que  **\<client >** est l’enfant). Toutefois, en raison des informations de clé primaire/clé étrangère contenues dans le schéma de la table de base de données, cette opération d'insertion provoque une violation de clé étrangère dans la base de données et échoue.  
  
 Pour indiquer à la logique de mise à jour pour inverser la relation parent-enfant pendant l’opération de mise à jour, le **inverse** annotation est spécifiée sur le  **\<relation >** élément. En conséquence, les enregistrements sont d'abord ajoutés dans la table Cust, puis dans la table Ord, et l'opération réussit.  
  
 Le code de mise à jour suivant insère une commande (OrderID=2) dans la table Ord et un client (CustomerID='AAAAA') dans la table Cust à l'aide du schéma XSD spécifié :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before/>  
    <updg:after>  
      <Order OrderID="2" CustomerID="AAAAA" >  
        <Customer CustomerID="AAAAA" CompanyName="AAAAA Company" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Création de ces tables dans le **tempdb** base de données :  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust(CustomerID varchar(5) primary key,   
                      CompanyName varchar(20))  
    GO  
    CREATE TABLE Ord (OrderID int primary key,   
                      CustomerID varchar(5) references Cust(CustomerID))  
    GO  
    ```  
  
2.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdateSchema.xml.  
  
3.  Copiez le modèle de code de mise à jour (updategram) ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdategram.xml dans le répertoire où vous avez enregistré le fichier SampleUpdateSchema.xml.  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (SampleUpdateSchema.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
4.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations de sécurité de mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
