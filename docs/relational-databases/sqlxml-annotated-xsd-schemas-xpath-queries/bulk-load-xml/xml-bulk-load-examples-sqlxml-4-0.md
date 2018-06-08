---
title: Exemples de chargement en masse XML (SQLXML 4.0) | Documents Microsoft
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
- overflow-field annotation
- ConnectionCommand property
- XMLFragment property
- relationship chains [SQLXML]
- multiple table bulk loading
- examples [SQLXML], XML Bulk Load
- overflow data [SQLXML]
- sql:datatype
- transaction mode [SQLXML]
- CheckConstraints property
- sql:overflow-field
- XML Bulk Load [SQLXML], examples
- TempFilePath property
- datatype annotation
- Transaction property
- KeepIdentity property
- Execute method
- streaming XML data
- xml data type [SQL Server], SQLXML
- bulk load [SQLXML], examples
ms.assetid: 970e4553-b41d-4a12-ad50-0ee65d1f305d
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4527b1c3fb4e3573bad5b34a3c4743da16d94487
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-bulk-load-examples-sqlxml-40"></a>Exemples de chargement en masse XML (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Les exemples suivants illustrent la fonctionnalité de chargement en masse XML dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Chaque exemple fournit un schéma XSD et son schéma XDR équivalent.  
  
## <a name="bulk-loader-script-validateandbulkloadvbs"></a>Script de chargement en masse (ValidateAndBulkload.vbs)  
 Le script suivant, écrit le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript), charge un document XML dans le DOM XML ; il valide par rapport à un schéma ; et, si le document est valide, exécute une en masse XML charge afin de charger le XML dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table. Ce script peut être utilisé avec chacun des exemples individuels qui s'y rapportent plus loin dans cette rubrique.  
  
> [!NOTE]  
>  Le chargement en masse XML n'émet pas d'avertissement ou d'erreur si aucun contenu n'est téléchargé à partir du fichier de données. Par conséquent, il est conseillé de valider votre fichier de données XML avant d'exécuter une opération de chargement en masse.  
  
```vbs  
Dim FileValid  
  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
'Validate the data file prior to bulkload  
Dim sOutput   
sOutput = ValidateFile("SampleXMLData.xml", "", "SampleSchema.xml")  
WScript.Echo sOutput  
  
If FileValid Then  
   ' Check constraints and initiate transaction (if needed)  
   ' objBL.CheckConstraints = True  
   ' objBL.Transaction=True  
  'Execute XML bulkload using file.  
  objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
  set objBL=Nothing  
End If  
  
Function ValidateFile(strXmlFile,strUrn,strXsdFile)  
  
   ' Create a schema cache and add SampleSchema.xml to it.  
   Dim xs, fso, sAppPath  
   Set fso = CreateObject("Scripting.FileSystemObject")   
   Set xs = CreateObject("MSXML2.XMLSchemaCache.6.0")  
   sAppPath = fso.GetFolder(".")   
   xs.Add strUrn, sAppPath & "\" & strXsdFile  
  
   ' Create an XML DOMDocument object.  
   Dim xd   
   Set xd = CreateObject("MSXML2.DOMDocument.6.0")  
  
   ' Assign the schema cache to the DOM document.  
   ' schemas collection.  
   Set xd.schemas = xs  
  
   ' Load XML document as DOM document.  
   xd.async = False  
   xd.Load sAppPath & "\" & strXmlFile  
  
   ' Return validation results in message to the user.  
   If xd.parseError.errorCode <> 0 Then  
        ValidateFile = "Validation failed on " & _  
             strXmlFile & vbCrLf & _  
             "=======" & vbCrLf & _  
             "Reason: " & xd.parseError.reason & _  
             vbCrLf & "Source: " & _  
             xd.parseError.srcText & _  
             vbCrLf & "Line: " & _  
             xd.parseError.Line & vbCrLf  
             FileValid = False  
    Else  
        ValidateFile = "Validation succeeded for " & _  
             strXmlFile & vbCrLf & _  
             "========" & _  
             vbCrLf & "Contents to be bulkloaded" & vbCrLf  
             FileValid = True  
    End If  
End Function  
```  
  
## <a name="a-bulk-loading-xml-in-a-table"></a>A. Chargement en masse XML dans une table  
 Cet exemple établit une connexion à l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui est spécifié dans la propriété ConnectionString (MyServer). L’exemple spécifie également l’errorlogfile, propriété. Par conséquent, la sortie d'erreur est enregistrée dans le fichier spécifié (« C:\error.log »), dont l'emplacement peut également être modifié. Notez également que la méthode Execute a comme paramètres à la fois le fichier de schéma de mappage (SampleSchema.xml) et le fichier de données XML (SampleXMLData.xml). Lorsque le chargement en masse s’exécute, la table Cust que vous avez créé dans **tempdb** base de données contient de nouveaux enregistrements basés sur le contenu du fichier de données XML.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Pour tester un exemple de chargement en masse  
  
1.  Créez cette table :  
  
    ```sql  
    CREATE TABLE Cust(CustomerID  int PRIMARY KEY,  
                      CompanyName varchar(20),  
                      City        varchar(20));  
    GO  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez à ce fichier le schéma XSD suivant :  
  
    ```xml  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
       <xsd:element name="ROOT" sql:is-constant="1" >  
         <xsd:complexType>  
           <xsd:sequence>  
             <xsd:element name="Customers" sql:relation="Cust" maxOccurs="unbounded">  
               <xsd:complexType>  
                 <xsd:sequence>  
                   <xsd:element name="CustomerID"  type="xsd:integer" />  
                   <xsd:element name="CompanyName" type="xsd:string" />  
                   <xsd:element name="City"        type="xsd:string" />  
                 </xsd:sequence>  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
          </xsd:complexType>  
         </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez à ce fichier le document XML suivant :  
  
    ```xml  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Sean Chai</CompanyName>  
        <City>New York</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Tom Johnston</CompanyName>  
         <City>Los Angeles</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Institute of Art</CompanyName>  
        <City>Chicago</City>  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez à ce fichier le code VBScript fourni ci-dessus au début de cette rubrique. Modifiez la chaîne de connexion pour fournir le nom de serveur approprié. Spécifiez le chemin d’accès approprié pour les fichiers qui sont spécifiés comme paramètres de la méthode Execute.  
  
5.  Exécutez le code VBScript. La fonctionnalité de chargement en masse XML charge les données XML dans la table Cust.  
  
 Voici le schéma XDR équivalent :  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers"  sql:relation="Cust" >  
      <element type="CustomerID"  sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City"        sql:field="City" />  
  
   </ElementType>  
</Schema>  
```  
  
## <a name="b-bulk-loading-xml-data-in-multiple-tables"></a>B. Chargement en masse des données XML dans plusieurs tables  
 Dans cet exemple, le document XML comprend les  **\<client >** et  **\<ordre >** éléments.  
  
```xml  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Sean Chai</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Tom Johnston</CompanyName>  
     <City>LA</City>    
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Institute of Art</CompanyName>  
    <Order OrderID="4" />  
  </Customers>  
</ROOT>  
```  
  
 Cet exemple charge en masse les données XML dans les deux tables, **Cust** et **CustOrder**:  
  
-   Client (CustomerID, CompanyName, City)  
  
-   CustOrder (OrderID, CustomerID)  
  
 Le schéma XSD suivant définit la vue XML de ces tables. Le schéma spécifie la relation parent-enfant entre les  **\<client >** et  **\<ordre >** éléments.  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
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
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
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
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Chargement en masse XML utilise la relation clé primaire/étrangère clée spécifiée ci-dessus entre le  **\<Cust >** et  **\<CustOrder >** éléments en masse chargement les données dans les deux tables.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Pour tester un exemple de chargement en masse  
  
1.  Créez deux tables dans **tempdb** base de données :  
  
    ```sql  
    USE tempdb;  
    CREATE TABLE Cust(  
           CustomerID  int PRIMARY KEY,  
           CompanyName varchar(20),  
           City        varchar(20));  
    CREATE TABLE CustOrder(        OrderID     int PRIMARY KEY,   
            CustomerID int FOREIGN KEY REFERENCES Cust(CustomerID));  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez le schéma XSD fourni dans cet exemple au fichier.  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleData.xml. Ajoutez le document XML fourni précédemment dans cet exemple au fichier.  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez à ce fichier le code VBScript fourni ci-dessus au début de cette rubrique. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Spécifiez le chemin d’accès approprié pour les fichiers qui sont spécifiés comme paramètres de la méthode Execute.  
  
5.  Exécutez le code VBScript ci-dessus. La fonctionnalité de chargement en masse XML charge le document XML dans les tables Cust et CustOrder.  
  
 Voici le schéma XDR équivalent :  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
<sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
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
  
## <a name="c-using-chain-relationships-in-the-schema-to-bulk-load-xml"></a>C. Utilisation des relations de chaîne dans le schéma pour charger en masse des données XML  
 Cet exemple illustre la façon dont la relation M:N spécifiée dans le schéma de mappage est utilisée par la fonctionnalité de chargement en masse XML pour charger des données dans une table qui représente une relation M:N.  
  
 Considérons par exemple ce schéma XSD :  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Ord"  
          parent-key="OrderID"  
          child="OrderDetail"  
          child-key="OrderID" />  
  
    <sql:relationship name="ODProduct"  
          parent="OrderDetail"  
          parent-key="ProductID"  
          child="Product"  
          child-key="ProductID"   
          inverse="true"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Ord"   
                     sql:key-fields="OrderID" >  
          <xsd:complexType>  
            <xsd:sequence>  
             <xsd:element name="Product"  
                          sql:relation="Product"   
                          sql:key-fields="ProductID"  
                          sql:relationship="OrderOD ODProduct">  
               <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
           <xsd:attribute name="OrderID"   type="xsd:integer" />   
           <xsd:attribute name="CustomerID"   type="xsd:string" />  
         </xsd:complexType>  
       </xsd:element>  
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Le schéma spécifie un  **\<ordre >** élément avec un  **\<produit >** élément enfant. Le  **\<ordre >** élément est mappé à la table Ord et  **\<produit >** élément est mappé à la table Product dans la base de données. La relation de chaîne spécifiée sur le  **\<produit >** élément identifie une relation m : n représentée par la table OrderDetail. (Une commande peut inclure de nombreux produits, et un produit peut être inclus dans de nombreuses commandes.)  
  
 Lorsque vous chargez en masse un document XML avec ce schéma, les enregistrements sont ajoutés aux tables Ord, Product et OrderDetail.  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez trois tables :  
  
    ```sql  
    CREATE TABLE Ord (  
             OrderID     int  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  Enregistrez le schéma fourni ci-dessus dans cet exemple sous le nom SampleSchema.xml.  
  
3.  Enregistrez l'exemple de données XML ci-après sous le nom SampleXMLData.xml :  
  
    ```xml  
    <ROOT>    
      <Order OrderID="1" CustomerID="ALFKI">  
        <Product ProductID="1" ProductName="Chai" />  
        <Product ProductID="2" ProductName="Chang" />  
      </Order>  
      <Order OrderID="2" CustomerID="ANATR">  
        <Product ProductID="3" ProductName="Aniseed Syrup" />  
        <Product ProductID="4" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez à ce fichier le code VBScript fourni ci-dessus au début de cette rubrique. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Supprimez les marques de commentaire des lignes suivantes dans le code source de cet exemple.  
  
    ```vbs  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    ```  
  
5.  Exécutez le code VBScript. La fonctionnalité de chargement en masse XML charge le document XML dans les tables Ord et Product.  
  
## <a name="d-bulk-loading-in-identity-type-columns"></a>D. Chargement en masse dans des colonnes de type identity  
 Cet exemple montre comment la fonctionnalité de chargement en masse gère les colonnes de type identity. Dans cet exemple, les données sont chargées en masse dans trois tables (Ord, Product et OrderDetail).  
  
 Dans ces tables :  
  
-   OrderID dans la table Ord est une colonne de type identity.  
  
-   ProductID dans la table Product est une colonne de type identity.  
  
-   Les colonnes OrderID et ProductID dans la table OrderDetail sont des colonnes de clés étrangères qui font référence aux colonnes de clés primaires correspondantes dans les tables Ord et Product.  
  
 Voici les schémas de table de cet exemple :  
  
```  
Ord (OrderID, CustomerID)  
Product (ProductID, ProductName)  
OrderDetail (OrderID, ProductID)  
```  
  
 Dans cet exemple de chargement en masse XML, la propriété KeepIdentity du modèle d’objet de chargement en masse a la valeur false. Par conséquent, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] génère des valeurs d'identité pour les colonnes ProductID et OrderID des tables Product et Ord, respectivement (toutes les valeurs fournies dans les documents à charger en masse sont ignorées).  
  
 Dans ce cas, la fonctionnalité de chargement en masse XML identifie la relation clé primaire/clé étrangère qui existe entre les tables. La fonctionnalité de chargement en masse insère au préalable des enregistrements dans les tables ayant la clé primaire, puis propage la valeur d'identité générée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aux tables ayant des colonnes de clés étrangères. Dans l'exemple suivant, la fonctionnalité de chargement en masse XML insère des données dans les tables dans cet ordre :  
  
1.  Product  
  
2.  Ord  
  
3.  OrderDetail  
  
    > [!NOTE]  
    >  Pour propager les valeurs d'identité générées dans les tables Products et Orders, la logique de traitement nécessite que la fonctionnalité de chargement en masse XML effectue le suivi de ces valeurs pour une insertion ultérieure dans la table OrderDetails. Pour ce faire, la fonctionnalité de chargement en masse XML crée des tables intermédiaires, remplit les données de ces tables, puis les supprime ultérieurement.  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez les tables suivantes :  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez ce schéma XSD à ce fichier.  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
     <xsd:annotation>  
       <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
       </xsd:appinfo>  
     </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez le document XML suivant :  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez le code VBScript suivant à ce fichier. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Spécifiez le chemin d’accès approprié pour les fichiers qui font Office de paramètres pour le **Execute** (méthode).  
  
    ```  
    Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "C:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction = False  
    objBL.KeepIdentity = False  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    Set objBL = Nothing  
    MsgBox "Done."  
    ```  
  
5.  Exécutez le code VBScript. La fonctionnalité de chargement en masse XML charge les données dans les tables appropriées.  
  
## <a name="e-generating-table-schemas-before-bulk-loading"></a>E. Génération de schémas de table avant le chargement en masse  
 La fonctionnalité de chargement en masse XML peut éventuellement générer les tables, si ces dernières n'existent pas avant le chargement en masse. La définition de la propriété SchemaGen de l’objet SQLXMLBulkLoad does TRUE cette. Vous pouvez également demander le chargement en masse XML pour supprimer des tables existantes et de les recréer en définissant la sgdroptables, propriété sur TRUE. L'exemple VBScript ci-dessous illustre l'utilisation de ces propriétés.  
  
 Par ailleurs, cet exemple définit deux propriétés supplémentaires à TRUE :  
  
-   CheckConstraints. La définition de cette propriété à TRUE garantit que les données insérées dans les tables ne violent pas les contraintes spécifiées sur les tables (dans le cas présent, il s'agit des contraintes PRIMARY KEY/FOREIGN KEY spécifiées entre les tables Cust et CustOrder). S'il existe une violation de contrainte, le chargement en masse échoue.  
  
-   XMLFragment. Cette propriété doit être définie à TRUE, car l'exemple de document XML (source de données) ne contient aucun élément unique, de niveau supérieur (il s'agit par conséquent d'un fragment).  
  
 Code VBScript :  
  
```  
Dim objBL   
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
objBL.CheckConstraints=true  
objBL.XMLFragment = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
```  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez au fichier le schéma XSD fourni dans l'exemple antérieur, « Utilisation des relations de chaîne dans le schéma pour charger en masse des données XML ».  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez au fichier le document XML fourni dans l'exemple antérieur, « Utilisation des relations de chaîne dans le schéma pour charger en masse des données XML ». Supprimer le \<racine > élément à partir du document (pour en faire un fragment).  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez le code VBScript de cet exemple à ce fichier. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Spécifiez le chemin d’accès approprié pour les fichiers qui sont spécifiés comme paramètres de la méthode Execute.  
  
4.  Exécutez le code VBScript. La fonctionnalité de chargement en masse XML crée les tables nécessaires d'après le schéma de mappage fourni, puis charge en masse les données dans ce dernier.  
  
## <a name="f-bulk-loading-from-a-stream"></a>F. Chargement en masse à partir d'un flux de données  
 La méthode d’exécution du modèle d’objet de chargement en masse XML accepte deux paramètres. Le premier paramètre est le fichier de schéma de mappage. Le second paramètre fournit les données XML à charger dans la base de données. Il existe deux façons de passer les données XML à la méthode Execute de chargement en masse XML :  
  
-   Spécifiez le nom de fichier en tant que paramètre.  
  
-   Passez un flux qui contient les données XML.  
  
 Cet exemple montre comment effectuer un chargement en masse à partir d'un flux de données.  
  
 VBScript exécute au préalable une instruction SELECT pour récupérer les informations du client de la table Customers dans la base de données Northwind. Dans la mesure où la clause FOR XML est spécifiée (avec l'option ELEMENTS) dans l'instruction SELECT, la requête retourne un document XML centré sur l'élément de ce formulaire :  
  
```  
<Customer>  
  <CustomerID>..</CustomerID>  
  <CompanyName>..</CompanyName>  
  <City>..</City>  
</Customer>  
...  
```  
  
 Le script transmet ensuite le code XML sous forme de flux à la méthode Execute comme deuxième paramètre. Le bloc de méthode Execute charge les données dans la table Cust.  
  
 Étant donné que ce script définit la propriété SchemaGen sur TRUE et sgdroptables, propriété sur TRUE, le chargement en masse XML crée la table Cust dans la base de données spécifié. (Si la table existe déjà, elle est d'abord supprimée puis recréée.)  
  
 Exemple VBScript :  
  
```  
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
Set objCmd = CreateObject("ADODB.Command")  
Set objConn = CreateObject("ADODB.Connection")  
Set objStrmOut = CreateObject ("ADODB.Stream")  
  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile     = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen        = True  
objBL.SGDropTables     = True  
objBL.XMLFragment      = True  
' Open a connection to the instance of SQL Server to get the source data.  
  
objConn.Open "provider=SQLOLEDB;server=(local);database=tempdb;integrated security=SSPI"  
Set objCmd.ActiveConnection = objConn  
objCmd.CommandText = "SELECT CustomerID, CompanyName, City FROM Customers FOR XML AUTO, ELEMENTS"  
  
' Open the return stream and execute the command.  
Const adCRLF = -1  
Const adExecuteStream = 1024  
objStrmOut.Open  
objStrmOut.LineSeparator = adCRLF  
objCmd.Properties("Output Stream").Value = objStrmOut  
objCmd.Execute , , adExecuteStream  
objStrmOut.Position = 0  
  
' Execute bulk load. Read source XML data from the stream.  
objBL.Execute "SampleSchema.xml", objStrmOut  
  
Set objBL = Nothing  
```  
  
 Le schéma de mappage XSD suivant fournit les informations nécessaires pour créer la table :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="ROOT" sql:is-constant="true" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customers"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="Customers" sql:relation="Cust" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element name="CustomerID"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(5)"/>  
      <xsd:element name="CompanyName"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
      <xsd:element name="City"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
</xsd:schema>  
```  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
    </ElementType>  
</Schema>  
```  
  
### <a name="opening-a-stream-on-an-existing-file"></a>Ouverture d'un flux de données sur un fichier existant  
 Vous pouvez également ouvrir un flux de données sur un fichier de données XML existant et passer le flux de données en tant que paramètre à la méthode Execute (au lieu de passer le nom de fichier comme paramètre).  
  
 Voici un exemple Visual Basic de passage d'un flux de données en tant que paramètre :  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad  
Dim objStrm As New ADODB.Stream  
Dim objFileSystem As New Scripting.FileSystemObject  
Dim objFile As Scripting.TextStream  
  
MsgBox "Begin BulkLoad..."  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
' Here again a stream is specified that contains the source data   
' (instead of the file name). But this is just an illustration.  
' Usually this is useful if you have an XML data   
' stream that is created by some other means that you want to bulk   
' load. This example starts with an XML text file, so it may not be the   
' best to use a stream (you can specify the file name directly).  
' Here you could have specified the file name itself.   
Set objFile = objFileSystem.OpenTextFile("c:\SampleData.xml")  
objStrm.Open  
objStrm.WriteText objFile.ReadAll  
objStrm.Position = 0  
objBL.Execute "c:\SampleSchema.xml", objStrm  
  
Set objBL = Nothing  
MsgBox "Done."  
End Sub  
```  
  
 Pour tester l'application, utilisez le document XML suivant dans un fichier (SampleData.xml) et le schéma XSD fourni dans cet exemple :  
  
 Données sources XML (SampleData.xml) :  
  
```  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Hanari Carnes</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Toms Spezialitten</CompanyName>  
     <City>LA</City>  
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Victuailles en stock</CompanyName>  
    <Order CustomerID= "4444" OrderID="4" />  
</Customers>  
</ROOT>  
```  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="g-bulk-loading-in-overflow-columns"></a>G. Chargement en masse dans les colonnes de dépassement  
 Si le schéma de mappage spécifie une colonne de dépassement à l’aide de la **SQL : Overflow-champ** annotation, le chargement en masse XML copie toutes les données non consommées du document source vers cette colonne.  
  
 Examinez ce schéma XSD :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
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
  <xsd:element name="Customers" sql:relation="Cust"  
                                sql:overflow-field="OverflowColumn" >  
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
          <xsd:attribute name="CustomerID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Le schéma identifie une colonne de dépassement (OverflowColumn) pour la table Cust. Par conséquent, tous les non consommées données XML pour chaque  **\<client >** élément est ajouté à cette colonne.  
  
> [!NOTE]  
>  Tous les éléments abstraits (éléments pour lesquels **abstract = « true »** est spécifié) et tous les attributs interdits (attributs pour lesquels **interdite = « true »** est spécifié) sont considérés comme un dépassement par chargement en masse XML et sont ajoutés à la colonne de dépassement de capacité, si spécifié. (Sinon, ils sont ignorés.)  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez deux tables dans **tempdb** base de données :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle',  
                  OverflowColumn nvarchar(200));  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID    int PRIMARY KEY,  
                  CustomerID int FOREIGN KEY   
                                 REFERENCES Cust(CustomerID));  
    GO  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez le schéma XSD fourni dans cet exemple au fichier.  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez le document XML suivant au fichier :  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City><![CDATA[NY]]> </City>  
        <Junk>garbage in overflow</Junk>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <![CDATA[LA]]>   
        <!-- <xyz><address>111 Maple, Seattle</address></xyz>   -->  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez à ce fichier le code Microsoft Visual Basic Scripting Edition (VBScript) suivant. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Spécifiez le chemin d’accès approprié pour les fichiers qui sont spécifiés comme paramètres de la méthode Execute.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Exécutez le code VBScript.  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"   
                       sql:overflow-field="OverflowColumn"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="h-specifying-the-file-path-for-temp-files-in-transaction-mode"></a>H. Spécification du chemin d'accès aux fichiers temporaires en mode de transaction  
 Lorsque vous effectuez un chargement en bloc en mode de transaction (autrement dit, lorsque la propriété de Transaction est définie sur TRUE), vous devez également définir la propriété TempFilePath lorsqu’une des conditions suivantes est vraie :  
  
-   Vous effectuez un chargement en masse sur un serveur distant.  
  
-   Vous souhaitez utiliser un autre dossier ou lecteur local (différent du chemin d'accès spécifié par la variable d'environnement TEMP) pour stocker les fichiers temporaires créés en mode de transaction.  
  
 Par exemple, le code VBScript suivant effectue un chargement en masse des données à partir du fichier SampleXMLData.xml dans les tables de base de données en mode de transaction. La propriété TempFilePath est spécifiée pour définir le chemin d’accès pour les fichiers temporaires générés en mode de transaction.  
  
```  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.Transaction=True  
objBL.TempFilePath="\\Server\MyDir"  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
set objBL=Nothing  
```  
  
> [!NOTE]  
>  Le chemin d'accès aux fichiers temporaires doit être un emplacement partagé accessible au compte de service de l'instance cible de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et au compte exécutant l'application de chargement en masse. Sauf si vous effectuez un chargement en bloc sur un serveur local, le chemin d’accès de fichier temporaire doit être un chemin d’accès UNC (tel que \\\servername\sharename).  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez cette table dans **tempdb** base de données :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (     CustomerID uniqueidentifier,   
          LastName  varchar(20));  
    GO  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez au fichier le schéma XSD suivant :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez le document XML suivant au fichier :  
  
    ```  
    <ROOT>  
    <Customers CustomerID="6F9619FF-8B86-D011-B42D-00C04FC964FF"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom ValidateAndBulkload.vbs. Ajoutez le code VBScript suivant à ce fichier. Modifiez la chaîne de connexion pour fournir le nom de serveur et le nom de base de données appropriés. Spécifiez le chemin d’accès approprié pour les fichiers qui sont spécifiés comme paramètres de la méthode Execute. Également spécifier le chemin d’accès approprié pour la propriété TempFilePath.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.TempFilePath="\\server\folder"  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Exécutez le code VBScript.  
  
     Le schéma doit spécifier correspondant **SQL : DataType** pour le **CustomerID** attribut lorsque la valeur de **CustomerID** est spécifié comme un GUID qui inclut des accolades ({et}), telles que :  
  
    ```  
    <ROOT>  
    <Customers CustomerID="{6F9619FF-8B86-D011-B42D-00C04FC964FF}"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
     Voici le schéma mis à jour :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string"   
                        sql:datatype="uniqueidentifier" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
     Lorsque **SQL : DataType** est spécifié en identifiant le type de colonne en tant que **uniqueidentifier**, l’opération de chargement en masse supprime les accolades ({et}) à partir de la **CustomerID** valeur avant l’insertion dans la colonne.  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
<ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
</ElementType>  
<ElementType name="Customers" sql:relation="Cust" >  
  <AttributeType name="CustomerID"  sql:datatype="uniqueidentifier" />  
  <AttributeType name="LastName"   />  
  
  <attribute type="CustomerID" />  
  <attribute type="LastName"   />  
</ElementType>  
</Schema>  
```  
  
## <a name="i-using-an-existing-database-connection-with-the-connectioncommand-property"></a>I. Utilisation d'une connexion de base de données existante avec la propriété ConnectionCommand  
 Vous pouvez utiliser une connexion ADO existante pour effectuer un chargement en masse XML. Cela s'avère utile si le chargement en masse XML n'est que l'une des nombreuses opérations à effectuer sur une source de données.  
  
 La propriété ConnectionCommand vous permet d’utiliser une connexion ADO existante à l’aide d’un objet command ADO. L'exemple Visual Basic suivant illustre ce concept :  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad4  
Dim objCmd As New ADODB.Command  
Dim objConn As New ADODB.Connection  
  
'Open a connection to an instance of SQL Server.  
objConn.Open "provider=SQLOLEDB;data source=(local);database=tempdb;integrated security=SSPI"  
'Ask the Command object to use the connection just established.  
Set objCmd.ActiveConnection = objConn  
  
'Tell Bulk Load to use the active command object that is using the Connection obj.  
objBL.ConnectionCommand = objCmd  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
'The Transaction property must be set to True if you use ConnectionCommand.  
objBL.Transaction = True  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
End Sub  
```  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez deux tables dans **tempdb** base de données :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust(  
                   CustomerID   varchar(5) PRIMARY KEY,  
                   CompanyName  varchar(30),  
                   City         varchar(20));  
    GO  
    CREATE TABLE CustOrder(  
                   CustomerID  varchar(5) references Cust (CustomerID),  
                   OrderID     varchar(5) PRIMARY KEY);  
    GO  
    ```  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Ajoutez au fichier le schéma XSD suivant :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
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
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
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
              <xsd:attribute name="CustomerID" type="xsd:integer" />  
             </xsd:complexType>  
           </xsd:element>  
         </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Ajoutez le document XML suivant au fichier :  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Créez une application Visual Basic (Standard EXE) et le code précédent. Ajoutez ces références au projet :  
  
    ```  
    Microsoft XML BulkLoad for SQL Server 4.0 Type Library  
    Microsoft ActiveX Data objects 2.6 Library  
    ```  
  
5.  Exécutez l’application.  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
         <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
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
  
## <a name="j-bulk-loading-in-xml-data-type-columns"></a>J. Chargement en masse dans les colonnes de type de données xml  
 Si le schéma de mappage spécifie une [type de données xml](../../../t-sql/xml/xml-transact-sql.md) colonne à l’aide de la **SQL : DataType = « xml »** annotation, le chargement en masse XML peut copier des éléments enfants XML pour le champ mappé à partir du document source vers cette colonne.  
  
 Examinez le schéma XSD suivant, qui mappe une vue de la table Production.ProductModel dans l'exemple de base de données AdventureWorks. Dans ce tableau, le champ CatalogDescription de **xml** type de données est mappé à un  **\<Desc >** à l’aide de l’élément le **SQL : Field** et **SQL : DataType = « xml »** annotations.  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Name" type="xs:string"></xsd:element>  
        <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element name="ProductDescription">  
              <xsd:complexType>  
                <xsd:sequence>  
                  <xsd:element name="Summary" type="xs:anyType"/>  
                </xsd:sequence>  
              </xsd:complexType>  
            </xsd:element>  
          </xsd:sequence>  
        </xsd:complexType>  
        </xsd:element>   
     </xsd:sequence>  
     <xsd:attribute name="ProductModelID" sql:field="ProductModelID" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Assurez-vous que l'exemple de base de données AdventureWorks est installé.  
  
2.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleSchema.xml. Copiez le schéma XSD ci-dessus, puis collez-le dans le fichier et enregistrez ce dernier.  
  
3.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom SampleXMLData.xml. Copiez le document XML ci-dessous et collez-le dans le fichier, puis enregistrez ce dernier dans le même dossier que celui utilisé pour l'étape précédente.  
  
    ```  
    <ProductModel ProductModelID="2005">  
        <Name>Mountain-100 (2005 model)</Name>  
        <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
            <p1:ProductDescription xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
                  xmlns:wm="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
                  xmlns:wf="http://www.adventure-works.com/schemas/OtherFeatures"   
                  xmlns:html="http://www.w3.org/1999/xhtml"   
                  xmlns="">  
                <p1:Summary>  
                    <html:p>Our top-of-the-line competition mountain bike.   
          Performance-enhancing options include the innovative HL Frame,   
          super-smooth front suspension, and traction for all terrain.  
                            </html:p>  
                </p1:Summary>  
                <p1:Manufacturer>  
                    <p1:Name>AdventureWorks</p1:Name>  
                    <p1:Copyright>2002-2005</p1:Copyright>  
                    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
                </p1:Manufacturer>  
                <p1:Features>These are the product highlights.   
                     <wm:Warranty>  
                        <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
                        <wm:Description>parts and labor</wm:Description>  
                    </wm:Warranty><wm:Maintenance>  
                        <wm:NoOfYears>10 years</wm:NoOfYears>  
                        <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
                    </wm:Maintenance><wf:wheel>High performance wheels.</wf:wheel><wf:saddle>  
                        <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle><wf:pedal>  
                        <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal><wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter   
          and wall-thickness required of a premium mountain frame.   
          The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame><wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset></p1:Features>  
                <!-- add one or more of these elements... one for each specific product in this product model -->  
                <p1:Picture>  
                    <p1:Angle>front</p1:Angle>  
                    <p1:Size>small</p1:Size>  
                    <p1:ProductPhotoID>118</p1:ProductPhotoID>  
                </p1:Picture>  
                <!-- add any tags in <specifications> -->  
                <p1:Specifications> These are the product specifications.  
                       <Material>Almuminum Alloy</Material><Color>Available in most colors</Color><ProductLine>Mountain bike</ProductLine><Style>Unisex</Style><RiderExperience>Advanced to Professional riders</RiderExperience></p1:Specifications>  
            </p1:ProductDescription>  
        </Desc>  
    </ProductModel>  
    ```  
  
4.  Créez un fichier dans votre éditeur de texte ou éditeur XML par défaut, puis enregistrez-le sous le nom BulkloadXml.vbs. Copiez le code VBScript suivant et collez-le dans le fichier. Enregistrez-le dans le même dossier que celui utilisé pour les fichiers de schéma et de données XML.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=AdventureWorks;integrated security=SSPI"  
  
    Dim fso, sAppPath  
    Set fso = CreateObject("Scripting.FileSystemObject")   
    sAppPath = fso.GetFolder(".")   
  
    objBL.ErrorLogFile = sAppPath & "\error.log"  
  
    'Execute XML bulkload using file.  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Exécutez le script BulkloadXml.vbs.  
  
  
