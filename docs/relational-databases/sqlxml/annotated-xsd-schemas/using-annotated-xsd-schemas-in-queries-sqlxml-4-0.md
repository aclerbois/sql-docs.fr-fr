---
title: À l’aide de XSD schémas annotés dans les requêtes (SQLXML 4.0) | Documents Microsoft
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
- queries [SQLXML]
- inline schemas [SQLXML]
- XPath queries [SQLXML], annotated XSD schemas in queries
- queries [SQLXML], annotated XSD schemas
- retrieving data
- mapping schema [SQLXML], queries
- multiple inline schemas
- annotated XSD schemas, queries
- XSD schemas [SQLXML], queries
- templates [SQLXML], annotated XSD schemas in queries
ms.assetid: 927a30a2-eae8-420d-851d-551c5f884f3c
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: cc4d6f7c2d77f2c375765bd745bee8e35e70f622
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-annotated-xsd-schemas-in-queries-sqlxml-40"></a>Utilisation de schémas XSD annotés dans les requêtes (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez spécifier des requêtes sur un schéma annoté afin de récupérer des données de la base de données ; pour ce faire, spécifiez des requêtes XPath dans un modèle par rapport au schéma XSD.  
  
 Le  **\<XPath-requête >** élément vous permet de spécifier une requête XPath sur la vue XML définie par le schéma annoté. Le schéma annoté sur lequel la requête XPath doit être exécutée est identifié à l’aide de la **schéma de mappage** attribut de la  **\<XPath-requête >** élément.  
  
 Les modèles sont des documents XML valides qui contiennent une ou plusieurs requêtes. Les requêtes XPath et FOR XML retournent un fragment de document. Les modèles servent de conteneurs pour les fragments de documents ; par conséquent, les modèles offrent un moyen de spécifier un élément unique, de niveau supérieur.  
  
 Les exemples de cette rubrique utilisent des modèles pour spécifier une requête XPath portant sur un schéma annoté afin de récupérer des données dans la base de données.  
  
 Considérons par exemple ce schéma annoté :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact" >  
     <xsd:complexType>  
       <xsd:attribute name="ContactID" type="xsd:string" />   
       <xsd:attribute name="FirstName" type="xsd:string" />   
       <xsd:attribute name="LastName"  type="xsd:string" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Pour les besoins de l'exemple, ce schéma XSD est stocké dans le fichier nommé Schema2.xml. Vous pouvez alors exécuter une requête XPath sur le schéma annoté spécifié dans le fichier modèle suivant (Schema2T.xml) :  
  
```  
<sql:xpath-query   
     xmlns:sql="urn:schemas-microsoft-com:xmlsql"  
     >  
          Person.Contact[@ContactID="1"]  
</sql:xpath-query>  
```  
  
 Vous pouvez ensuite créer et utiliser le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter la requête dans le cadre d'un fichier modèle. Pour plus d’informations, consultez [de schémas XDR annotés &#40;déconseillé dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="using-inline-mapping-schemas"></a>Utilisation de schémas de mappage insérés  
 Un schéma annoté peut être inclus directement dans un modèle ; une requête XPath peut ensuite être spécifiée dans le modèle par rapport au schéma inséré. Le modèle peut également être un code de mise à jour (updategram).  
  
 Un modèle peut inclure plusieurs schémas insérés. Pour utiliser un schéma en ligne qui est inclus dans un modèle, spécifiez la **id** de l’attribut avec une valeur unique du  **\<xsd : Schema >** élément, puis utiliser **#idvalue** pour référencer le schéma inline. Le **id** attribut est un comportement identique à la **prefix** ({urn : schemas-microsoft-com-sql} id) utilisée dans les schémas XDR.  
  
 Par exemple, le modèle suivant spécifie deux schémas annotés insérés :  
  
```  
<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<xsd:schema xmlns:xsd='http://www.w3.org/2001/XMLSchema'  
        xmlns:ms='urn:schemas-microsoft-com:mapping-schema'  
        id='InLineSchema1' sql:is-mapping-schema='1'>  
  <xsd:element name='Employees' ms:relation='HumanResources.Employee'>  
    <xsd:complexType>  
      <xsd:attribute name='LoginID'   
                     type='xsd:string'/>  
      <xsd:attribute name='Title'   
                     type='xsd:string'/>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
  
<xsd:schema xmlns:xsd='http://www.w3.org/2001/XMLSchema'  
        xmlns:ms='urn:schemas-microsoft-com:mapping-schema'  
        id='InLineSchema2' sql:is-mapping-schema='1'>  
  <xsd:element name='Contacts' ms:relation='Person.Contact'>  
    <xsd:complexType>  
  
      <xsd:attribute name='ContactID'   
                     type='xsd:string' />  
      <xsd:attribute name='FirstName'   
                     type='xsd:string' />  
      <xsd:attribute name='LastName'   
                     type='xsd:string' />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
  
<sql:xpath-query xmlns:sql='urn:schemas-microsoft-com:xml-sql'   
        mapping-schema='#InLineSchema1'>  
    /Employees[@LoginID='adventure-works\guy1']  
</sql:xpath-query>  
  
<sql:xpath-query xmlns:sql='urn:schemas-microsoft-com:xml-sql'   
        mapping-schema='#InLineSchema2'>  
    /Contacts[@ContactID='1']  
</sql:xpath-query>  
</ROOT>  
```  
  
 Le modèle spécifie également deux requêtes XPath. Chacun de la  **\<-requête xpath >** éléments identifie de façon unique le schéma de mappage en spécifiant le **schéma de mappage** attribut.  
  
 Lorsque vous spécifiez un schéma inline dans le modèle, le **sql : is-mapping-schema** annotation doit également être spécifiée sur le  **\<xsd : Schema >** élément. Le **sql : is-mapping-schema** prend une valeur booléenne (0 = Faux, 1 = true). Un schéma inline avec **sql : is-mapping-schema = « 1 »** est traité comme un schéma annoté inséré et n’est pas retourné dans le document XML.  
  
 Le **sql : is-mapping-schema** annotation appartient à l’espace de noms du modèle **urn : schemas-microsoft-com : sql**.  
  
 Pour tester cet exemple, enregistrez le modèle (InlineSchemaTemplate.xml) dans un répertoire local, puis créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) afin d'exécuter le modèle. Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 En plus de spécifier le **schéma de mappage** de l’attribut le  **\<XPath-requête >** élément dans un modèle (lorsqu’il existe une requête XPath) ou sur  **\<updg:sync >** élément dans une mise à jour, vous pouvez procédez comme suit :  
  
-   Spécifiez le **schéma de mappage** de l’attribut le  **\<racine >** élément (déclaration globale) dans le modèle. Ce schéma de mappage devient ensuite le schéma par défaut qui sera utilisé par tous les nœuds de XPath et de mise à jour qui n’explicite **schéma de mappage** annotation.  
  
-   Spécifiez le **schéma de mappage** à l’aide d’ADO **commande** objet.  
  
 Le **schéma de mappage** attribut qui est spécifié sur le  **\<-requête xpath >** ou  **\<updg:sync >** élément a la priorité la plus élevée ; ADO **commande** objet a la priorité la plus faible.  
  
 Notez que si vous spécifiez une requête XPath dans un modèle et que vous ne spécifiez pas un schéma de mappage sur lequel la requête XPath est exécutée, la requête XPath est traitée comme un **dbobject** requête de type. Considérons par exemple le modèle suivant :  
  
```  
<sql:xpath-query   
     xmlns:sql="urn:schemas-microsoft-com:xmlsql">  
          Production.ProductPhoto[@ProductPhotoID='100']/@LargePhoto  
</sql:xpath-query>  
```  
  
 Le modèle spécifie une requête XPath mais ne spécifie pas de schéma de mappage. Par conséquent, cette requête est traitée comme un **dbobject** une requête de type dans laquelle Production.ProductPhoto est le nom de table et @ProductPhotoID= '100' représente un prédicat qui recherche une photo du produit avec la valeur d’ID de 100. @LargePhoto est la colonne à partir de laquelle récupérer la valeur.  
  
  
