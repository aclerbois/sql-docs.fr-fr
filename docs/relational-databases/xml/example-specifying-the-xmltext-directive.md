---
title: 'Exemple : spécification de la directive XMLTEXT | Microsoft Docs'
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XMLTEXT directive
ms.assetid: e78008ec-51e8-4fd1-b86f-1058a781de17
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2019e547dca2415c3b40f5e14c5286e0cd05682c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-specifying-the-xmltext-directive"></a>Exemple : spécification de la directive XMLTEXT
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cet exemple illustre l’adressage des informations contenues dans la colonne de dépassement de capacité à l’aide de la directive **XMLTEXT** dans une instruction `SELECT` utilisant le mode EXPLICIT.  
  
 Soit la table `Person` . Cette table possède une colonne nommée `Overflow` , qui stocke les données non consommées du document XML.  
  
```  
USE tempdb;  
GO  
CREATE TABLE Person(PersonID varchar(5), PersonName varchar(20), Overflow nvarchar(200));  
GO  
INSERT INTO Person VALUES   
    ('P1','Joe',N'<SomeTag attr1="data">content</SomeTag>')  
   ,('P2','Joe',N'<SomeTag attr2="data"/>')  
   ,('P3','Joe',N'<SomeTag attr3="data" PersonID="P">content</SomeTag>');  
```  
  
 La requête suivante extrait des colonnes de la table `Person` . Concernant la colonne `Overflow` , *AttributeName* n’est pas spécifié, mais *directive* a pour valeur `XMLTEXT` et fournit une composante du nom d’une colonne de la table universelle.  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!!XMLTEXT] -- No AttributeName; XMLTEXT directive  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Dans le document XML obtenu :  
  
-   Comme *AttributeName* n’est pas spécifié pour la colonne `Overflow` et que la directive `xmltext` est spécifiée, les attributs de l’élément <`overflow`> sont ajoutés à la liste d’attributs de l’élément englobant <`Parent`>.  
  
-   En raison du conflit entre l’attribut `PersonID` de l’élément <`xmltext`> et l’attribut `PersonID` extrait du même niveau d’éléments, l’attribut de l’élément <`xmltext`> est ignoré, même si `PersonID` a la valeur NULL. En général, un attribut remplace un attribut de même nom dans les données en excès.  
  
 Voici le résultat obtenu :  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe" attr1="data">content</Parent>  
 <Parent PersonID="P2" PersonName="Joe" attr2="data"></Parent>  
 <Parent PersonID="P3" PersonName="Joe" attr3="data">content</Parent>
 ```  
  
 Si les données en excès contiennent des sous-éléments et que la même requête est exécutée, les sous-éléments contenus dans la colonne `Overflow` sont ajoutés en tant que sous-éléments de l'élément englobant <`Parent`>.  
  
 Supposons, par exemple, que les données de la table `Person` sont modifiées afin que la colonne `Overflow` contienne des sous-éléments.  
  
```  
USE tempdb;  
GO  
TRUNCATE TABLE Person;  
GO  
INSERT INTO Person VALUES   
    ('P1','Joe',N'<SomeTag attr1="data">content</SomeTag>')  
   ,('P2','Joe',N'<SomeTag attr2="data"/>')  
    ,('P3','Joe',N'<SomeTag attr3="data" PersonID="P"><name>PersonName</name></SomeTag>');  
```  
  
 Si la même requête est exécutée, les sous-éléments de l'élément <`xmltext`> sont ajoutés en tant que sous-éléments de l'élément englobant <`Parent`> :  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!!XMLTEXT] -- no AttributeName, XMLTEXT directive  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Voici le résultat obtenu :  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe" attr1="data">content</Parent>  
 <Parent PersonID="P2" PersonName="Joe" attr2="data"></Parent>  
 <Parent PersonID="P3" PersonName="Joe" attr3="data">  
 <name>PersonName</name>  
 </Parent>
 ```  
  
 Si *AttributeName* est spécifié avec la directive `xmltext`, les attributs de l’élément <`overflow`> sont ajoutés en tant qu’attributs des sous-éléments de l’élément englobant <`Parent`>. Le nom spécifié pour *AttributeName* devient le nom du sous-élément.  
  
 Dans la requête suivante, *AttributeName*, <`overflow`>, est spécifié avec la directive `xmltext` *:*  
  
```  
SELECT 1 as Tag, NULL as parent,  
       PersonID as [Parent!1!PersonID],  
       PersonName as [Parent!1!PersonName],  
       Overflow as [Parent!1!overflow!XMLTEXT] -- Overflow is AttributeName  
                      -- XMLTEXT is a directive  
FROM Person  
FOR XML EXPLICIT  
```  
  
 Voici le résultat obtenu :  
  
 ```   
 <Parent PersonID="P1" PersonName="Joe">  
 <overflow attr1="data">content</overflow>  
 </Parent>  
 <Parent PersonID="P2" PersonName="Joe">  
 <overflow attr2="data" />  
 </Parent>  
 <Parent PersonID="P3" PersonName="Joe">  
 <overflow attr3="data" PersonID="P">  
 <name>PersonName</name>  
 </overflow>  
 </Parent>
 ```  
  
 Dans l’élément de requête suivant, l’argument *directive* est spécifié pour l’attribut `PersonName`. Par conséquent, `PersonName` est ajouté en tant que sous-élément de l'élément englobant <`Parent`>. Les attributs de <`xmltext`> sont néanmoins ajoutés à l'élément englobant <`Parent`>. Le contenu de l'élément <`overflow`>, les sous-éléments, sont ajoutés avant les autres sous-éléments des éléments englobants <`Parent`>.  
  
```  
SELECT 1      AS Tag, NULL as parent,  
       PersonID   AS [Parent!1!PersonID],  
       PersonName AS [Parent!1!PersonName!element], -- element directive  
       Overflow   AS [Parent!1!!XMLTEXT]  
FROM Person  
FOR XML EXPLICIT;  
```  
  
 Voici le résultat obtenu :  
  
 ```   
 <Parent PersonID="P1" attr1="data">content<PersonName>Joe</PersonName>  
 </Parent>  
 <Parent PersonID="P2" attr2="data">  
 <PersonName>Joe</PersonName>  
 </Parent>  
 <Parent PersonID="P3" attr3="data">  
 <name>PersonName</name>  
 <PersonName>Joe</PersonName>  
 </Parent>
 ```  
  
 Si les données de la colonne `XMLTEXT` contiennent des attributs sur l'élément racine, ces attributs ne sont pas montrés dans le schéma de données XML et l'analyseur MSXML ne valide pas le fragment de document XML obtenu. Exemple :  
  
```  
SELECT 1 AS Tag,  
       0 ASParent,  
       N'<overflow a="1"/>' AS 'overflow!1!!xmltext'  
FOR XML EXPLICIT, xmldata;  
```  
  
 Voici l'ensemble de résultats. Notez que l'attribut en excès `a` ne figure pas dans le schéma renvoyé :  
  
 ```   
 <Schema name="Schema2"  
 xmlns="urn:schemas-microsoft-com:xml-data"  
 xmlns:dt="urn:schemas-microsoft-com:datatypes">  
 <ElementType name="overflow" content="mixed" model="open">`  
 </ElementType>`  
 </Schema>`  
 <overflow xmlns="x-schema:#Schema2" a="1">  
 </overflow>
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
