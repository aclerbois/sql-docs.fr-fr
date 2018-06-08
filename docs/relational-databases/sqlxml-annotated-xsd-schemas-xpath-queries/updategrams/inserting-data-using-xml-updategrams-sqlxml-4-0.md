---
title: Insertion de données à l’aide de codes XML (SQLXML 4.0) | Documents Microsoft
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
- xsi:nil attribute
- unique values
- <after> block
- id attribute
- data insertions [SQLXML]
- nil attribute
- <before> block
- updg:guid attribute
- multiple record insertions
- returnid attribute
- updategrams [SQLXML], inserting data
- updg:at-identity attribute
- invalid characters [SQLXML]
- updg:returnid attribute
- updg:id attribute
- namespaces [SQLXML], updategrams
- IDENTITY-type column
- guid attribute
- record insertion [SQLXML]
- null values [SQLXML]
- at-identity attribute
- xml data type [SQL Server], SQLXML
ms.assetid: 4dc48762-bc12-43fb-b356-ea1b9c1e287e
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 563a79b68a39a886d70234f2e9f6eaae118d11ca
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="inserting-data-using-xml-updategrams-sqlxml-40"></a>Insertion de données à l'aide de codes de mise à jour (updategrams) XML (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Une mise à jour indique une opération d’insertion lorsqu’une instance d’enregistrement apparaît dans les  **\<après >** bloc mais ne pas dans le correspondant  **\<avant >** bloc. Dans ce cas, la mise à jour insère l’enregistrement dans le  **\<après >** bloc dans la base de données.  
  
 Voici le format du code de mise à jour pour une opération d'insertion :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   [<updg:before>  
   </updg:before>]  
    <updg:after [updg:returnid="x y ...] >  
       <ElementName [updg:id="value"]   
                   [updg:at-identity="x"]   
                   [updg:guid="y"]  
                   attribute="value"   
                   attribute="value"  
                   ...  
       />  
      [<ElementName .../>... ]  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
## <a name="before-block"></a>\<avant de > bloc  
 Le  **\<avant >** bloc peut être omis pour une opération d’insertion. Si le paramètre facultatif **schéma de mappage** attribut n’est pas spécifié, le  **\<ElementName >** qui est spécifié dans les cartes de mise à jour pour une table de base de données et les éléments enfants ou des attributs de mappage à des colonnes dans la table.  
  
## <a name="after-block"></a>\<une fois > bloc  
 Vous pouvez spécifier un ou plusieurs enregistrements dans la  **\<après >** bloc.  
  
 Si le  **\<après >** bloc ne fournit pas une valeur pour une colonne particulière, la mise à jour utilise la valeur par défaut qui est spécifiée dans le schéma annoté (si un schéma a été spécifié). Si le schéma ne spécifie pas de valeur par défaut pour la colonne, la mise à jour ne spécifie pas de valeur explicite pour cette colonne et attribue à la place, le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valeur par défaut (si spécifié) pour cette colonne. S'il n'y a aucune valeur par défaut [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et que la colonne accepte une valeur NULL, le code de mise à jour attribue la valeur NULL à la colonne. Si la colonne ne possède pas de valeur par défaut et qu'elle n'accepte pas de valeur NULL, la commande échoue et le code de mise à jour retourne une erreur. Le paramètre facultatif **updg:returnid** attribut est utilisé pour retourner la valeur d’identité générée par le système lorsqu’un enregistrement est ajouté dans une table avec une colonne de type IDENTITY.  
  
## <a name="updgid-attribute"></a>Attribut updg:id  
 Si la mise à jour insère uniquement des enregistrements, la mise à jour ne nécessite pas le **updg : ID** attribut. Pour plus d’informations sur **updg : ID**, consultez [mise à jour des données à l’aide de programmes &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/updating-data-using-xml-updategrams-sqlxml-4-0.md).  
  
## <a name="updgat-identity-attribute"></a>Attribut updg:at-identity  
 Lorsqu’une mise à jour insère un enregistrement dans une table qui comporte une colonne de type IDENTITY, la mise à jour peut capturer la valeur attribuée au système à l’aide de l’option **updg : à l’identité** attribut. Le code de mise à jour peut utiliser ensuite cette valeur dans les opérations suivantes. Lors de l’exécution de mise à jour, vous pouvez retourner la valeur d’identité qui est générée en spécifiant le **updg:returnid** attribut.  
  
## <a name="updgguid-attribute"></a>Attribut updg:guid  
 Le **updg : GUID** attribut est un attribut facultatif qui génère un identificateur global unique. Cette valeur reste dans la portée de l’ensemble du  **\<synchronisation >** bloc dans lequel elle est spécifiée. Vous pouvez utiliser cette valeur n’importe où dans le  **\<synchronisation >** bloc. L’attribut appelle la **::NewGuid()** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fonction pour générer l’identificateur unique.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l’aide de la procédure ci-après, vous devez respecter la configuration requise spécifiée dans [configuration requise pour exécuter les exemples de SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
 Avant d'utiliser les exemples de code de mise à jour, notez les points suivants :  
  
-   La plupart des exemples utilisent le mappage par défaut (en d'autres termes, aucun schéma de mappage n'est spécifié dans le code de mise à jour (updategram)). Pour plus d’exemples de codes qui utilisent des schémas de mappage, consultez [spécification d’un schéma de mappage annoté dans une mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
-   La plupart des exemples sont basés sur l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)]. Toutes les mises à jour sont appliquées aux tables de cette base de données.  
  
### <a name="a-inserting-a-record-by-using-an-updategram"></a>A. Insertion d'un enregistrement à l'aide d'un code de mise à jour  
 Ce code de mise à jour centré sur les attributs insère un enregistrement dans la table HumanResources.Employee dans la base de données [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].  
  
 Dans cet exemple, le code de mise à jour ne spécifie pas de schéma de mappage. Par conséquent, le code de mise à jour utilise le mappage par défaut, dans lequel le nom d'élément est mappé à un nom de table et les attributs ou éléments enfants sont mappés aux colonnes dans cette table.  
  
 Le schéma [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] pour la table HumanResources.Department impose une restriction NOT NULL sur toutes les colonnes sauf OrganizationNode et OrganizationLevel. Par conséquent, le code de mise à jour doit inclure des valeurs spécifiées pour toutes les colonnes. DepartmentID est une colonne de type IDENTITY. Par conséquent, aucune valeur n'est spécifiée pour cette colonne.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name="New Product Research"   
            GroupName="Research and Development"   
            ModifiedDate="2010-08-31"/>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MyUpdategram.xml.  
  
2.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Dans un mappage centré sur l'élément, le code de mise à jour ressemble à ceci :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department>  
            <Name> New Product Research </Name>  
            <GroupName> Research and Development </GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Dans un code de mise à jour en mode mixte (centré sur l'élément et centré sur l'attribut), un élément peut avoir des attributs et des sous-éléments, comme indiqué dans ce code de mise à jour :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name=" New Product Research "   
            <GroupName>Research and Development</GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="b-inserting-multiple-records-by-using-an-updategram"></a>B. Insertion de plusieurs enregistrements à l'aide d'un code de mise à jour  
 Ce code de mise à jour ajoute deux nouveaux enregistrements de décalage à la table HumanResources.Shift. Mise à jour ne spécifie pas le paramètre facultatif  **\<avant >** bloc.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom Updategram-AddShifts.xml.  
  
2.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Une autre version de cet exemple est une mise à jour qui utilise deux  **\<après >** blocs, au lieu d’un bloc pour insérer les deux employés. Cette opération est valide et peut être encodée comme suit :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
    <updg:before>  
    </updg:before>  
    <updg:after >  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="c-working-with-valid-sql-server-characters-that-are-not-valid-in-xml"></a>C. Utilisation de caractères SQL Server valides qui ne sont pas valides en XML  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les noms de table peuvent inclure un espace, comme la table Order Details dans la base de données Northwind. Toutefois, cela n’est pas valide dans les caractères XML valides [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificateurs, mais des identificateurs XML non valides peuvent être encodés à l’aide de ' __xHHHH\_\_' comme valeur d’encodage, où HHHH représente le code de UCS-2 hexadécimal à quatre chiffres du caractère dans l’ordre du premier bit plus significatif.  
  
> [!NOTE]  
>  Cet exemple utilise l'exemple de base de données Northwind. Vous pouvez installer la base de données Northwind à l’aide d’un script SQL disponible en téléchargement à partir de ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkId=30196).  
  
 En outre, le nom d’élément doit figurer entre crochets ([]). Étant donné que les caractères [et] ne sont pas valide dans XML, vous devez les encoder comme _x005B\_ et _x005D\_, respectivement. (Si vous utilisez un schéma de mappage, vous pouvez fournir des noms d'élément qui ne contiennent pas de caractères non valides, tels que des espaces blancs. Le schéma de mappage effectue le mappage nécessaire ; par conséquent, il est inutile d'encoder ces caractères).  
  
 Ce code de mise à jour ajoute un enregistrement à la table Order Details dans la base de données Northwind :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
      <_x005B_Order_x0020_Details_x005D_ OrderID="1"  
            ProductID="11"  
            UnitPrice="$1.0"  
            Quantity="1"  
            Discount="0.0" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 La colonne UnitPrice dans la table Order Details est de le **money** type. Pour appliquer la conversion de type approprié (à partir d’un **chaîne** type à un **money** type), le signe dollar ($) doit être ajouté en tant que partie de la valeur. Si la mise à jour ne spécifie pas un schéma de mappage, le premier caractère de la **chaîne** valeur est évaluée. Si le premier caractère est le signe $, la conversion appropriée est appliquée.  
  
 Si la mise à jour est spécifiée par rapport à un schéma de mappage dans lequel la colonne est marquée de façon appropriée en tant que **dt:type="fixed.14.4 »** ou **SQL : DataType = « money »**, le signe dollar ($) n’est pas nécessaire et la conversion est contrôlée par le mappage. Il s'agit de la méthode recommandée pour garantir que la conversion de type appropriée a lieu.  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom UpdategramSpacesInTableName.xml.  
  
2.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="d-using-the-at-identity-attribute-to-retrieve-the-value-that-has-been-inserted-in-the-identity-type-column"></a>D. Utilisation de l'attribut at-identity pour récupérer la valeur ayant été insérée dans la colonne de type IDENTITY  
 Le code de mise à jour suivant insère deux enregistrements : un dans la table Sales.SalesOrderHeader et un autre dans la table Sales.SalesOrderDetail.  
  
 En premier lieu, le code de mise à jour ajoute un enregistrement à la table Sales.SalesOrderHeader. Dans cette table, la colonne SalesOrderID est une colonne de type IDENTITY. Par conséquent, lorsque vous ajoutez cet enregistrement à la table, la mise à jour utilise le **à identité** attribut pour capturer la valeur SalesOrderID attribuée en tant que « x » (une valeur d’espace réservé). L’est utilisé spécifie ensuite cette **à identité** variable en tant que la valeur de l’attribut SalesOrderID dans le \<Sales.SalesOrderDetail > élément.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
   <Sales.SalesOrderHeader updg:at-identity="x"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00001111-2222-3333-4444-556677889900"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
      <Sales.SalesOrderDetail SalesOrderID="x"  
                LineNumber="1"  
                OrderQty="1"  
                ProductID="776"  
                SpecialOfferID="1"  
                UnitPrice="2429.9928"  
                UnitPriceDiscount="0.00"  
                rowguid="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"  
                ModifiedDate="2001-07-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Si vous voulez retourner la valeur d’identité générée par le **updg : à l’identité** attribut, vous pouvez utiliser la **updg:returnid** attribut. Le code de mise à jour modifié suivant retourne cette valeur d'identité. (Ce code de mise à jour ajoute deux enregistrements de commande et deux enregistrements de détail de commande pour rendre l'exemple un peu plus complexe.)  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync>  
  <updg:before>  
  </updg:before>  
  <updg:after updg:returnid="x y" >  
       <HumanResources.Shift updg:at-identity="x" Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift updg:at-identity="y" Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:after>  
 </updg:sync>  
</ROOT>  
```  
  
 Lorsque le code de mise à jour est exécuté, il retourne des résultats semblables aux suivants, qui comprennent notamment la valeur d'identité (valeur générée de la colonne ShiftID utilisée pour l'identité de table) ayant été générée :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">   
  <returnid>   
    <x>4</x>   
    <y>5</y>   
  </returnid>   
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom Updategram-returnId.xml.  
  
2.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="e-using-the-updgguid-attribute-to-generate-a-unique-value"></a>E. Utilisation de l'attribut updg:guid pour générer une valeur unique  
 Dans cet exemple, le code de mise à jour insère un enregistrement dans les tables Cust et CustOrder. En outre, la mise à jour génère une valeur unique pour l’attribut CustomerID à l’aide de la **updg : GUID** attribut.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after updg:returnid="x" >  
      <Cust updg:guid="x" >  
         <CustID>x</CustID>  
         <LastName>Fuller</LastName>  
      </Cust>  
      <CustOrder>  
         <CustID>x</CustID>  
         <OrderID>1</OrderID>  
      </CustOrder>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Mise à jour Spécifie le **returnid** attribut. En conséquence, le GUID généré est retourné :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <returnid>  
    <x>7111BD1A-7F0B-4CEE-B411-260DADFEFA2A</x>   
  </returnid>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom Updategram-GenerateGuid.xml.  
  
2.  Créez les tables suivantes :  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust (CustID uniqueidentifier, LastName varchar(20))  
    CREATE TABLE CustOrder (CustID uniqueidentifier, OrderID int)  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="f-specifying-a-schema-in-an-updategram"></a>F. Spécification d'un schéma dans un code de mise à jour  
 Le code de mise à jour dans cet exemple insère un enregistrement dans la table suivante :  
  
```  
CustOrder(OrderID, EmployeeID, OrderType)  
```  
  
 Un schéma XSD est spécifié dans ce code de mise à jour (autrement dit, il n'y a aucun mappage par défaut des éléments et des attributs du code de mise à jour). Le schéma fournit le mappage nécessaire des éléments et des attributs aux tables et aux colonnes de base de données.  
  
 Le schéma suivant (CustOrderSchema.xml) décrit un  **\<CustOrder >** élément qui comprend le **OrderID** et **EmployeeID** attributs. Pour rendre le schéma plus intéressant, une valeur par défaut est affectée à la **EmployeeID** attribut. Un code de mise à jour utilise uniquement la valeur par défaut d'un attribut pour les opérations d'insertion, puis uniquement si le code de mise à jour ne spécifie pas cet attribut.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="CustOrder" >  
   <xsd:complexType>  
        <xsd:attribute name="OrderID"     type="xsd:integer" />   
        <xsd:attribute name="EmployeeID"  type="xsd:integer" />  
        <xsd:attribute name="OrderType  " type="xsd:integer" default="1"/>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Ce code de mise à jour insère un enregistrement dans la table CustOrder. Le code de mise à jour spécifie uniquement les valeurs des attributs OrderID et EmployeeID. Il ne spécifie pas la valeur de l'attribut OrderType. Par conséquent, le code de mise à jour utilise la valeur par défaut de l'attribut EmployeeID spécifié dans le schéma précédent.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
             xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync mapping-schema='CustOrderSchema.xml'>  
<updg:after>  
       <CustOrder OrderID="98000" EmployeeID="1" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 Pour plus d’exemples de codes qui spécifient un schéma de mappage, consultez [spécification d’un schéma de mappage annoté dans une mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Créez cette table dans le **tempdb** base de données :  
  
    ```  
    USE tempdb  
    CREATE TABLE CustOrder(  
                     OrderID int,   
                     EmployeeID int,   
                     OrderType int)  
    ```  
  
2.  Copiez le schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom CustOrderSchema.xml.  
  
3.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom CustOrderUpdategram.xml dans le même dossier qu'à l'étape précédente.  
  
4.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le code de mise à jour (updategram).  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le schéma XDR équivalent :  
  
```  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
 <ElementType name="CustOrder" >  
    <AttributeType name="OrderID" />  
    <AttributeType name="EmployeeID" />  
    <AttributeType name="OrderType" default="1" />  
    <attribute type="OrderID"  />  
    <attribute type="EmployeeID" />  
    <attribute type="OrderType" />  
  </ElementType>  
</Schema>  
```  
  
### <a name="g-using-the-xsinil-attribute-to-insert-null-values-in-a-column"></a>G. Utilisation de l'attribut xsi:nil pour insérer des valeurs Null dans une colonne  
 Si vous souhaitez insérer une valeur null dans la colonne correspondante dans la table, vous pouvez spécifier le **xsi : nil** attribut sur un élément dans une mise à jour. Dans le schéma XSD correspondant, le schéma XSD **nillable** attribut doit être spécifié.  
  
 Considérons par exemple ce schéma XSD :  
  
```  
<?xml version="1.0"?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="Student" sql:relation="Students">  
  <xsd:complexType>  
    <xsd:all>  
      <xsd:element name="fname" sql:field="first_name"   
                                type="xsd:string"   
                                 nillable="true"/>  
    </xsd:all>  
    <xsd:attribute name="SID"   
                        sql:field="StudentID"  
                        type="xsd:ID"/>      
    <xsd:attribute name="lname"       
                        sql:field="last_name"  
                        type="xsd:string"/>  
    <xsd:attribute name="minitial"    
                        sql:field="middle_initial"   
                        type="xsd:string"/>  
    <xsd:attribute name="years"       
                         sql:field="no_of_years"  
                         type="xsd:integer"/>  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Le schéma XSD spécifie **nillable = « true »** pour le  **\<fname >** élément. Le code de mise à jour suivant utilise ce schéma :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
      xmlns:updg="urn:schemas-microsoft-com:xml-updategram"  
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
<updg:sync mapping-schema='StudentSchema.xml'>  
  <updg:before/>  
  <updg:after>  
    <Student SID="S00004" lname="Elmaci" minitial="" years="2">  
      <fname xsi:nil="true">  
    </fname>  
    </Student>  
  </updg:after>  
</updg:sync>  
  
</ROOT>  
```  
  
 Mise à jour spécifie **xsi : nil** pour le  **\<fname >** élément dans le  **\<après >** bloc. Par conséquent, lorsque ce code de mise à jour est exécuté, une valeur NULL est insérée pour la colonne first_name dans la table.  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Créez la table suivante dans le **tempdb** base de données :  
  
    ```  
    USE tempdb  
    CREATE TABLE Students (  
       StudentID char(6)NOT NULL ,  
       first_name varchar(50),  
       last_name varchar(50),  
       middle_initial char(1),  
       no_of_years int NULL)  
    GO  
    ```  
  
2.  Copiez le schéma ci-dessus et collez-le dans un fichier texte. Enregistrez ce fichier sous le nom StudentSchema.xml.  
  
3.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom StudentUpdategram.xml dans le même dossier qu'à l'étape précédente pour enregistrer StudentSchema.xml.  
  
4.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le code de mise à jour (updategram).  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="h-specifying-namespaces-in-an-updategram"></a>H. Spécification d'espaces de noms dans un code de mise à jour  
 Dans un code de mise à jour, vous pouvez avoir des éléments qui appartiennent à un espace de noms déclaré dans le même élément dans le code de mise à jour. Dans ce cas, le schéma correspondant doit également déclarer le même espace de noms et l'élément doit appartenir à cet espace de noms cible.  
  
 Par exemple, dans la mise à jour suivant (UpdateGram-elementhavingnamespace.XML), le  **\<ordre >** élément appartient à un espace de noms déclaré dans l’élément.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema='XSD-ElementHavingNameSpace.xml'>  
    <updg:after>  
       <x:Order  xmlns:x="http://server/xyz/schemas/"  
             updg:at-identity="SalesOrderID"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00009999-8888-7777-6666-554433221100"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Dans ce cas, le schéma doit également déclarer l'espace de noms comme montré dans ce schéma :  
  
 Le schéma suivant (XSD-ElementHavingNamespace.xml) montre comment l'élément et les attributs correspondants doivent être déclarés.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
     xmlns:dt="urn:schemas-microsoft-com:datatypes"   
     xmlns:sql="urn:schemas-microsoft-com:mapping-schema"    
     xmlns:x="http://server/xyz/schemas/"   
     targetNamespace="http://server/xyz/schemas/" >  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" type="x:Order_type"/>  
  <xsd:complexType name="Order_type">  
    <xsd:attribute name="SalesOrderID"  type="xsd:ID"/>  
    <xsd:attribute name="RevisionNumber" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OrderDate" type="xsd:dateTime"/>  
    <xsd:attribute name="DueDate" type="xsd:dateTime"/>  
    <xsd:attribute name="ShipDate" type="xsd:dateTime"/>  
    <xsd:attribute name="Status" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OnlineOrderFlag" type="xsd:boolean"/>  
    <xsd:attribute name="SalesOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="PurchaseOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="AccountNumber" type="xsd:string"/>  
    <xsd:attribute name="CustomerID" type="xsd:int"/>  
    <xsd:attribute name="ContactID" type="xsd:int"/>  
    <xsd:attribute name="SalesPersonID" type="xsd:int"/>  
    <xsd:attribute name="TerritoryID" type="xsd:int"/>  
    <xsd:attribute name="BillToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipMethodID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardApprovalCode" type="xsd:string"/>  
    <xsd:attribute name="CurrencyRateID" type="xsd:int"/>  
    <xsd:attribute name="SubTotal" type="xsd:decimal"/>  
    <xsd:attribute name="TaxAmt" type="xsd:decimal"/>  
    <xsd:attribute name="Freight" type="xsd:decimal"/>  
    <xsd:attribute name="TotalDue" type="xsd:decimal"/>  
    <xsd:attribute name="Comment" type="xsd:string"/>  
    <xsd:attribute name="rowguid" type="xsd:string"/>  
    <xsd:attribute name="ModifiedDate" type="xsd:dateTime"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Copiez le schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom XSD-ElementHavingNamespace.xml.  
  
2.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom Updategram-ElementHavingNamespace.xml dans le même dossier que celui utilisé pour enregistrer XSD-ElementHavingnamespace.xml.  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le code de mise à jour (updategram).  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="i-inserting-data-into-an-xml-data-type-column"></a>I. Insertion de données dans une colonne de type de données XML  
 Le **xml** type de données a été introduit dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Vous pouvez utiliser les codes pour insérer et mettre à jour les données stockées dans **xml** colonnes avec les conditions suivantes :  
  
-   Le **xml** colonne ne peut pas être utilisée pour identifier une ligne existante. Par conséquent, il ne peut pas être inclus dans le **updg : avant** section d’une mise à jour.  
  
-   Espaces de noms dans la portée du fragment XML inséré dans le **xml** colonne est conservée et leurs déclarations d’espace de noms sont ajoutées à l’élément supérieur du fragment inséré.  
  
 Par exemple, dans la mise à jour suivant (SampleUpdateGram.xml), le  **\<Desc >** élément met à jour la colonne ProductDescription dans la Production > productModel table les [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] base de données exemple. Le résultat de cette mise à jour est que le contenu XML de la colonne ProductDescription est mises à jour avec le contenu XML de la  **\<Desc >** élément.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync mapping-schema="SampleSchema.xml" >  
       <updg:before>  
<ProductModel ProductModelID="19">  
   <Name>Mountain-100</Name>  
</ProductModel>  
    </updg:before>  
    <updg:after>  
 <ProductModel>  
    <Name>Mountain-100</Name>  
    <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
        <p1:ProductDescription xmlns:p1="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
              xmlns:wm="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
              xmlns:wf="http://www.adventure-works.com/schemas/OtherFeatures"   
              xmlns:html="http://www.w3.org/1999/xhtml"   
              xmlns="">  
  <p1:Summary>  
     <html:p>Insert Example</html:p>  
  </p1:Summary>  
  <p1:Manufacturer>  
    <p1:Name>AdventureWorks</p1:Name>  
    <p1:Copyright>2002</p1:Copyright>  
    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
  </p1:Manufacturer>  
  <p1:Features>These are the product highlights.   
    <wm:Warranty>  
       <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
       <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
    <wm:Maintenance>  
       <wm:NoOfYears>10 years</wm:NoOfYears>  
       <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
    </wm:Maintenance>  
    <wf:wheel>High performance wheels.</wf:wheel>  
    <wf:saddle>  
      <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle>  
    <wf:pedal>  
       <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal>  
    <wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter and wall-thickness required of a premium mountain frame. The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame>  
    <wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset>  
   </p1:Features>  
   <p1:Picture>  
      <p1:Angle>front</p1:Angle>  
      <p1:Size>small</p1:Size>  
      <p1:ProductPhotoID>118</p1:ProductPhotoID>  
   </p1:Picture>  
   <p1:Specifications> These are the product specifications.  
     <Material>Almuminum Alloy</Material>  
     <Color>Available in most colors</Color>  
     <ProductLine>Mountain bike</ProductLine>  
     <Style>Unisex</Style>  
     <RiderExperience>Advanced to Professional riders</RiderExperience>  
   </p1:Specifications>  
  </p1:ProductDescription>  
 </Desc>  
      </ProductModel>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Le code de mise à jour fait référence au schéma XSD annoté suivant (SampleSchema.xml).  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
     <xsd:complexType>  
       <xsd:sequence>  
          <xsd:element name="Name" type="xsd:string"></xsd:element>  
          <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
           <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="ProductDescription">  
                 <xsd:complexType>  
                   <xsd:sequence>  
                     <xsd:element name="Summary" type="xsd:anyType">  
                     </xsd:element>  
                   </xsd:sequence>  
                 </xsd:complexType>  
              </xsd:element>  
            </xsd:sequence>  
           </xsd:complexType>  
          </xsd:element>   
       </xsd:sequence>  
       <xsd:attribute name="ProductModelID" sql:field="ProductModelID"/>  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Copiez le schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom XSD-SampleSchema.xml.  
  
    > [!NOTE]  
    >  Codes prenant en charge le mappage par défaut, il n’existe aucun moyen pour identifier le début et la fin de la **xml** type de données. Cela signifie qu’un schéma de mappage est requis lors de l’insertion ou de mise à jour des tables avec **xml** colonnes de type de données. Lorsqu'un schéma n'est pas fourni, SQLXML retourne une erreur indiquant que l'une des colonnes est absente de la table.  
  
2.  Copiez le code de mise à jour ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom SampleUpdategram.xml dans le même dossier que celui utilisé pour enregistrer SampleSchema.xml.  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le code de mise à jour (updategram).  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations de sécurité de mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
