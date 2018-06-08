---
title: Introduction aux schémas XSD annotés (SQLXML 4.0) | Documents Microsoft
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
- namespaces [SQLXML], annotated XSD schemas
- mapping schema [SQLXML], about mapping schema
- views [SQLXML]
- valid XSD schemas [SQLXML]
- annotations [SQLXML]
- XSD schemas [SQLXML], creating XML views
- annotated XSD schemas, creating XML views
- minimum XSD schema
- annotated XSD schemas, examples
- XML views [SQLXML]
ms.assetid: 15282db1-65c4-43be-bdb7-e9ef49cb33a2
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8b771edea6a71767e05e9d24c3ddd542bae2930c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-annotated-xsd-schemas-sqlxml-40"></a>Introduction aux schémas XSD annotés (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez créer des vues XML de données relationnelles à l'aide du langage XSD (XML Schema Definition). Ces vues peuvent ensuite être interrogées à l'aide de requêtes XML Path (XPath). Cette opération équivaut à créer des vues à l'aide d'instructions CREATE VIEW et à spécifier ensuite des requêtes SQL contre les vues.  
  
 Un schéma XML décrit la structure d'un document XML, ainsi que diverses contraintes agissant sur les données du document. Lorsque vous spécifiez des requêtes XPath à exécuter dans le schéma, la structure du document XML retournée est déterminée par le schéma dans lequel la requête XPath est exécutée.  
  
 Dans un schéma XSD, les  **\<xsd : Schema >** élément englobe le schéma entier ; toutes les déclarations d’élément doivent être contenues dans le  **\<xsd : Schema >** élément. Vous pouvez décrire les attributs qui définissent l’espace de noms dans lequel le schéma réside et les espaces de noms qui sont utilisés dans le schéma en tant que propriétés de la  **\<xsd : Schema >** élément.  
  
 Un schéma XSD valide doit contenir le  **\<xsd : Schema >** élément défini comme suit :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<!-- additional schema definitions here -->  
</xsd:schema>  
```  
  
 Le  **\<xsd : Schema >** élément est dérivé de la spécification d’espace de noms de schéma XML à http://www.w3.org/2001/XMLSchema.  
  
## <a name="annotations-to-the-xsd-schema"></a>Annotations dans le schéma XSD  
 Vous pouvez utiliser un schéma XSD avec des annotations qui décrivent le mappage à une base de données, interroger la base de données, puis retourner les résultats sous la forme d'un document XML. Les annotations sont fournies pour mapper un schéma XSD à des tables et des colonnes de base de données. Vous pouvez définir et exécuter des requêtes XPath dans la vue XML créée par le schéma XSD pour interroger la base de données et obtenir des résultats sous forme de données XML.  
  
> [!NOTE]  
>  Dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0, le langage de schéma XSD prend en charge les annotations introduites avec le langage de schéma XDR (XML-Data Reduced) dans [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]. Le langage XDR annoté est déconseillé dans SQLXML 4.0.  
  
 Dans le contexte de la base de données relationnelle, il est utile de mapper le schéma XSD arbitraire à un magasin relationnel. Une manière d'y parvenir consiste à annoter le schéma XSD. Un schéma XSD avec annotations est appelé un *schéma de mappage*, qui fournit des informations sur la façon dont les données XML sont à mapper au magasin relationnel. Un schéma de mappage constitue en réalité une vue XML des données relationnelles. Ces mappages peuvent servir à extraire des données relationnelles sous la forme d'un document XML.  
  
## <a name="namespace-for-annotations"></a>Espace de noms des annotations  
 Dans un schéma XSD, les annotations sont spécifiées à l’aide de l’espace de noms **urn : schemas-microsoft-mapping-schema**. Comme indiqué dans l’exemple suivant, le moyen le plus simple pour spécifier l’espace de noms est de le spécifier dans le  **\<xsd : Schema >** balise.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
...  
</xsd:schema>  
```  
  
 Le préfixe d’espace de noms qui est utilisé est arbitraire. Dans cette documentation, le **sql** préfixe est utilisé pour désigner l’espace de noms des annotations et distinguer les annotations dans cet espace de noms de celles d’autres espaces de noms.  
  
## <a name="example-of-an-annotated-xsd-schema"></a>Exemple de schéma XSD annoté  
 Dans l’exemple suivant, le schéma XSD comprend un  **\<Person.Contact >** élément. Le  **\<employé >** élément a un **ContactID** attribut et  **\<FirstName >** et  **\<LastName >** des éléments enfants :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
  <xsd:element name="Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"    
                     type="xsd:string" />   
        <xsd:element name="LName"  
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ConID" type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Des annotations sont ajoutées à ce schéma XSD pour mapper ses éléments et attributs aux tables et colonnes de base de données :  
  
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
        <xsd:attribute name="ConID"   
                       sql:field="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Dans le schéma de mappage, le  **\<Contact >** élément est mappé à la table Person.Contact dans la base de données exemple AdventureWorks à l’aide de la **SQL : relation** annotation. Les attributs ConID, FName et LName sont mappés aux colonnes ContactID, FirstName et LastName dans la table Person.Contact à l’aide de la **SQL : Field** annotations.  
  
 Ce schéma XSD annoté crée la vue XML des données relationnelles. Vous pouvez interroger cette vue XML au moyen du langage XPath. En guise de résultat, une requête XPath retourne un document XML au lieu de l'ensemble de lignes retourné par les requêtes SQL.  
  
> [!NOTE]  
>  Dans le schéma de mappage, le respect de la casse pour les valeurs relationnelles spécifiées (telles que le nom de table et le nom de colonne) varie selon que SQL Server utilise des paramètres de classement sensibles à la casse. Pour plus d’informations, consultez [Collation and Unicode Support](../../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="other-resources"></a>Autres ressources  
 Vous trouverez des informations supplémentaires sur le langage XSD (XML Schema Definition), le langage XPath ( XML Path) et le langage XSLT (Extensible Stylesheet Language Transformations) sur les sites Web aux adresses suivantes :  
  
-   XML Schema Part 0 : Primer, W3C (de recommandationhttp://www.w3.org/TR/xmlschema-0/)  
  
-   XML Schema Part 1 : Structures, le W3C (de recommandationhttp://www.w3.org/TR/xmlschema-1/)  
  
-   XML Schema Part 2 : Datatypes W3C (de recommandationhttp://www.w3.org/TR/xmlschema-2/)  
  
-   XML Path Language (XPath) ()http://www.w3.org/TR/xpath)  
  
-   XSL Transformations (XSLT) (http://www.w3.org/TR/xslt)  
  
## <a name="see-also"></a>Voir aussi  
 [Annoté considérations de sécurité de schéma &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)   
 [Les schémas XDR annotés &#40;déconseillés dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)  
  
  
