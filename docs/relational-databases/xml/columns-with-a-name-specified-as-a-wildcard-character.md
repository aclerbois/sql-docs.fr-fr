---
title: "Colonnes avec un nom spécifié sous la forme d’un caractère générique | Microsoft Docs"
ms.custom: 
ms.date: 03/01/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: xml
ms.reviewer: 
ms.suite: sql
ms.technology:
- dbe-xml
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- names [SQL Server], columns with
ms.assetid: d9551df1-5bb4-4c0b-880a-5bb049834884
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 7ea97a62d0e1869d45146fd54b39f73d0114382e
ms.sourcegitcommit: 37f0b59e648251be673389fa486b0a984ce22c81
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
# <a name="columns-with-a-name-specified-as-a-wildcard-character"></a>Colonnes avec un nom spécifié sous la forme d'un caractère générique
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
Si le nom de colonne spécifié est un caractère générique (\*), le contenu de la colonne concernée est inséré comme si aucun nom de colonne n’était spécifié. Si cette colonne est une colonne de type non-**xml** , le contenu de la colonne est inséré en tant que nœud de texte, comme le montre l’exemple suivant :  
  
```  
USE AdventureWorks2012;  
GO  
SELECT E.BusinessEntityID "@EmpID",   
       FirstName "*",   
       MiddleName "*",   
       LastName "*"  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
    ON E.BusinessEntityID = P.BusinessEntityID  
WHERE E.BusinessEntityID=1  
FOR XML PATH;  
```  
  
 Voici le résultat obtenu :  
  
 `<row EmpID="1">KenJSánchez</row>`  
  
 Si la colonne est de type **xml** , l’arborescence XML correspondante est insérée. Par exemple, la requête suivante spécifie « * » pour le nom de la colonne qui contient le document XML renvoyé par la requête XQuery portant sur la colonne Instructions.  
  
```  
SELECT   
       ProductModelID,  
       Name,  
       Instructions.query('declare namespace MI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
                /MI:root/MI:Location   
              ') as "*"  
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH;   
GO  
```  
  
 Voici l'ensemble de résultats. le document XML renvoyé par la requête XQuery est inséré sans élément d'habillage.  
  
 `<row>`  
  
 `<ProductModelID>7</ProductModelID>`  
  
 `<Name>HL Touring Frame</Name>`  
  
 `<MI:Location LocationID="10">...</MI:Location>`  
  
 `<MI:Location LocationID="20">...</MI:Location>`  
  
 `...`  
  
 `</row>`  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  
