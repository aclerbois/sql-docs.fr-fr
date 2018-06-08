---
title: Prologue XQuery | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
- namespaces [XQuery]
- default namespace declarations
ms.assetid: 03924684-c5fd-44dc-8d73-c6ab90f5e069
caps.latest.revision: 26
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: debf4aac70b13c5bb5cbb37db2b71687f33a3df1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modules-and-prologs---xquery-prolog"></a>Modules et prologues - prologue XQuery
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Une requête XQuery se compose d'un prologue et d'un corps. Le prologue XQuery est une série de déclarations et de définitions qui créent ensemble l'environnement requis pour le traitement des requêtes. Dans SQL Server, le prologue XQuery peut inclure des déclarations d'espace de noms. Le corps XQuery se compose d'une séquence d'expressions qui spécifient le résultat de requête voulu.  
  
 Par exemple, la requête XQuery ci-dessous est spécifiée sur la colonne Instructions de **xml** type qui stocke les instructions de fabrication au format XML. La requête récupère les instructions de fabrication pour l'emplacement de l'atelier `10`. Le `query()` méthode de la **xml** type de données est utilisé pour spécifier la requête XQuery.  
  
```  
SELECT Instructions.query('declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') AS Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le prologue XQuery inclut une déclaration de préfixe (AWMI) d’espace de noms, `(namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";`.  
  
-   Le mot clé `declare namespace` définit un préfixe d'espace de noms utilisé ultérieurement dans le corps de la requête.  
  
-   `/AWMI:root/AWMI:Location[@LocationID="10"]` représente le corps de la requête.  
  
## <a name="namespace-declarations"></a>Déclarations d'espace de noms  
 Une déclaration d'espace de noms définit un préfixe et l'associe à un URI d'espace de noms, comme illustré dans la requête ci-dessous. Dans la requête, `CatalogDescription` est un **xml** colonne de type.  
  
 Lorsque vous spécifiez une requête XQuery sur cette colonne, le prologue de la requête spécifie la déclaration `declare namespace` pour associer le préfixe `PD` (description du produit) à l'URI d'espace de noms. Ce préfixe est alors utilisé dans le corps de la requête à la place de l'URI d'espace de noms. Les nœuds XML résultants sont dans l'espace de noms associé à l'URI d'espace de noms.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Pour améliorer la lisibilité de la requête, vous pouvez déclarer les espaces de noms à l'aide de WITH XMLNAMESPACES au lieu de déclarer une liaison entre des préfixes et des espaces de noms dans le prologue de la requête à l'aide de `declare namespace`.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
  
SELECT CatalogDescription.query('  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Pour plus d’informations, consultez, [espaces de noms à ajouter à des requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
### <a name="default-namespace-declaration"></a>Déclaration d'espace de noms par défaut  
 Au lieu de déclarer un préfixe d'espace de noms à l'aide de la déclaration `declare namespace`, vous pouvez utiliser la déclaration `declare default element namespace` pour lier un espace de noms par défaut pour des noms d'élément. Dans ce cas, vous ne spécifiez aucun préfixe.  
  
 Dans l'exemple suivant, l'expression désignant le chemin d'accès dans le corps de la requête ne spécifie pas de préfixe d'espace de noms. Par défaut, tous les noms d'élément appartiennent au même espace de noms par défaut, spécifié dans le prologue.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace  "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
 Vous pouvez déclarer un espace de noms par défaut en utilisant WITH XMLNAMESPACES :  
  
```  
WITH XMLNAMESPACES (DEFAULT 'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription')  
SELECT CatalogDescription.query('  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)  
  
  
