---
title: Enregistrer le processus de génération (SQLXML 4.0) | Documents Microsoft
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
- XML Bulk Load [SQLXML], record generation process
- node scopes [SQLXML]
- record subsets [SQLXML]
- scope [SQLXML]
- key ordering rules [SQLXML]
- record generation process for bulk loads [SQLXML]
- entering node scope [SQLXML]
- bulk load [SQLXML], record generation process
- leaving node scope [SQLXML]
- schema mapping [SQLXML]
ms.assetid: d8885bbe-6f15-4fb9-9684-ca7883cfe9ac
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 59ec68c1c2c321f45940eace9ef1f6a3dc9be9c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="record-generation-process-sqlxml-40"></a>Processus de génération d'enregistrements (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le chargement en masse XML traite les données d'entrée XML et prépare des enregistrements pour les tables appropriées dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La logique de la fonctionnalité de chargement en masse XML détermine quand générer un nouvel enregistrement, quelles valeurs d'attributs ou d'éléments enfants copier dans les champs de l'enregistrement et quand l'enregistrement est terminé et prêt à être envoyé à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour insertion.  
  
 Le chargement en masse XML ne charge pas toutes les données d'entrée XML dans la mémoire et ne produit pas de jeux d'enregistrements complets avant d'envoyer les données à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cela tient au fait que les données d'entrée XML peuvent correspondre à un document volumineux et que le chargement du document entier en mémoire peut s'avérer onéreux. À la place, le chargement en masse XML effectue les opérations suivantes :  
  
1.  Il analyse le schéma de mappage et prépare le plan d'exécution nécessaire.  
  
2.  Il applique le plan d'exécution aux données dans le flux d'entrée.  
  
 Ce traitement séquentiel fait qu'il est important de fournir les données d'entrée XML d'une manière spécifique. Vous devez comprendre comment le chargement en masse XML analyse le schéma de mappage et comment le processus de génération d'enregistrements se produit. Comprendre cela vous permet de fournir un schéma de mappage au chargement en masse XML qui produit les résultats que vous souhaitez.  
  
 Le chargement en masse XML gère les annotations de schéma de mappage courantes, y compris les mappages de colonne et de table (spécifiés explicitement à l'aide d'annotations ou implicitement par le biais du mappage par défaut), ainsi que les relations de jointure.  
  
> [!NOTE]  
>  Cette rubrique suppose que vous connaissez bien les schémas de mappage XDR ou XSD annotés. Pour plus d’informations sur les schémas, consultez [Introduction aux schémas de XSD annoté &#40;SQLXML 4.0&#41; ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md) ou [de schémas XDR annotés &#40;déconseillé dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
 Pour comprendre la génération d'enregistrements, vous devez comprendre les concepts suivants :  
  
-   Étendue d'un nœud  
  
-   Règle de génération d'enregistrements  
  
-   Sous-ensemble d'enregistrements et règle de tri par clé  
  
-   Exceptions à la règle de génération d'enregistrements  
  
## <a name="scope-of-a-node"></a>Étendue d’un nœud  
 Un nœud (un élément ou attribut) dans un document XML entre *dans l’étendue* lorsque le chargement en masse XML le rencontre dans le flux de données d’entrée XML. Pour un nœud d'élément, la balise de début de l'élément place l'élément dans l'étendue. Pour un nœud d'attribut, le nom d'attribut place l'attribut dans l'étendue.  
  
 Un nœud quitte l'étendue lorsqu'il ne reste plus de données pour lui : soit au niveau de la balise de fin (dans le cas d'un nœud d'élément), soit à la fin d'une valeur d'attribut (dans le cas d'un nœud d'attribut).  
  
## <a name="record-generation-rule"></a>Règle de génération d'enregistrements  
 Lorsqu'un nœud (élément ou attribut) entre dans l'étendue, il est possible de générer un enregistrement à partir de ce nœud. L'enregistrement dure tant que le nœud associé est dans l'étendue. Lorsque le nœud sort de l'étendue, le chargement en masse XML considère l'enregistrement généré comme complet (avec les données) et l'envoie à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour insertion.  
  
 Considérons, par exemple, le fragment de schéma XSD suivant :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Customers" >  
   <xsd:complexType>  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
     <xsd:attribute name="CompanyName" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Le schéma spécifie un  **\<client >** élément avec **CustomerID** et **CompanyName** attributs. Le **SQL : relation** annotation mappe le  **\<client >** élément à la table Customers.  
  
 Considérez ce fragment d'un document XML :  
  
```  
<Customer CustomerID="1" CompanyName="xyz" />  
<Customer CustomerID="2" CompanyName="abc" />  
...  
```  
  
 Lorsque le chargement en masse XML est fourni avec le schéma décrit dans les paragraphes précédents et les données XML comme entrée, il traite les nœuds (éléments et attributs) dans les données sources comme suit :  
  
-   La balise de début de la première  **\<client >** élément place cet élément dans l’étendue. Ce nœud est mappé à la table Customers. Par conséquent, le chargement en masse XML génère un enregistrement pour la table Customers.  
  
-   Dans le schéma, tous les attributs de la  **\<client >** sont mappés aux colonnes de la table Customers. Lorsque ces attributs entrent dans l'étendue, le chargement en masse XML copie leurs valeurs dans l'enregistrement de client qui est déjà généré par l'étendue parente.  
  
-   Lorsque le chargement en masse XML atteint la balise de fin pour le  **\<client >** élément, l’élément est hors de portée. Cela conduit le chargement en masse XML à considérer l'enregistrement comme complet et à l'envoyer à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Chargement en masse XML suit ce processus pour chaque ultérieures  **\<client >** élément.  
  
> [!IMPORTANT]  
>  Dans ce modèle, comme un enregistrement est inséré lorsque la balise de fin est atteinte (ou lorsque le nœud est hors de portée), vous devez définir toutes les données associées à l'enregistrement dans l'étendue du nœud.  
  
## <a name="record-subset-and-the-key-ordering-rule"></a>Sous-ensemble d’enregistrements et de la clé de tri de règle  
 Lorsque vous spécifiez un schéma de mappage qui utilise  **\<SQL : Relationship >**, le terme de sous-ensemble fait référence au jeu d’enregistrements qui est généré sur le côté étranger de la relation. Dans l’exemple suivant, les enregistrements CustOrder sont sur le côté étranger,  **\<SQL : Relationship >**.  
  
 Prenons l'exemple d'une base de données contenant les tables suivantes :  
  
-   Cust (CustomerID, CompanyName, City)  
  
-   CustOrder (CustomerID, OrderID)  
  
 CustomerID dans la table CustOrder est une clé étrangère qui fait référence à la clé primaire CustomerID dans la table Cust.  
  
 À présent, considérez la vue XML telle qu'elle est spécifiée dans le schéma XSD annoté ci-dessous. Ce schéma utilise  **\<SQL : Relationship >** pour spécifier la relation entre les tables Cust et CustOrder.  
  
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
  
 L'exemple de données XML et les étapes de création d'un exemple fonctionnel sont fournis ci-dessous.  
  
-   Lorsqu’un  **\<client >** nœud d’élément dans le fichier de données XML entre dans l’étendue, le chargement en masse XML génère un enregistrement pour la table Cust. Chargement en masse XML copie ensuite les valeurs de colonne nécessaires (CustomerID, CompanyName et City) à partir de la  **\<CustomerID >**,  **\<CompanyName >** et le  **\<ville >** des éléments enfants en tant que ces éléments entrent dans l’étendue.  
  
-   Lorsqu’un  **\<ordre >** nœud d’élément entre dans l’étendue, le chargement en masse XML génère un enregistrement pour la table CustOrder. Chargement en masse XML copie la valeur de la **OrderID** d’attribut à cet enregistrement. La valeur requise pour la colonne CustomerID est obtenue à partir de la  **\<CustomerID >** élément enfant de le  **\<client >** élément. Chargement en masse XML utilise les informations qui sont spécifiées dans  **\<SQL : Relationship >** pour obtenir la valeur de clé étrangère CustomerID pour cet enregistrement, à moins que le **CustomerID** attribut a été spécifié dans le  **\<ordre >** élément. La règle générale est que, si l’élément enfant spécifie explicitement une valeur pour l’attribut de clé étrangère, chargement en masse XML utilise cette valeur et n’obtient pas la valeur de l’élément parent en utilisant le  **\<SQL : Relationship >**. Comme cela  **\<ordre >** nœud d’élément est hors de portée, le chargement en masse XML envoie l’enregistrement à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puis traite tous les  **\<ordre >** nœuds d’élément de la même manière.  
  
-   Enfin, le  **\<client >** nœud d’élément est hors de portée. À ce stade, le chargement en masse XML envoie l'enregistrement de client à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le chargement en masse XML suit ce processus pour tous les clients suivants dans le flux de données XML.  
  
 Voici deux observations à propos du schéma de mappage :  
  
-   Lorsque le schéma satisfait la règle « imbrication » (par exemple, toutes les données qui sont associées avec le client et l’ordre est défini dans l’étendue de la  **\<client >** et  **\<ordre >** nœuds d’élément), le chargement en masse réussit.  
  
-   Dans la description de la  **\<client >** élément, les éléments sont spécifiés dans l’ordre approprié de ses enfants. Dans ce cas, le  **\<CustomerID >** élément enfant est spécifié avant le  **\<ordre >** élément enfant. Cela signifie que dans le fichier de données XML d’entrée, la  **\<CustomerID >** valeur de l’élément n’est disponible en tant que la clé étrangère valeur lorsque le  **\<ordre >** élément entre dans l’étendue. Les attributs de clé sont spécifiés en premier ; ceci est la « règle de tri par clé ».  
  
     Si vous spécifiez la  **\<CustomerID >** élément enfant après le  **\<ordre >** élément enfant, la valeur n’est pas disponible lorsque le  **\<ordre >** élément entre dans l’étendue. Lorsque le  **\</Order >** la balise de fin est ensuite lue, l’enregistrement de la table CustOrder est considérée comme terminée et qu’il est inséré dans la table CustOrder avec une valeur NULL pour la colonne CustomerID, qui n’est pas le résultat souhaité.  
  
#### <a name="to-create-a-working-sample"></a>Pour créer un exemple fonctionnel  
  
1.  Enregistrez le schéma fourni dans cet exemple sous le nom SampleSchema.xml.  
  
2.  Créez les tables suivantes :  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                 OrderID        int         PRIMARY KEY,  
                 CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
3.  Enregistrez l'exemple de données d'entrée XML ci-après sous le nom SampleXMLData.xml :  
  
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
  
4.  Pour effectuer le chargement en masse XML, enregistrez et exécutez l'exemple [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) suivant (BulkLoad.vbs) :  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
## <a name="exceptions-to-the-record-generation-rule"></a>Exceptions à la règle de génération d'enregistrements  
 Le chargement en masse XML ne génère pas d'enregistrement pour un nœud lorsqu'il entre dans l'étendue si ce nœud est de type IDREF ou IDREFS. Vous devez vous assurer qu'une description complète de l'enregistrement se produit à un point quelconque dans le schéma. Le **dt : type = « nmtokens »** annotations sont ignorées tout comme le type IDREFS est ignoré.  
  
 Par exemple, considérez le schéma XSD suivant décrivant  **\<client >** et  **\<ordre >** éléments. Le  **\<client >** élément inclut un **OrderList** attribut de type IDREFS. Le  **\<SQL : Relationship >** balise spécifie la relation un-à-plusieurs entre le client et une liste de commandes.  
  
 Voici le schéma :  
  
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
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="CompanyName" type="xsd:string" />  
    <xsd:attribute name="City" type="xsd:string" />  
    <xsd:attribute name="OrderList"   
                       type="xsd:IDREFS"   
                       sql:relation="CustOrder"   
                       sql:field="OrderID"  
                       sql:relationship="CustCustOrder" >  
    </xsd:attribute>  
  </xsd:complexType>  
 </xsd:element>  
  
  <xsd:element name="Order" sql:relation="CustOrder" >  
   <xsd:complexType>  
    <xsd:attribute name="OrderID" type="xsd:string" />  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="OrderDate" type="xsd:date" />  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Chargement en masse n’ignore les nœuds de type IDREFS, il n’existe aucune génération d’enregistrement lorsque le **OrderList** nœud d’attribut entre dans l’étendue. Par conséquent, si vous souhaitez que les enregistrements de commandes soient ajoutés à la table Orders, vous devez décrire ces commandes quelque part dans le schéma. Dans ce schéma, en spécifiant le  **\<ordre >** élément garantit que le chargement en masse XML ajoute les enregistrements de commande dans la table Orders. Le  **\<ordre >** élément décrit tous les attributs qui sont requis pour remplir l’enregistrement pour la table CustOrder.  
  
 Vous devez vous assurer que le **CustomerID** et **OrderID** des valeurs dans le  **\<client >** élément correspondent aux valeurs dans le  **\<ordre >** élément. Vous êtes chargé de maintenir l'intégrité référentielle.  
  
#### <a name="to-test-a-working-sample"></a>Pour tester un exemple fonctionnel  
  
1.  Créez les tables suivantes :  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
                  CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
                  CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID),  
                  OrderDate      datetime DEFAULT '2000-01-01')  
    GO  
    ```  
  
2.  Enregistrez le schéma de mappage fourni dans cet exemple sous le nom SampleSchema.xml.  
  
3.  Enregistrez l'exemple de données XML ci-après sous le nom SampleXMLData.xml :  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1111" CompanyName="Sean Chai" City="NY"  
                 OrderList="Ord1 Ord2" />  
      <Customers CustomerID="1112" CompanyName="Dont Know" City="LA"  
                 OrderList="Ord3 Ord4" />  
      <Order OrderID="Ord1" CustomerID="1111" OrderDate="1999-01-01" />  
      <Order OrderID="Ord2" CustomerID="1111" OrderDate="1999-02-01" />  
      <Order OrderID="Ord3" CustomerID="1112" OrderDate="1999-03-01" />  
      <Order OrderID="Ord4" CustomerID="1112" OrderDate="1999-04-01" />  
    </ROOT>  
    ```  
  
4.  Pour exécuter le chargement en masse XML, enregistrez et exécutez cet exemple VBScript (SampleVB.vbs) :  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  
