---
title: 'Exemple : extraction d’informations sur les employés | Microsoft Docs'
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
- EXPLICIT mode
ms.assetid: 63cd6569-2600-485b-92b4-1f6ba09db219
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 63715be88d37eb13dc43e914266a81e4730b7b19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-retrieving-employee-information"></a>Exemple : extraction d'informations sur les employés
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cet exemple extrait un ID et un nom pour chaque employé. Dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , les ID des employés se trouvent dans la colonne BusinessEntityID de la table Employee. Les noms des employés figurent dans la table Person. La colonne BusinessEntityID peut être utilisée pour joindre les tables.  
  
 Supposons que vous souhaitez générer un document XML à l'aide de la transformation FOR XML EXPLICIT comme suit :  
  
```  
<Employee EmpID="1" >  
  <Name FName="Ken" LName="Sánchez" />  
</Employee>  
...  
```  
  
 Étant donné que la hiérarchie comprend deux niveaux, vous écrivez deux requêtes `SELECT` et appliquez UNION ALL. Voici la première requête qui extrait les valeurs de l'élément <`Employee`> et de ses attributs. La requête attribue `1` comme valeur `Tag` pour l'élément <`Employee`> et NULL comme valeur `Parent`, car il s'agit de l'élément de niveau supérieur.  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID AS [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 Voici la seconde requête. Elle extrait les valeurs de l'élément <`Name`>. Elle attribue `2` comme valeur `Tag` pour l'élément <`Name`> et `1` comme valeur de balise `Parent` identifiant <`Employee`> en tant que parent.  
  
```  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 Vous combinez ces requêtes avec `UNION AL`L, appliquez `FOR XML EXPLICIT` et spécifiez la clause `ORDER BY` requise. Vous devez trier l'ensemble de lignes par `BusinessEntityID` puis par nom afin que les valeurs NULL du nom apparaissent en premier. En exécutant la requête suivante sans la clause FOR XML, vous pouvez visualiser la table universelle générée.  
  
 Voici la requête finale :  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID as [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
ORDER BY [Employee!1!EmpID],[Name!2!FName]  
FOR XML EXPLICIT;  
```  
  
 Voici le résultat partiel :  
  
 `<Employee EmpID="1">`  
  
 `<Name FName="Ken" LName="Sánchez" />`  
  
 `</Employee>`  
  
 `<Employee EmpID="2">`  
  
 `<Name FName="Terri" LName="Duffy" />`  
  
 `</Employee>`  
  
 `...`  
  
 La première instruction `SELECT` spécifie les noms des colonnes dans l'ensemble de lignes obtenu. Ces noms forment deux groupes de colonnes. Le groupe qui possède la valeur `Tag` `1` dans le nom de colonne identifie `Employee` en tant qu'élément et `EmpID` en tant qu'attribut. L'autre groupe de colonnes possède la valeur `Tag` `2` dans la colonne et identifie <`Name`> en tant qu'élément et `FName` et `LName` en tant qu'attributs.  
  
 Le tableau suivant montre l'ensemble de lignes partiel généré par la requête :  
  
 `Tag Parent  Employee!1!EmpID Name!2!FName Name!2!LName`  
  
 `--- ------  ---------------- ------------ ------------`  
  
 `1   NULL    1                NULL         NULL`  
  
 `2   1       1                Ken          Sánchez`  
  
 `1   NULL    2                NULL         NULL`  
  
 `2   1       2                Terri        Duffy`  
  
 `1   NULL    3                NULL         NULL`  
  
 `2   1       3                Roberto      Tamburello`  
  
 `...`  
  
 Les lignes de la table universelle sont traitées comme suit pour générer l'arborescence XML obtenue :  
  
 La première ligne identifie la valeur `Tag` `1`. Par conséquent, le groupe de colonnes qui a la valeur `Tag``1` est identifié, `Employee!1!EmpID`. Cette colonne identifie `Employee` en tant que nom d'élément. Un élément <`Employee`> possédant les attributs `EmpID` est ensuite créé. Les valeurs de colonnes correspondantes sont affectées à ces attributs.  
  
 La deuxième ligne a la valeur `Tag`  `2`. Par conséquent, le groupe de colonnes qui a la valeur `Tag``2` dans le nom de colonne, `Name!2!FName`, `Name!2!LName`, est identifié. Ces noms de colonnes identifient `Name` comme nom d'élément. Un élément <`Name`> possédant les attributs `FName` et `LName` est créé. Les valeurs de colonnes correspondantes sont ensuite affectées à ces attributs. Cette ligne identifie `1` comme `Parent`. Cet élément enfant est ajouté à l'élément <`Employee`> précédent.  
  
 Ce processus est répété pour le reste des lignes de l'ensemble de lignes. Notez l'importance du tri des lignes dans la table universelle pour que FOR XML EXPLICIT puisse traiter l'ensemble de lignes dans l'ordre et générer le document XML souhaité.  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
