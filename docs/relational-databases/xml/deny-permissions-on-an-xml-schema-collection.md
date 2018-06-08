---
title: Refuser des autorisations sur une collection de schémas XML | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- denying permissions [SQL Server], XML server collections
ms.assetid: e2b300b0-e734-4c43-a4da-c78e6e5d4fba
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7605e6e9e5555f83cd770ae3ce70177b6cf57bc2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deny-permissions-on-an-xml-schema-collection"></a>Refuser des autorisations sur une collection de schémas XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les autorisations de créer une collection de schémas XML ou d'en utiliser qui existe déjà peuvent être refusées.  
  
## <a name="denying-permission-to-create-an-xml-schema-collection"></a>Refus de l'autorisation de créer une collection de schémas XML  
 Vous pouvez refuser l'autorisation de créer une collection de schémas XML en suivant l'une des méthodes suivantes :  
  
-   en refusant l'autorisation ALTER sur le schéma relationnel ;  
  
-   en refusant l'autorisation CONTROL sur le schéma relationnel pour refuser toute autorisation sur le schéma relationnel et les objets qu'il contient ;  
  
-   en refusant l'autorisation ALTER ANY SCHEMA sur la base de données. Dans ce cas, le principal ne peut créer de collection de schémas XML nulle part dans la base de données. Il est important de savoir que refuser les autorisations ALTER ou CONTROL sur la base de données revient à refuser toutes les autorisations sur tous les objets de la base de données.  
  
## <a name="denying-permissions-on-an-xml-schema-collection-object"></a>Refus d'autorisations sur un objet de collection de schémas XML  
 Nous vous présentons maintenant les autorisations qui peuvent être refusées sur une collection de schémas XML existante, ainsi que les conséquences d'un tel refus :  
  
-   Le refus de l'autorisation ALTER empêche le principal de modifier le contenu de la collection de schémas XML.  
  
-   Le refus de l'autorisation CONTROL empêche le principal d'effectuer une quelconque opération sur la collection de schémas XML.  
  
-   Le refus de l'autorisation REFERENCES empêche le principal de typer les colonnes et les paramètres ou d'en contraindre le type xml à l'aide de la collection de schémas XML. Le principal perd également la faculté de pouvoir faire référence à la collection mentionnée ci-dessus dans d'autres collections de schémas XML.  
  
-   Le refus de l'autorisation VIEW DEFINITION empêche le principal d'afficher le contenu de la collection de schémas XML.  
  
-   Le refus de l'autorisation EXECUTE empêche le principal d'insérer ou de mettre à jour les valeurs des colonnes, des variables et des paramètres typés ou contraints par la collection de schémas XML. Le principal perd également la faculté de d'interroger les valeurs de ces mêmes colonnes et variables de type xml.  
  
## <a name="examples"></a>Exemples  
 Les scénarios proposés dans les exemples suivants montrent le fonctionnement des autorisations sur les schémas XML. Chaque exemple crée la base de données de test, les schémas relationnels et les connexions nécessaires. Ces connexions reçoivent les autorisations nécessaires sur la collection de schémas XML. Chaque exemple procède au nettoyage qui s'impose à la fin de la procédure.  
  
### <a name="a-preventing-a-user-from-creating-an-xml-schema-collection"></a>A. Scénario pour empêcher un utilisateur de créer une collection de schémas XML  
 Une des méthodes pour empêcher un utilisateur de créer une collection de schémas XML consiste à lui refuser l'autorisation ALTER sur un schéma relationnel. Cela est illustré par l'exemple suivant.  
  
 Cet exemple crée un utilisateur `TestLogin1`et une base de données. Il crée également un schéma relationnel, en plus du schéma `dbo` , dans la base de données. L'autorisation `CREATE XML SCHEMA` de départ permet à l'utilisateur de créer une collection de schémas n'importe où dans la base de données. L'exemple refuse ensuite l'autorisation `ALTER` à l'utilisateur sur l'un des schémas relationnels. L'utilisateur ne peut donc plus créer de collection de schémas XML dans ce schéma relationnel.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
GO  
-- Now deny permission from TestLogin1 to alter myOtherDBSchema.  
setuser  
GO  
DENY ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
GO  
-- Now TestLogin1 cannot create xml schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Final cleanup  
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
### <a name="b-denying-permissions-on-an-xml-schema-collection"></a>B. Refus d'autorisations sur une collection de schémas XML  
 L'exemple suivant montre comment une autorisation spécifique sur une collection de schémas XML existante peut être refusée à une connexion. Dans cet exemple, une connexion de test se voit refuser l'autorisation REFERENCES sur une collection de schémas XML existante.  
  
 Cet exemple crée un utilisateur `TestLogin1`et une base de données. Il crée également un schéma relationnel, en plus du schéma `dbo` , dans la base de données. L'autorisation `CREATE XML SCHEMA` de départ permet à l'utilisateur de créer une collection de schémas n'importe où dans la base de données.  
  
 L'autorisation `REFERENCES` sur la collection de schémas XML permet à `TestLogin1` d'utiliser le schéma lors de la création d'une colonne `xml` typée dans une table. Si l'autorisation `REFERENCES` sur la collection de schémas XML est refusée, elle empêche `TestLogin1` de faire appel à la collection de schémas XML.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, the following  
-- permission is required.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="http://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Grant permission to TestLogin1 to create a table and reference the XML schema collection.  
SETUSER  
GO  
GRANT CREATE TABLE TO TestLogin1  
GO  
-- The user also needs REFERENCES permission to use the XML schema collection  
-- to create a typed XML column (REFERENCES permission on the schema   
-- collection is not needed).  
GRANT REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection   
TO TestLogin1  
GO  
  
--TestLogin1 can use the schema.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
GO  
-- Drop the table.  
DROP TABLE T  
GO  
-- Now deny REFERENCES permission to TestLogin1 on the schema created previously.  
SETUSER  
GO  
DENY REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection TO TestLogin1  
  
GO  
-- Now TestLogin1 cannot create xml schema collection  
SETUSER 'TestLogin1'  
GO  
-- Following statement fails. TestLogin1 does not have REFERENCES   
-- permission on the XML schema collection.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
GO  
  
-- Final cleanup  
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)   
 [DENY – refus d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [GRANT – octroi d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
