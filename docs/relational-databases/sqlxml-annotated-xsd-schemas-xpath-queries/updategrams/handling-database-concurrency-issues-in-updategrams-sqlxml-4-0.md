---
title: La gestion des problèmes d’accès concurrentiel de base de données dans les codes (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- <before> block
- low concurrency protection
- database concurrency [SQLXML]
- timestamp column [SQLXML]
- updategrams [SQLXML], database concurrency
- high concurrency protection [SQLXML]
- optimistic concurrency control
- concurrency [SQLXML]
- intermediate concurrency protection [SQLXML]
ms.assetid: d4b908d1-b25b-4ad9-8478-9cd882e8c44e
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 73ae79d0831820366f5ec6454573df26c7b36e01
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handling-database-concurrency-issues-in-updategrams-sqlxml-40"></a>Gestion des problèmes d'accès concurrentiel aux bases de données dans les codes de mise à jour (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Comme d'autres mécanismes de mise à jour de base de données, les codes de mise à jour doivent faire face à des mises à jour simultanées de données dans un environnement multi-utilisateur. Les codes de mise à jour utilisent le contrôle d'accès concurrentiel optimiste, qui utilise la comparaison des données de champ sélectionnées comme instantanés pour garantir que les données à mettre à jour n'ont pas été altérées par une autre application utilisateur depuis qu'elles ont été lues à partir de la base de données. Codes incluent ces valeurs d’instantané dans le  **\<avant >** bloc des codes. Avant la mise à jour de la base de données, mise à jour vérifie les valeurs qui sont spécifiés dans le  **\<avant >** bloc par rapport aux valeurs actuellement dans la base de données pour vous assurer que la mise à jour est valide.  
  
 Le contrôle d'accès concurrentiel optimiste offre trois niveaux de protection dans un code de mise à jour : bas (aucune), intermédiaire et haut. Vous pouvez décider de quel niveau de protection vous avez besoin en spécifiant le code de mise à jour en conséquence.  
  
## <a name="lowest-level-of-protection"></a>Niveau de protection le plus bas  
 Ce niveau correspond à une mise à jour aveugle, dans laquelle la mise à jour est traitée sans se référer aux autres mises à jour effectuées depuis la dernière lecture de la base de données. Dans ce cas, vous spécifiez uniquement les colonnes clés primaires dans le  **\<avant >** bloquer pour identifier l’enregistrement et que vous spécifiez les informations mises à jour dans le  **\<après >** bloc.  
  
 Par exemple, le nouveau numéro de téléphone du contact dans le code de mise à jour suivant est correct, indépendamment de ce que le numéro de téléphone était précédemment. Notez comment la  **\<avant >** bloc spécifie uniquement la colonne clé primaire (ContactID).  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
   <Person.Contact ContactID="1" />  
</updg:before>  
<updg:after>  
   <Person.Contact ContactID="1" Phone="111-111-1111" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="intermediate-level-of-protection"></a>Niveau de protection intermédiaire  
 Dans ce niveau de protection, le code de mise à jour compare la ou les valeurs actuelles des données qui sont mises à jour avec la ou les valeurs présentes dans la ou les colonnes de la base de données pour garantir que les valeurs n'ont pas été modifiées par quelque autre transaction depuis que l'enregistrement a été lu par votre transaction.  
  
 Vous pouvez obtenir ce niveau de protection en spécifiant les colonnes de clé primaires et l’ou les colonnes que vous mettez à jour dans le  **\<avant >** bloc.  
  
 Par exemple, ce code de mise à jour modifie la valeur dans la colonne Phone de la table Person.Contact pour le contact dont ContactID a la valeur 1. Le  **\<avant >** bloc spécifie le **téléphone** attribut pour vérifier que cette valeur d’attribut correspond à la valeur dans la colonne correspondante dans la base de données avant d’appliquer la valeur mise à jour.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
   <Person.Contact ContactID="1" Phone="398-555-0132" />  
</updg:before>  
<updg:after>  
   <Person.Contact ContactID="1" Phone="111-111-1111" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="high-level-of-protection"></a>Niveau de protection élevé  
 Un niveau de protection élevé garantit qu'un enregistrement reste le même depuis que votre application a lu pour la dernière fois cet enregistrement (autrement dit, depuis que votre application a lu l'enregistrement, celui-ci n'a été modifié par aucune autre transaction).  
  
 Il existe deux façons d'obtenir ce niveau de protection élevé contre les mises à jour simultanées :  
  
-   Spécifier des colonnes supplémentaires dans la table dans le  **\<avant >** bloc.  
  
     Si vous spécifiez des colonnes supplémentaires dans le  **\<avant >** bloc, la mise à jour compare les valeurs sont spécifiées pour ces colonnes avec les valeurs qui étaient dans la base de données avant d’appliquer la mise à jour. Si l'une quelconque des colonnes d'enregistrement a changé depuis que votre transaction a lu l'enregistrement, le code de mise à jour n'effectue pas la mise à jour.  
  
     Par exemple, la mise à jour suivant met à jour le nom de l’équipe, mais spécifie des colonnes supplémentaires (StartTime, EndTime) dans le  **\<avant >** bloc, en demandant ainsi un niveau supérieur de protection contre les mises à jour simultanées.  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync >  
    <updg:before>  
       <HumanResources.Shift ShiftID="1"   
                 Name="Day"   
                 StartTime="1900-01-01 07:00:00.000"   
                 EndTime="1900-01-01 15:00:00.000" />  
    </updg:before>  
    <updg:after>  
       <HumanResources.Shift Name="Morning" />  
    </updg:after>  
    </updg:sync>  
    </ROOT>  
    ```  
  
     Cet exemple spécifie le niveau de protection le plus élevé en spécifiant toutes les valeurs de colonne pour l’enregistrement dans le  **\<avant >** bloc.  
  
-   Spécifier la colonne timestamp (si disponible) dans le  **\<avant >** bloc.  
  
     Au lieu de spécifier toutes les colonnes d’enregistrement dans le  **\<avant**> bloc, vous pouvez spécifiez simplement la colonne timestamp (si la table possède un), ainsi que les colonnes clés primaires dans le  **\<avant >** bloc. La base de données met à jour la colonne timestamp en spécifiant une valeur unique après chaque mise à jour de l'enregistrement. Dans ce cas, le code de mise à jour compare la valeur de l'horodateur avec la valeur correspondante dans la base de données. La valeur d'horodateur stockée dans la base de données est une valeur binaire. Par conséquent, la colonne timestamp doit être spécifiée dans le schéma en tant que **dt:type="bin.hex »**, **dt:type="bin.base64 »**, ou **SQL : DataType = « timestamp »**. (Vous pouvez spécifier le **xml** type de données ou le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données.)  
  
#### <a name="to-test-the-updategram"></a>Pour tester le code de mise à jour  
  
1.  Créez cette table dans le **tempdb** base de données :  
  
    ```  
    USE tempdb  
    CREATE TABLE Customer (  
                 CustomerID  varchar(5),  
                 ContactName varchar(20),  
                 LastUpdated timestamp)  
    ```  
  
2.  Ajoutez cet exemple d'enregistrement :  
  
    ```  
    INSERT INTO Customer (CustomerID, ContactName) VALUES   
                         ('C1', 'Andrew Fuller')  
    ```  
  
3.  Copiez le schéma XSD suivant et collez-le dans le Bloc-notes. Enregistrez-le sous le nom ConcurrencySampleSchema.xml :  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="Customer" sql:relation="Customer" >  
       <xsd:complexType>  
            <xsd:attribute name="CustomerID"    
                           sql:field="CustomerID"   
                           type="xsd:string" />   
  
            <xsd:attribute name="ContactName"    
                           sql:field="ContactName"   
                           type="xsd:string" />  
  
            <xsd:attribute name="LastUpdated"   
                           sql:field="LastUpdated"   
                           type="xsd:hexBinary"   
                 sql:datatype="timestamp" />  
  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
4.  Copiez le code de mise à jour ci-dessous dans le Bloc-notes et enregistrez-le sous le nom ConcurrencySampleTemplate.xml dans le répertoire où vous avez enregistré le schéma créé à l'étape précédente. (Notez que la valeur d'horodateur ci-dessous pour LastUpdated différera dans votre exemple de table Customer et copiez donc la valeur réelle pour LastUpdated à partir de la table et collez-la dans le code de mise à jour.)  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync mapping-schema="SampleSchema.xml" >  
    <updg:before>  
       <Customer CustomerID="C1"   
                 LastUpdated = "0x00000000000007D1" />  
    </updg:before>  
    <updg:after>  
       <Customer ContactName="Robert King" />  
    </updg:after>  
    </updg:sync>  
    </ROOT>  
    ```  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le schéma XDR équivalent :  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<ElementType name="Customer" sql:relation="Customer" >  
    <AttributeType name="CustomerID" />  
    <AttributeType name="ContactName" />  
    <AttributeType name="LastUpdated"  dt:type="bin.hex"   
                                       sql:datatype="timestamp" />  
    <attribute type="CustomerID" />  
    <attribute type="ContactName" />  
    <attribute type="LastUpdated" />  
</ElementType>  
</Schema>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations de sécurité de mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
