---
title: Référencer la collection de schémas XML intégrée (sys) | Microsoft Docs
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
- sys XML schema collections [SQL Server]
- schema collections [SQL Server], predefined
- predefined XML schema collections [SQL Server]
- XML schema collections [SQL Server], predefined
- built-in XML schema collections [SQL Server]
ms.assetid: 1e118303-5df0-4ee4-bd8d-14ced7544144
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4e0c3e8619db6d5f6c325b6762ca3bc4976f223b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reference-the-built-in-xml-schema-collection-sys"></a>Référencer la collection de schémas XML intégrée (sys)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Chaque base de données que vous créez a une collection de schémas XML **sys** prédéfinie dans le schéma relationnel **sys** . Elle réserve ces schémas prédéfinis, qui sont accessibles à toute autre collection de schémas XML créés par l'utilisateur. Les préfixes utilisés dans ces schémas prédéfinis ont une signification dans XQuery. Seul **xml** est un préfixe réservé.  
  
```  
xml = http://www.w3.org/XML/1998/namespace  
xs = http://www.w3.org/2001/XMLSchema  
xsi = http://www.w3.org/2001/XMLSchema-instance  
fn = http://www.w3.org/2004/07/xpath-functions  
sqltypes = http://schemas.microsoft.com/sqlserver/2004/sqltypes  
xdt = http://www.w3.org/2004/07/xpath-datatypes  
(no prefix) = urn:schemas-microsoft-com:xml-sql  
(no prefix) = http://schemas.microsoft.com/sqlserver/2004/SOAP  
```  
  
 Notez que l’espace de noms **sqltypes** contient des composants qui peuvent être référencés à partir de toute collection de schémas XML créée par l’utilisateur. Vous pouvez télécharger le schéma **sqltypes** à partir de ce [site web de Microsoft](http://go.microsoft.com/fwlink/?linkid=31850). Les composants intégrés incluent les éléments suivants :  
  
-   Types XSD  
  
-   Attributs XML **lang**, **base**et **space**  
  
-   Composants de l’espace de noms **sqltypes**  
  
 La requête ci-dessous retourne les composants intégrés qui peuvent être référencés à partir d'une collection de schémas XML créés par l'utilisateur :  
  
```  
SELECT C.name, N.name, C.symbol_space_desc from sys.xml_schema_components C join sys.xml_schema_namespaces N  
on ((C.xml_namespace_id = N.xml_namespace_id) AND (C.xml_collection_id = N.xml_collection_id))  
join sys.xml_schema_collections SC  
on SC.xml_collection_id = C.xml_collection_id  
where ((C.xml_collection_id = 1) AND (C.name is not null) AND (C.scoping_xml_component_id is null)   
AND (SC.schema_id = 4))  
GO  
```  
  
 L'exemple suivant montre comment ces composants sont référencés dans un schéma utilisateur. `CREATE XML SCHEMA COLLECTION` crée une collection de schémas XML qui fait référence au type `varchar` défini dans l’espace de noms `sqltypes` . L'exemple référence également l'attribut `lang` défini dans l'espace de noms `xml` .  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema   
   xmlns="http://www.w3.org/2001/XMLSchema"   
   targetNamespace="myNS"  
   xmlns:ns="myNS"  
   xmlns:s="http://schemas.microsoft.com/sqlserver/2004/sqltypes" >   
   <import namespace="http://www.w3.org/XML/1998/namespace"/>  
   <import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
   <element name="root">  
      <complexType>  
          <sequence>  
             <element name="a" type="string"/>  
             <element name="b" type="string"/>  
             <!-- varchar type is defined in the sys.sys collection and   
                  can be referenced in any user-defined schema -->  
             <element name="c" type="s:varchar"/>  
          </sequence>  
          <attribute name="att" type="int"/>  
          <!-- xml:lang attribute is defined in the sys.sys collection   
               and can be referenced in any user-defined schema -->  
          <attribute ref="xml:lang"/>  
      </complexType>  
    </element>  
 </schema>'  
GO  
 -- Cleanup  
DROP xml schema collection SC   
GO  
```  
  
 Notez les points suivants :  
  
-   Vous ne pouvez pas modifier les schémas XML avec ces espaces de noms dans toutes les collections de schémas XML créés par l'utilisateur. Par exemple, la collection de schémas XML ci-dessous échoue, car elle ajoute un composant dans l'espace de noms protégé `sqltypes` :  
  
    ```  
    CREATE XML SCHEMA COLLECTION SC AS '  
    <schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace    
        ="http://schemas.microsoft.com/sqlserver/2004/sqltypes" >   
          <element name="root" type="string"/>  
    </schema>'  
    GO  
    ```  
  
-   Vous ne pouvez pas utiliser la collection de schémas XML `sys` pour taper des colonnes, des variables ou des paramètres `xml` . Par exemple, le code suivant retourne une erreur :  
  
    ```  
    DECLARE @x xml (sys.sys)  
    ```  
  
-   La sérialisation de ces schémas intégrés n'est pas prise en charge. Par exemple, le code suivant retourne une erreur :  
  
    ```  
    SELECT XML_SCHEMA_NAMESPACE(N'sys',N'sys')  
    GO  
    ```  
  
 Le code suivant est un autre exemple dans lequel vous créez une collection de schémas XML qui utilise le type `varchar` défini dans l'espace de noms `sqltypes` :  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"   
        targetNamespace="myNS" xmlns:ns="myNS"  
        xmlns:s="http://schemas.microsoft.com/sqlserver/2004/sqltypes">  
   <import     
     namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
      <simpleType name="myType">  
            <restriction base="s:varchar">  
                  <maxLength value="20"/>  
            </restriction>  
      </simpleType>  
      <element name="root" type="ns:myType"/>  
</schema>'  
go  
```  
  
 Comme illustré ci-dessous, vous pouvez créer une variable `XML` typée, lui attribuer une instance XML et vérifier que la valeur du type d'élément <`root`> est un type `varchar`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "http://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
data(/ns:root[1]) instance of sqltypes:varchar?')  
GO  
```  
  
 L’expression `instance of sqltypes:varchar?` retourne TRUE, car la valeur de l’élément &lt;`root`&gt; est d’un type dérivé de **varchar`@var` d’après le schéma associé à la variable** .  
  
## <a name="see-also"></a> Voir aussi  
 [Collections de schémas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
