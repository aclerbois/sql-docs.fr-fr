---
title: Exemples de DiffGrams (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], examples
- examples [SQLXML], DiffGram
- diffgr:parentID
- parentID annotation
ms.assetid: fc148583-dfd3-4efb-a413-f47b150b0975
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1ad818de46852201fc2bc111684b461d5c15ada3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="diffgram-examples-sqlxml-40"></a>Exemples de DiffGrams (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique contient des exemples de DiffGrams qui réalisent des opérations d'insertion, de mise à jour et de suppression dans la base de données. Avant d'utiliser les exemples, notez les points suivants :  
  
-   Les exemples utilisent deux tables (Cust et Ord) qui doivent être créées si vous souhaitez tester les exemples de DiffGrams :  
  
    ```  
    Cust(CustomerID, CompanyName, ContactName)  
    Ord(OrderID, CustomerID)  
    ```  
  
-   La plupart des exemples de cette rubrique utilise le schéma XSD suivant :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
    <xsd:annotation>  
      <xsd:documentation>  
        Diffgram Customers/Orders Schema.  
      </xsd:documentation>  
      <xsd:appinfo>  
           <sql:relationship name="CustomersOrders"   
                      parent="Cust"  
                      parent-key="CustomerID"  
                      child-key="CustomerID"  
                      child="Ord"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
    <xsd:element name="Customer" sql:relation="Cust">  
      <xsd:complexType>  
        <xsd:sequence>  
          <xsd:element name="CompanyName"    type="xsd:string"/>  
          <xsd:element name="ContactName"    type="xsd:string"/>  
           <xsd:element name="Order" sql:relation="Ord" sql:relationship="CustomersOrders">  
            <xsd:complexType>  
              <xsd:attribute name="OrderID" type="xsd:int" sql:field="OrderID"/>  
              <xsd:attribute name="CustomerID" type="xsd:string"/>  
            </xsd:complexType>  
          </xsd:element>  
        </xsd:sequence>  
        <xsd:attribute name="CustomerID" type="xsd:string" sql:field="CustomerID"/>  
      </xsd:complexType>  
    </xsd:element>  
  
    </xsd:schema>     
    ```  
  
     Enregistrez ce schéma sous le nom DiffGramSchema.xml dans le dossier où vous enregistrez les autres fichiers utilisés dans les exemples.  
  
## <a name="a-deleting-a-record-by-using-a-diffgram"></a>A. Suppression d'un enregistrement à l'aide d'un DiffGram  
 Le DiffGram de cet exemple supprime un client (dont CustomerID a la valeur ALFKI) de la table Cust et supprime l'enregistrement de commande correspondant (dont OrderID a la valeur 1) de la table Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance/>  
  
    <diffgr:before>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI"   
               OrderID="1"/>  
        <Customer diffgr:id="Customer1"   
                  msdata:rowOrder="0"   
                  CustomerID="ALFKI">  
           <CompanyName>Alfreds Futterkiste</CompanyName>  
           <ContactName>Maria Anders</ContactName>  
        </Customer>  
    </diffgr:before>  
    <msdata:errors/>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Dans le  **\<avant >** de bloc, il est un  **\<ordre >** élément (**diffgr : ID = « Order1 »**) et un  **\<client >** élément (**diffgr : ID = « Customer1 »**). Ces éléments représentent les enregistrements existants de la base de données. Le  **\<DataInstance >** élément n’a pas les enregistrements correspondants (avec le même **diffgr : ID**). Cela indique une opération de suppression.  
  
#### <a name="to-test-the-diffgram"></a>Pour tester le DiffGram  
  
1.  Création de ces tables dans le **tempdb** base de données.  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Ajoutez les exemples de données suivants :  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiez le DiffGram ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MyDiffGram.xml dans le même dossier qu'à l'étape précédente.  
  
4.  Copiez le DiffGramSchema fourni plus haut dans cette rubrique et collez-le dans un fichier texte. Enregistrez le fichier sous le nom DiffGramSchema.xml.  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le DiffGram.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="b-inserting-a-record-by-using-a-diffgram"></a>B. Insertion d'un enregistrement à l'aide d'un DiffGram  
 Dans cet exemple, le DiffGram insère un enregistrement dans la table Cust et un enregistrement dans la table Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"   
      sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
          xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
          xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
       <Customer diffgr:id="Customer1" msdata:rowOrder="0"    
                 diffgr:hasChanges="inserted" CustomerID="ALFKI">  
        <CompanyName>C3Company</CompanyName>  
        <ContactName>C3Contact</ContactName>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"  
               diffgr:hasChanges="inserted"   
               CustomerID="ALFKI" OrderID="1"/>  
      </Customer>  
    </DataInstance>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Dans ce DiffGram le  **\<avant >** bloc n’est pas spécifié (aucune base de données existant enregistre identifié). Il existe deux instances d’enregistrements (identifiées par le  **\<client >** et  **\<ordre >** éléments dans le  **\<DataInstance >** bloc) qui mappent aux tables Cust et Ord, respectivement. Deux de ces éléments spécifient la **diffgr : HasChanges** attribut (**hasChanges = « inserted »**). Cela indique une opération d'insertion. Dans ce DiffGram, si vous spécifiez **hasChanges = « modified »**, vous indiquez que vous souhaitez modifier un enregistrement qui n’existe pas, ce qui entraîne une erreur.  
  
#### <a name="to-test-the-diffgram"></a>Pour tester le DiffGram  
  
1.  Création de ces tables dans le **tempdb** base de données.  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Ajoutez les exemples de données suivants :  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiez le DiffGram ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MyDiffGram.xml dans le même dossier qu'à l'étape précédente.  
  
4.  Copiez le DiffGramSchema fourni plus haut dans cette rubrique et collez-le dans un fichier texte. Enregistrez le fichier sous le nom DiffGramSchema.xml.  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le DiffGram.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="c-updating-an-existing-record-by-using-a-diffgram"></a>C. Mise à jour d'un enregistrement existant à l'aide d'un DiffGram  
 Dans cet exemple, le DiffGram met à jour les informations sur les clients (CompanyName et ContactName) pour le client ALFKI.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer1"   
                msdata:rowOrder="0" diffgr:hasChanges="modified"   
                CustomerID="ALFKI">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Antonio Moreno</ContactName>  
      </Customer>  
    </DataInstance>  
  
    <diffgr:before>  
     <Customer diffgr:id="Customer1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
    </diffgr:before>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Le  **\<avant >** bloc inclut un  **\<client >** élément (**diffgr : ID = « Customer1 »**). Le  **\<DataInstance >** bloc inclut correspondant  **\<client >** élément avec la même **id**. Le  **\<client >** élément dans le  **\<NewDataSet >** spécifie également **diffgr : HasChanges = « modified »**. Cela indique une opération de mise à jour et l’enregistrement de client dans le **Cust** table est mise à jour en conséquence. Notez que si le **diffgr : HasChanges** attribut n’est pas spécifié, la logique de traitement DiffGram ignore cet élément et aucune mise à jour n’est effectuées.  
  
#### <a name="to-test-the-diffgram"></a>Pour tester le DiffGram  
  
1.  Création de ces tables dans le **tempdb** base de données.  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Ajoutez les exemples de données suivants :  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiez le DiffGram ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MyDiffGram.xml dans le même dossier qu'à l'étape précédente.  
  
4.  Copiez le DiffGramSchema fourni plus haut dans cette rubrique et collez-le dans un fichier texte. Enregistrez le fichier sous le nom DiffGramSchema.xml.  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le DiffGram.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="d-inserting-updating-and-deleting-records-by-using-a-diffgram"></a>D. Insertion, mise à jour et suppression d'enregistrements à l'aide d'un DiffGram  
 Dans cet exemple, un DiffGram relativement complexe est utilisé pour effectuer des opérations d'insertion, de mise à jour et de suppression.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                diffgr:hasChanges="modified"   
                CustomerID="ANATR">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Elizabeth Lincoln</ContactName>  
          <Order diffgr:id="Order2" msdata:rowOrder="1"   
               msdata:hiddenCustomerID="ANATR"   
               CustomerID="ANATR" OrderID="2"/>  
      </Customer>  
  
      <Customer diffgr:id="Customer3" msdata:rowOrder="2"   
                CustomerID="ANTON">  
         <CompanyName>Chop-suey Chinese</CompanyName>  
         <ContactName>Yang Wang</ContactName>  
         <Order diffgr:id="Order3" msdata:rowOrder="2"   
               msdata:hiddenCustomerID="ANTON"   
               CustomerID="ANTON" OrderID="3"/>  
      </Customer>  
      <Customer diffgr:id="Customer4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                CustomerID="AROUT">  
         <CompanyName>Around the Horn</CompanyName>  
         <ContactName>Thomas Hardy</ContactName>  
         <Order diffgr:id="Order4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                msdata:hiddenCustomerID="AROUT"   
               CustomerID="AROUT" OrderID="4"/>  
      </Customer>  
    </DataInstance>  
    <diffgr:before>  
      <Order diffgr:id="Order1" msdata:rowOrder="0"   
             msdata:hiddenCustomerID="ALFKI"   
             CustomerID="ALFKI" OrderID="1"/>  
      <Customer diffgr:id="Customer1" msdata:rowOrder="0"   
                CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                CustomerID="ANATR">  
        <CompanyName>Ana Trujillo Emparedados y helados</CompanyName>  
        <ContactName>Ana Trujillo</ContactName>  
      </Customer>  
    </diffgr:before>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 La logique du DiffGram traite ce DiffGram comme suit :  
  
-   Conformément à la logique de traitement DiffGram, tous les éléments de niveau supérieur dans le  **\<avant >** bloquer mappent aux tables correspondantes, comme décrit dans le schéma de mappage.  
  
-   Le  **\<avant >** bloc a un  **\<ordre >** élément (**dffgr:id = « Order1 »**) et un  **\<client >** élément (**diffgr : ID = « Customer1 »**) pour lequel il n’existe aucun élément correspondant dans le  **\<DataInstance >** bloc (avec le même ID). Cela indique une opération de suppression et les enregistrements sont supprimés des tables Cust et Ord.  
  
-   Le  **\<avant >** bloc contient un  **\<client >** élément (**diffgr : ID = « Customer2 »**) pour lesquels existe un correspondant  **\<client >** élément dans le  **\<DataInstance >** bloc (avec le même ID). L’élément dans le  **\<DataInstance >** bloc spécifie **diffgr : HasChanges = « modified »**. Il s’agit d’une opération de mise à jour dans lequel client ANATR, les informations CompanyName et ContactName sont mis à jour dans la table Cust à l’aide des valeurs spécifiées dans le  **\<DataInstance >** bloc.  
  
-   Le  **\<DataInstance >** bloc contient un  **\<client >** élément (**diffgr : ID = « Customer3 »**) et un  **\<ordre >** élément (**diffgr : ID = « Order3 »**). Aucune de ces éléments spécifient la **diffgr : HasChanges** attribut. Par conséquent, la logique de traitement du DiffGram ignore ces éléments.  
  
-   Le  **\<DataInstance >** bloc contient un  **\<client >** élément (**diffgr : ID = « Client4 »**) et un  **\<ordre >** élément (**diffgr : ID = « Order4 »**) pour lesquels il n’existe aucun élément correspondant dans le \<avant > bloc. Ces éléments dans le  **\<DataInstance >** bloc spécifier **diffgr : HasChanges = « inserted »**. Par conséquent, un nouvel enregistrement est ajouté dans les tables Cust et Ord.  
  
#### <a name="to-test-the-diffgram"></a>Pour tester le DiffGram  
  
1.  Créez les tables suivantes dans le **tempdb** base de données.  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Ajoutez les exemples de données suivants :  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Copiez le DiffGram ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MyDiffGram.xml dans le même dossier qu'à l'étape précédente.  
  
4.  Copiez le DiffGramSchema fourni plus haut dans cette rubrique et collez-le dans un fichier texte. Enregistrez le fichier sous le nom DiffGramSchema.xml.  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le DiffGram.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="e-applying-updates-by-using-a-diffgram-with-the-diffgrparentid-annotation"></a>E. Application de mises à jour à l'aide d'un DiffGram avec l'annotation diffgr:parentID  
 Cet exemple illustre comment la **parentID** annotation est spécifiée dans le  **\<avant >** bloc du DiffGram est utilisée pour appliquer les mises à jour.  
  
```  
<NewDataSet />  
<diffgr:before>  
   <Order diffgr:id="Order1" msdata:rowOrder="0" OrderID="2" />  
   <Order diffgr:id="Order3" msdata:rowOrder="2" OrderID="4" />  
  
   <OrderDetail diffgr:id="OrderDetail1"   
                diffgr:parentId="Order1"   
                msdata:rowOrder="0"   
                ProductID="13"   
                OrderID="2" />  
   <OrderDetail diffgr:id="OrderDetail3"   
                diffgr:parentId="Order3"  
                ProductID="77"  
                OrderID="4"/>  
</diffgr:before>  
</diffgr:diffgram>  
```  
  
 Ce DiffGram spécifie une opération de suppression, car il est uniquement un  **\<avant >** bloc. Dans le DiffGram, le **parentID** annotation est utilisée pour spécifier une relation parent-enfant entre les commandes et les détails de la commande. Lorsque SQLXML supprime des enregistrements, il les supprime de la table enfant qui est identifiée par cette relation, puis supprime les enregistrements de la table parente correspondante.  
  
  
