---
title: Collections de schémas XML (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XSD schemas [SQL Server]
- xml_schema_namespace function
- XML schema collections [SQL Server], about XML schema collections
- metadata [SQL Server], XML schema collections
- queries [XML in SQL Server], XML schema collections
- schema collections [SQL Server]
- schemas [SQL Server], XML
- XML [SQL Server], schema collections
- XML schema collections [SQL Server]
- schema collections [SQL Server], about XML schema collections
ms.assetid: 659d41aa-ccec-4554-804a-722a96ef25c2
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a0a3b92ae3ac44f071dc3bc36527a9b3f28396ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-schema-collections-sql-server"></a>Collections de schémas XML (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Comme l’explique la rubrique [xml &#40;Transact-SQL&#41;](../../t-sql/xml/xml-transact-sql.md), SQL Server assure un stockage natif des données XML par le biais du type de données **xml**. Vous pouvez éventuellement associer des schémas XSD à une variable ou à une colonne de type **xml** à l'aide d'une collection de schémas XML. La collection de schémas XML stocke les schémas XML importés et peut ensuite servir à :  
  
-   valider des instances XML ;  
  
-   typer les données XML lors de leur stockage dans la base de données.  
  
 Remarquez que la collection de schémas XML est une entité de métadonnées de même qu'une table de la base de données. Vous pouvez les créer, les modifier et les supprimer. Les schémas spécifiés dans une instruction [CREATE XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) sont automatiquement importés dans l’objet de collection de schémas XML nouvellement créé. Vous pouvez importer d’autres schémas ou composants de schéma dans un objet de collection existant dans la base de données à l’aide de l’instruction [ALTER XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md).  
  
 Comme l’explique la rubrique [XML typé et non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md), le code XML stocké dans une colonne ou une variable à laquelle un schéma est associé est dit XML **typé**, car le schéma fournit toutes les informations nécessaires sur le type des données de l’instance. SQL Server utilise ces informations de type pour optimiser le stockage des données.  
  
 Le moteur de traitement des requêtes utilise aussi le schéma pour vérifier le type et optimiser les requêtes et la modification des données.  
  
 De plus, SQL Server utilise la collection de schémas XML associée, en cas de code **xml**typé, pour valider l'instance XML. Si l'instance XML est conforme au schéma, la base de données autorise le stockage de l'instance dans le système avec ses informations de type. Sinon, elle rejette l'instance.  
  
 Vous pouvez utiliser la fonction intrinsèque XML_SCHEMA_NAMESPACE pour récupérer la collection de schémas stockée dans la base de données. Pour plus d’informations, consultez [Afficher une collection de schémas XML stockée](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Vous pouvez aussi utiliser la collection de schémas XML pour typer les variables, les paramètres et les colonnes XML.  
  
##  <a name="ddl"></a> DDL pour la gestion des collections de schémas  
 Vous pouvez créer des collections de schémas XML dans la base de données et les associer à des variables et des colonnes de type **xml** . Pour gérer des collections de schémas dans la base de données, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit les instructions DDL suivantes :  
  
-   [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) Importe les composants de schéma dans une base de données.  
  
-   [ALTER XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md) Modifie les composants de schéma dans une collection de schémas XML existante.  
  
-   [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md) Supprime une collection de schémas XML complète et tous ses composants.  
  
 Pour utiliser une collection de schémas XML et les schémas qu'elle contient, vous devez d'abord créer la collection et les schémas à l'aide de l'instruction CREATE XML SCHEMA COLLECTION. Une fois la collection de schémas créée, vous pouvez ensuite créer des variables et des colonnes de type **xml** et y associer la collection de schémas. Notez qu'après la création d'une collection de schémas, différents composants de schémas sont stockés dans les métadonnées. Vous pouvez également utiliser l'instruction ALTER XML SCHEMA COLLECTION pour ajouter des composants aux schémas existants ou pour ajouter de nouveaux schémas à une collection existante.  
  
 Pour supprimer la collection de schémas, utilisez l'instruction DROP XML SCHEMA COLLECTION. Cela supprime tous les schémas contenus dans la collection et supprime l'objet collection. Notez qu’avant de pouvoir supprimer une collection de schémas, les conditions décrites dans [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md) doivent être remplies.  
  
##  <a name="components"></a> Description des composants de schémas  
 Lorsque vous utilisez l'instruction CREATE XML SCHEMA COLLECTION, différents composants de schémas sont importés dans la base de données. Les composants de schémas incluent des éléments de schémas, des attributs et des définitions de types. Lorsque vous utilisez l'instruction DROP XML SCHEMA COLLECTION, vous supprimez l'intégralité de la collection.  
  
 CREATE XML SCHEMA COLLECTION enregistre les composants de schémas dans différentes tables système.  
  
 Imaginons, par exemple, le schéma suivant :  
  
```  
<?xml version="1.0"?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            targetNamespace="uri:Cust_Orders2"  
            xmlns="uri:Cust_Orders2" >  
  <xsd:attribute name="SomeAttribute" type="xsd:int" />  
  <xsd:complexType name="SomeType" />  
  <xsd:complexType name="OrderType" >  
    <xsd:sequence>  
      <xsd:element name="OrderDate" type="xsd:date" />  
      <xsd:element name="RequiredDate" type="xsd:date" />  
      <xsd:element name="ShippedDate" type="xsd:date" />  
    </xsd:sequence>  
    <xsd:attribute name="OrderID" type="xsd:ID" />  
    <xsd:attribute name="CustomerID"  />  
    <xsd:attribute name="EmployeeID"  />  
  </xsd:complexType>  
  <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order" type="OrderType"  
                     maxOccurs="unbounded" />  
       </xsd:sequence>  
      <xsd:attribute name="CustomerID" type="xsd:string" />  
      <xsd:attribute name="OrderIDList" type="xsd:IDREFS" />  
  </xsd:complexType>  
  <xsd:element name="Customer" type="CustomerType" />  
</xsd:schema>  
```  
  
 Le schéma précédent montre les différents types de composants qui peuvent être stockés dans la base de données. Il s'agit notamment de `SomeAttribute`, `SomeType`, `OrderType`, `CustomerType`, `Customer`, `Order`, `CustomerID`, `OrderID`, `OrderDate`, `RequiredDate`et `ShippedDate`.  
  
### <a name="component-categories"></a>Catégories de composant  
 Les composants de schéma stockés dans la base de données appartiennent aux catégories suivantes :  
  
-   ELEMENT  
  
-   ATTRIBUTE  
  
-   TYPE (pour les types simples ou complexes)  
  
-   ATTRIBUTEGROUP  
  
-   MODELGROUP  
  
 Exemple :  
  
-   **SomeAttribute** est un composant ATTRIBUTE.  
  
-   **SomeType**, **OrderType**et **CustomerType** sont des composants TYPE.  
  
-   **Customer** est un composant ELEMENT.  
  
 Lorsque vous importez un schéma dans la base de données, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne stocke pas le schéma lui-même. Au lieu de cela, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke les divers composants individuels. Autrement dit, la balise \<Schema> n’est pas stockée ; seuls les composants qui y sont définis sont conservés. Tous les éléments de schémas ne sont pas préservés. Si la balise \<Schema> contient des attributs qui spécifient le comportement par défaut de ses composants, ces attributs sont déplacés vers les composants de schéma dans la balise pendant le processus d’importation, comme illustré dans le tableau suivant.  
  
|Nom de l'attribut|Comportement|  
|--------------------|--------------|  
|**attributeFormDefault**|L'attribut **form** est appliqué à toutes les déclarations d'attributs du schéma où il n'est pas déjà présent et la valeur est définie à la valeur de l'attribut **attributeFormDefault** .|  
|**elementFormDefault**|L'attribut **form** est appliqué à toutes les déclarations d'éléments du schéma où il n'est pas déjà présent et la valeur est définie à la valeur de l'attribut **elementFormDefault** .|  
|**blockDefault**|L'attribut **block** est appliqué à toutes les déclarations d'éléments et définitions de types où il n'est pas déjà présent et la valeur est définie à la valeur de l'attribut **blockDefault** .|  
|**finalDefault**|L'attribut **final** est appliqué à toutes les déclarations d'éléments et définitions de types où il n'est pas déjà présent et la valeur est définie à la valeur de l'attribut **finalDefault** .|  
|**targetNamespace**|Les informations relatives aux composants qui appartiennent à l'espace de noms cible sont stockées dans les métadonnées.|  
  
##  <a name="perms"></a> Autorisations sur une collection de schémas XML  
 Vous devez disposer des autorisations nécessaires pour effectuer les opérations suivantes :  
  
-   créer/charger la collection de schémas XML ;  
  
-   modifier la collection de schémas XML ;  
  
-   supprimer la collection de schémas XML ;  
  
-   utiliser la collection de schémas XML pour typer des colonnes, des variables et des paramètres de type **xml** ou l'utiliser dans des contraintes de tables ou de colonnes.  
  
 Le modèle de sécurité SQL Server accorde l'autorisation CONTROL sur chaque objet. Le bénéficiaire de cette autorisation obtient toutes les autres autorisations sur l'objet. Le propriétaire de l'objet a également toutes les autorisations sur l'objet.  
  
 Le propriétaire et le bénéficiaire de l'autorisation CONTROL sur un objet peuvent accorder n'importe quelle autorisation sur l'objet. Un utilisateur qui n'est pas propriétaire et qui n'a pas l'autorisation CONTROL peut tout de même accorder l'autorisation sur un objet lorsque WITH GRANT OPTION est spécifié. Par exemple, supposons que l'utilisateur A dispose de l'autorisation REFERENCES sur la collection de schémas S (par le biais de WITH GRANT OPTION), mais d'aucune autre autorisation sur S. L'utilisateur A peut attribuer à l'utilisateur B l'autorisation REFERENCES sur la collection de schémas S.  
  
 Le modèle de sécurité permet également aux autorisations de créer et d'utiliser des collections de schémas XML ou de transférer la propriété d'un utilisateur à un autre. Les rubriques suivantes décrivent les autorisations de collections de schémas XML.  
  
-   [Accorder des autorisations sur une collection de schémas XML](../../relational-databases/xml/grant-permissions-on-an-xml-schema-collection.md)  
  
     Cette rubrique explique comment accorder des autorisations permettant de créer une collection de schémas XML et comment accorder des autorisations sur un objet de collection de schémas XML.  
  
-   [Révoquer des autorisations sur une collection de schémas XML](../../relational-databases/xml/revoke-permissions-on-an-xml-schema-collection.md)  
  
     Cette rubrique explique comment utiliser la révocation d'autorisation pour empêcher la création d'une collection de schémas XML et comment révoquer des autorisations sur un objet de collection de schémas XML.  
  
-   [Refuser des autorisations sur une collection de schémas XML](../../relational-databases/xml/deny-permissions-on-an-xml-schema-collection.md)  
  
     Cette rubrique explique comment refuser des autorisations permettant de créer une collection de schémas XML et comment refuser l'autorisation sur un objet de collection de schémas XML.  
  
##  <a name="info"></a> Obtention d'informations sur les schémas XML et les collections de schémas  
 Les collections de schémas XML sont répertoriées dans l'affichage catalogue sys.xml_schema_collections. La collection de schémas XML « sys » est définie par le système. Elle contient les espaces de noms prédéfinis qu'il est possible d'utiliser dans toutes les collections de schémas XML définies par l'utilisateur sans avoir à les charger explicitement. Cette liste contient les espaces de noms pour xml, xs, xsi, fn et xdt. Il existe deux autres affichages catalogue : sys.xml_schema_namespaces, qui répertorie tous les espaces de noms de chaque collection de schémas XML, et sys.xml_components, qui répertorie tous les composants de schéma XML de chaque schéma XML.  
  
 La fonction intégrée **XML_SCHEMA_NAMESPACE**, *schemaName, XmlSchemacollectionName, namespace-uri*, produit une instance de type de données **xml** . Cette instance contient des fragments de schéma XML pour les schémas qui sont contenus dans une collection de schémas XML, à l'exception des schémas XML prédéfinis.  
  
 Pour répertorier le contenu d'une collection de schémas XML, vous pouvez au choix :  
  
-   écrire des requêtes Transact-SQL sur les affichages catalogue appropriés pour les collections de schémas XML ;  
  
-   utiliser la fonction intégrée **XML_SCHEMA_NAMESPACE()**. Vous pouvez appliquer les méthodes du type de données **xml** sur le résultat de cette fonction. En revanche, vous ne pouvez pas modifier les schémas XML sous-jacents.  
  
 Ces méthodes sont illustrées dans les exemples ci-après.  
  
### <a name="example-enumerate-the-xml-namespaces-in-an-xml-schema-collection"></a>Exemple : énumération des espaces de noms XML dans une collection de schémas XML  
 Utilisez la requête suivante pour la collection de schémas XML « myCollection » :  
  
```  
SELECT XSN.name  
FROM    sys.xml_schema_collections XSC JOIN sys.xml_schema_namespaces XSN  
    ON (XSC.xml_collection_id = XSN.xml_collection_id)  
WHERE    XSC.name = 'myCollection'     
```  
  
### <a name="example-enumerate-the-contents-of-an-xml-schema-collection"></a>Exemple : énumération du contenu d'une collection de schémas XML  
 L'instruction suivante énumère le contenu de la collection de schémas XML « myCollection » dans le schéma relationnel, dbo.  
  
```  
SELECT XML_SCHEMA_NAMESPACE (N'dbo', N'myCollection')  
```  
  
 Les schémas XML individuels de la collection peuvent être obtenus sous forme d’instances de type **xml** en spécifiant l’espace de noms cible comme troisième argument de **XML_SCHEMA_NAMESPACE()**. Cela est illustré par l'exemple suivant.  
  
### <a name="example-output-a-specified-schema-from-an-xml-schema-collection"></a>Exemple : extraction d'un schéma spécifique d'une collection de schémas XML  
 L’instruction suivante extrait le schéma XML dont l’espace de noms cible est « http://www.microsoft.com/books » de la collection de schémas XML « myCollection » dans le schéma relationnel, dbo.  
  
```  
SELECT XML_SCHEMA_NAMESPACE (N'dbo', N'myCollection',   
N'http://www.microsoft.com/books')  
```  
  
### <a name="querying-xml-schemas"></a>Interrogation des schémas XML  
 Vous pouvez interroger les schémas XML que vous avez chargés dans les collections de schémas XML en procédant ainsi :  
  
-   Écrivez des requêtes Transact-SQL sur les affichages catalogue appropriés pour les espaces de noms de schémas XML.  
  
-   Créez une table qui contient une colonne de type **xml** pour stocker vos schémas XML et aussi les charger dans le système de type XML. Vous pouvez interroger la colonne XML à l'aide des méthodes de type de données **xml** . Vous pouvez aussi placer un index XML sur cette colonne. Toutefois, dans ce cas, l'application doit assurer la cohérence entre les schémas XML stockés dans la colonne XML et le système de type XML. Par exemple, si vous supprimez l'espace de noms du schéma XML du système de type XML, vous devez aussi le supprimer de la table pour garantir la cohérence.  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher une collection de schémas XML stockée](../../relational-databases/xml/view-a-stored-xml-schema-collection.md)   
 [Prétraiter un schéma pour fusionner des schémas inclus](../../relational-databases/xml/preprocess-a-schema-to-merge-included-schemas.md)   
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
