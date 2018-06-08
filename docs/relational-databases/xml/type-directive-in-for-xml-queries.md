---
title: Directive TYPE dans les requêtes FOR XML | Microsoft Docs
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
- FOR XML clause, TYPE directive
- TYPE directive
ms.assetid: a3df6c30-1f25-45dc-b5a9-bd0e41921293
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2ec549950b912f4a2bd877163b60042c9820ae3c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="type-directive-in-for-xml-queries"></a>Directive TYPE dans les requêtes FOR XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  La prise en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de [xml &#40;Transact-SQL&#41;](../../t-sql/xml/xml-transact-sql.md) vous permet éventuellement de demander que le résultat d’une requête FOR XML soit renvoyé en tant que type de données **xml** en spécifiant la directive TYPE. Cela vous permet de traiter le résultat d'une requête FOR XML sur le serveur. Par exemple, vous pouvez spécifier une requête XQuery par rapport au résultat, affecter le résultat à une variable de type **xml** ou écrire des [requêtes FOR XML imbriquées](../../relational-databases/xml/use-nested-for-xml-queries.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des données d’instance de type de données XML au client en résultat de différentes constructions de serveur telles que des requêtes FOR XML qui utilisent la directive TYPE ou dans lesquelles le type de données **xml** permet de renvoyer des valeurs de données d’instance XML à partir de paramètres de sortie et de colonnes de table SQL. Dans le code de l'application cliente, le fournisseur ADO.NET demande à ce que ces informations de type de données XML soient envoyées dans un encodage binaire à partir du serveur. Toutefois, si vous utilisez FOR XML sans la directive TYPE, les données XML reviennent en tant que type chaîne. Dans tous les cas, le fournisseur client est toujours en mesure de gérer toute forme XML. Notez qu'une clause FOR XML de premier niveau sans directive TYPE ne peut pas être utilisée avec les curseurs.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l'utilisation de la directive TYPE avec des requêtes FOR XML.  
  
### <a name="retrieving-for-xml-query-results-as-xml-type"></a>Extraction des résultats de la requête FOR XML en tant que type xml  
 La requête suivante extrait des informations de contact client de la table `Contacts` . Étant donné que la directive `TYPE` est spécifiée dans `FOR XML`, le résultat est retourné en tant que type **xml** .  
  
```  
USE AdventureWorks2012;  
Go  
SELECT BusinessEntityID, FirstName, LastName  
FROM Person.Person  
ORDER BY BusinessEntityID  
FOR XML AUTO, TYPE;  
```  
  
 Voici le résultat partiel :  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez"/>`  
  
 `<Person.Person BusinessEntityID="2" FirstName="Terri" LastName="Duffy"/>`  
  
 `...`  
  
### <a name="assigning-for-xml-query-results-to-an-xml-type-variable"></a>Affectation des résultats de la requête FOR XML à une variable de type xml  
 Dans l’exemple suivant, un résultat FOR XML est affecté à une variable de type **xml**, `@x`. La requête récupère les informations de contact, telles que `BusinessEntityID`, `FirstName`, `LastName` et des numéros de téléphone supplémentaires, à partir de la colonne `AdditionalContactInfo` de `TYPE` **xml**. Sachant que la clause `FOR XML` spécifie la directive `TYPE`, le résultat XML est retourné en tant que type **xml** et affecté à une variable.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @x xml;  
SET @x = (  
   SELECT BusinessEntityID,   
          FirstName,   
          LastName,   
          AdditionalContactInfo.query('  
declare namespace aci="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
              //act:telephoneNumber/act:number') as MorePhoneNumbers  
   FROM Person.Person  
   FOR XML AUTO, TYPE);  
SELECT @x;  
GO  
```  
  
### <a name="querying-results-of-a-for-xml-query"></a>Interrogation des résultats d'une requête FOR XML  
 Les requêtes FOR XML renvoient des données XML. Par conséquent, vous pouvez appliquer des méthodes de type **xml** , telles que **query()** et **value()**, au résultat XML retourné par des requêtes FOR XML.  
  
 Dans la requête suivante, la méthode `query()` du type de données **xml** permet d’interroger le résultat de la requête `FOR XML`. Pour plus d’informations, consultez [Méthode query&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/query-method-xml-data-type.md).  
  
```  
USE AdventureWorks2012;  
GO  
SELECT (SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
DECLARE namespace aci="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
DECLARE namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumbers  
FROM Person.Person  
FOR XML AUTO, TYPE).query('/Person.Person[1]');  
```  
  
 La requête `SELECT … FOR XML` interne retourne un résultat de type **xml** auquel la clause `SELECT` externe applique la méthode `query()` au type **xml**. Notez la directive `TYPE` spécifiée.  
  
 Voici le résultat obtenu :  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez">`  
  
 `<PhoneNumbers>`  
  
 `<act:number xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">111-111-1111</act:number>`  
  
 `<act:number xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">112-111-1111</act:number>`  
  
 `</PhoneNumbers>`  
  
 `</Person.Person>`  
  
 Dans la requête suivante, la méthode `value()` du type de données **xml** permet de récupérer une valeur du résultat XML retourné par la requête `SELECT…FOR XML`. Pour plus d’informations, consultez [Méthode value&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/value-method-xml-data-type.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @FirstPhoneFromAdditionalContactInfo varchar(40);  
SELECT @FirstPhoneFromAdditionalContactInfo =   
 ( SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace aci="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
   //act:telephoneNumber/act:number  
   ') AS PhoneNumbers  
   FROM Person.Person Contact  
   FOR XML AUTO, TYPE).value('  
declare namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
  /Contact[@BusinessEntityID="1"][1]/PhoneNumbers[1]/act:number[1]', 'varchar(40)'  
 )  
SELECT @FirstPhoneFromAdditionalContactInfo;  
```  
  
 L'expression de chemin d'accès XQuery dans la méthode `value()` extrait le premier numéro de téléphone d'un contact client dont `BusinessEntityID` a pour valeur `1`.  
  
> [!NOTE]  
>  Si la directive TYPE n’est pas spécifiée, le résultat de la requête FOR XML est retourné en tant que type **nvarchar(max)**.  
  
### <a name="using-for-xml-query-results-in-insert-update-and-delete-transact-sql-dml"></a>Utilisation des résultats de la requête FOR XML dans INSERT, UPDATE et DELETE (Transact-SQL DML)  
 L'exemple suivant montre comment des requêtes FOR XML peuvent être utilisées dans des instructions DML (Data Manipulation Language, langage de manipulation de données). Dans l’exemple, la requête `FOR XML` retourne une instance de type **xml** . L'instruction `INSERT` insère ce document XML dans une table.  
  
```  
CREATE TABLE T1(intCol int, XmlCol xml);  
GO  
INSERT INTO T1   
VALUES(1, '<Root><ProductDescription ProductModelID="1" /></Root>');  
GO  
  
CREATE TABLE T2(XmlCol xml)  
GO  
INSERT INTO T2(XmlCol)   
SELECT (SELECT XmlCol.query('/Root')   
        FROM T1   
        FOR XML AUTO,TYPE);   
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
