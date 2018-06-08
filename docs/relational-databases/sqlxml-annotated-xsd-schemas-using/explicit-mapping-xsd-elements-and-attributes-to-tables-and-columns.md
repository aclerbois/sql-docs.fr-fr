---
title: Mappage explicite XSD éléments et attributs aux Tables et colonnes | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- XPath queries [SQLXML], annotated XSD schemas in queries
- sql:field
- row mapping [SQLXML]
- attribute mapping [SQLXML], explicit mapping
- field annotation
- XSD schemas [SQLXML], mapping attributes and elements
- names [SQLXML]
- relation annotation
- table/view mapping [SQLXML], explicit mapping
- sql:relation
- mapping schema [SQLXML], explicit mapping
- annotated XSD schemas, mapping attributes and elements
- column mapping [SQLXML]
- element mapping [SQLXML], explicit mapping
- table mapping [SQLXML], explicit mapping
- element/attribute mapping [SQLXML]
ms.assetid: 7a5ebeb6-7322-4141-a307-ebcf95976146
caps.latest.revision: 28
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1836db12438017023637185d6e8ba24a6e533a77
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="explicit-mapping-xsd-elements-and-attributes-to-tables-and-columns"></a>Mappage explicite XSD éléments et attributs aux Tables et colonnes
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Lors de l'utilisation d'un schéma XSD pour fournir une vue XML de la base de données relationnelle, les éléments et les attributs du schéma doivent être mappés avec les tables et les colonnes de la base de données. Les lignes de la table/vue de la base de données seront mappées avec les éléments du document XML. Les valeurs des colonnes de la base de données sont mappées avec les attributs ou les éléments.  
  
 Lorsque les requêtes XPath sont spécifiées par rapport au schéma XSD annoté, les données des éléments et des attributs du schéma sont extraites des tables et des colonnes avec lesquelles elles sont mappées. Pour obtenir une valeur unique de la base de données, le mappage spécifié dans le schéma XSD doit posséder les spécifications de relation et de champ. Si le nom d’un élément/attribut n’est pas le même nom que le nom de table ou de vue ou une colonne à laquelle il est mappé, le **SQL : relation** et **SQL : Field** annotations sont utilisées pour spécifier le mappage entre un élément ou attribut dans un document XML et la table (vue) ou une colonne dans une base de données.  
  
## <a name="sql-relation"></a>sql-relation  
 Le **SQL : relation** annotation est ajoutée pour mapper un nœud XML dans le schéma XSD à une table de base de données. Le nom d’une table (vue) est spécifié comme valeur de la **SQL : relation** annotation.  
  
 Lorsque **SQL : relation** est spécifiée sur un élément, l’étendue de cette annotation s’applique à tous les attributs et éléments enfants qui sont décrites dans la définition du type complexe de cet élément, fournissant ainsi un raccourci pour l’écriture d’annotations.  
  
 Le **SQL : relation** annotation est également utile lorsque les identificateurs qui sont valides dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas valides dans XML. Par exemple, « Détails des commandes » est un nom de table valide dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais non en XML. Dans ce cas, le **SQL : relation** annotation peut être utilisée pour spécifier le mappage, par exemple :  
  
```  
<xsd:element name="OD" sql:relation="[Order Details]">  
```  
  
## <a name="sql-field"></a>sql-field  
 Le **sql-champ** annotation mappe un élément ou un attribut à une colonne de base de données. Le **SQL : Field** annotation est ajoutée pour mapper un nœud XML dans le schéma à une colonne de base de données. Vous ne pouvez pas spécifier **SQL : Field** sur un élément de contenu vide.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l'aide des exemples suivants, vous devez répondre à certaines conditions requises. Pour plus d’informations, consultez [configuration requise pour exécuter les exemples de SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-the-sqlrelation-and-sqlfield-annotations"></a>A. Spécification des annotations sql:relation et sql:field  
 Dans cet exemple, le schéma XSD comprend un  **\<Contact >** élément de type complexe avec  **\<FName >** et  **\<LName >** des éléments enfants et les **ContactID** attribut.  
  
 Le **SQL : relation** annotation mappe le  **\<Contact >** élément à la table Person.Contact dans la base de données AdventureWorks. Le **SQL : Field** annotation mappe le  **\<FName >** élément à la colonne FirstName et  **\<LName >** élément à la colonne LastName.  
  
 Aucune annotation n’est spécifiée pour le **ContactID** attribut. Il s'ensuit un mappage par défaut de l'attribut avec la colonne du même nom.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Contact" sql:relation="Person.Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"  
                     sql:field="FirstName"   
                     type="xsd:string" />   
        <xsd:element name="LName"    
                     sql:field="LastName"    
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MySchema-annotated.xml.  
  
2.  Copiez le modèle suivant ci-dessous et collez-le dans un fichier texte. Enregistrez le fichier sous le nom MySchema-annotatedT.xml dans le même répertoire que celui où vous avez enregistré MySchema-annotated.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="MySchema-annotated.xml">  
        /Contact  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (MySchema-annotated.xml) varie en fonction du répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchema-annotated.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le jeu de résultats partiel :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
 <Contact ContactID="1">   
    <FName>Gustavo</FName>   
    <LName>Achong</LName>   
 </Contact>   
  .....  
</ROOT>  
```  
  
  
