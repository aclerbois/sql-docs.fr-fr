---
title: Révoquer des autorisations sur une collection de schémas XML | Microsoft Docs
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
- revoking permissions [SQL Server]
ms.assetid: 4e542b70-2d56-4a65-8a39-96a1ed477ca6
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2c667e8f97f7db859e75771e25cc9587caddf8f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="revoke-permissions-on-an-xml-schema-collection"></a>Révoquer des autorisations sur une collection de schémas XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Il est possible de retirer l'autorisation de créer une collection de schémas XML de l'une des façons suivantes :  
  
-   Retrait de l'autorisation ALTER pour le schéma relationnel. Ainsi, le principal ne peut pas créer de collection de schémas XML dans le schéma relationnel. Toutefois, le principal reste en mesure de le faire dans d'autres schémas relationnels de la même base de données.  
  
-   Retrait de l'autorisation ALTER ANY SCHEMA sur la base de données pour le principal. Dans ce cas, le principal ne peut pas créer de collection de schémas XML dans la base de données.  
  
-   Retrait des autorisations CREATE XML SCHEMA COLLECTION ou ALTER XML SCHEMA COLLECTION sur la base de données pour le principal. Le principal est ainsi empêché d'importer une collection de schémas XML dans la base de données. Le retrait des autorisations ALTER ou CONTROL sur la base de données a le même effet.  
  
## <a name="revoking-permissions-on-an-existing-xml-schema-collection-object"></a>Retrait des autorisations sur un objet Collection de schémas XML existant  
 Voici les autorisations qu'il est possible de retirer sur une collection de schémas XML, ainsi que les résultats qui en découlent :  
  
-   En cas de retrait de l'autorisation ALTER, un principal n'a plus le droit de modifier le contenu de la collection de schémas XML.  
  
-   En cas de retrait de l'autorisation TAKE OWNERSHIP, un principal n'a plus le droit de transférer l'appartenance de la collection de schémas XML.  
  
-   En cas de retrait de l'autorisation REFERENCES, un principal n'a plus le droit d'utiliser une collection de schémas XML pour modifier le type ou la contrainte des colonnes de type xml, dans les tables et les vues, ou des paramètres. De plus, elle n'a plus l'autorisation de faire référence à cette collection de schémas à partir d'autres collections de schémas XML.  
  
-   En cas de retrait de l'autorisation VIEW DEFINITION, un principal n'a plus le droit de consulter le contenu d'une collection de schémas XML.  
  
-   En cas de retrait de l'autorisation EXECUTE, un principal n'a plus le droit d'insérer ni de mettre à jour des valeurs dans des colonnes, des variables et des paramètres typés ou contraints par la collection XML. De plus, elle n'a plus la possibilité de lancer une requête sur ces colonnes, variables ou paramètres de type **xml** .  
  
## <a name="examples"></a>Exemples  
 Les scénarios proposés dans les exemples suivants illustrent le fonctionnement des autorisations sur les schémas XML. Chaque exemple crée la base de données de test, les schémas relationnels et les connexions nécessaires. Ces connexions reçoivent les autorisations nécessaires sur la collection de schémas XML. Chaque exemple procède au nettoyage qui s'impose à la fin de la procédure.  
  
### <a name="a-revoking-permissions-to-create-an-xml-schema-collection"></a>A. Retrait des autorisations de créer une collection de schémas XML  
 Cet exemple crée une connexion et un exemple de base de données. Il ajoute également un schéma relationnel à la base de données. Au départ, la connexion bénéficie d'une autorisation ALTER sur les deux schémas relationnels et des autorisations voulues pour créer des collections de schémas XML. Ensuite, l'exemple retire l'autorisation ALTER sur l'un des schémas relationnels de la base de données de façon à empêcher la connexion de créer une collection de schémas XML.  
  
```  
setuser  
go  
create login TestLogin1 with password='SQLSvrPwd1'  
go  
create database SampleDBForSchemaPermissions  
go  
use SampleDBForSchemaPermissions  
go  
-- Create another relational schema in the db (in addition to dbo schema)  
CREATE SCHEMA myOtherDBSchema  
go  
CREATE USER TestLogin1  
go  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed  
-- CREATE XML SCHEMA is a database level permission  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
go  
-- Now TestLogin1 can import an XML schema collection in both relational schemas.  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- TestLogin1 can create XML schema collection in myOtherDBSchema relational schema  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- Let us drop XML schema collections from both relational schemas  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
go  
DROP XML SCHEMA COLLECTION dbo.myTestSchemaCollection  
go  
-- now REVOKE permission from TestLogin1 to alter myOtherDBSchema  
setuser  
go  
REVOKE ALTER ON SCHEMA::myOtherDBSchema FROM TestLogin1  
go  
-- now TestLogin1 cannot create xml schema collection in myOtherDBSchema  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- TestLogin1 can still create XML schema collections in dbo  
-- It cannot create XML schema collections anywhere in the database  
-- if we REVOKE CREATE XML SCHEMA COLLECTION permission  
SETUSER  
go  
REVOKE CREATE XML SCHEMA COLLECTION FROM TestLogin1  
go  
  
setuser 'TestLogin1'  
go  
-- the following now should fail  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- Final cleanup  
SETUSER  
go  
USE master  
go  
DROP DATABASE SampleDBForSchemaPermissions  
go  
DROP LOGIN TestLogin1  
Go  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
