---
title: 'SQL : Relationship et la règle de tri par clé (SQLXML 4.0) | Documents Microsoft'
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
- sql:relationship
- key ordering rules [SQLXML]
- relationship annotation
ms.assetid: 914cb152-09f5-4b08-b35d-71940e4e9986
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2a49fbc8263eab936a153a798c7326fa99e71b06
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="annotation-interpretation---sqlrelationship-and-key-ordering-rule"></a>Interprétation d’annotation - SQL : Relationship et la règle de tri par clé
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans la mesure où la fonctionnalité de chargement en masse XML génère des enregistrements lorsque les nœuds de ces derniers entrent dans l'étendue, et qu'elle envoie ces enregistrements à Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque les nœuds correspondants sortent de l'étendue, les données de l'enregistrement doivent être présentes dans l'étendue du nœud.  
  
 Examinez le schéma XSD suivant, dans lequel la relation un-à-plusieurs entre  **\<client >** et  **\<ordre >** éléments (un client peut passer plusieurs commandes) est spécifié en utilisant le  **\<SQL : Relationship >** élément :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"<>   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
     <xsd:sequence>  
       <xsd:element name="CustomerID"  type="xsd:integer" />  
       <xsd:element name="CompanyName" type="xsd:string" />  
       <xsd:element name="City"        type="xsd:string" />  
       <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
         <xsd:complexType>  
          <xsd:attribute name="OrderID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Comme le  **\<client >** nœud d’élément entre dans l’étendue, le chargement en masse XML génère un enregistrement de client. Cet enregistrement est conservé jusqu'à ce que le chargement en masse XML lit  **\</Customer >**. Dans le traitement le  **\<ordre >** nœud d’élément, le chargement en masse XML utilise  **\<SQL : Relationship >** pour obtenir la valeur de la colonne de clé étrangère CustomerID de la table CustOrder à partir de la  **\<client >** élément, parent, car le  **\<ordre >** élément ne spécifie pas le **CustomerID** attribut. Cela signifie que la définition de la  **\<client >** élément, vous devez spécifier le **CustomerID** attribut dans le schéma avant de spécifier  **\<SQL : Relationship >**. Sinon, lorsque un  **\<ordre >** élément entre dans l’étendue, le chargement en masse XML génère un enregistrement pour la table CustOrder, et lorsque le code XML en bloc charge atteint le  **\</Order >** , balise de fin il envoie l’enregistrement à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sans la valeur de la colonne de clé étrangère CustomerID.  
  
 Enregistrez le schéma fourni dans cet exemple sous le nom SampleSchema.xml.  
  
### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez les tables suivantes :  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
               CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
               CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
2.  Enregistrez l'exemple de données ci-après sous le nom SampleXMLData.xml :  
  
    ```  
    <ROOT>    
      <Customers>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
        <CustomerID>1111</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>    
        <Order OrderID="3" />  
        <CustomerID>1112</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
        <CustomerID>1113</CustomerID>  
      </Customers>  
    </ROOT>  
    ```  
  
3.  Pour effectuer le chargement en masse XML, enregistrez l'exemple [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) suivant sous le nom MySample.vbs, puis exécutez-le :  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
     Il en résulte que la fonctionnalité de chargement en masse XML insère une valeur NULL dans la colonne de clé étrangère CustomerID de la table CustOrder. Si vous modifiez l’exemple de données XML afin que le  **\<CustomerID >** élément enfant s’affiche avant la  **\<ordre >** élément enfant, vous obtenez le résultat attendu : chargement en masse XML insère la valeur de clé étrangère spécifiée dans la colonne.  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID"  />  
   <ElementType name="CompanyName" />  
   <ElementType name="City"        />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
                 <sql:relationship  
                        key-relation    ="Cust"  
                        key             ="CustomerID"  
                        foreign-key     ="CustomerID"  
                        foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
  
