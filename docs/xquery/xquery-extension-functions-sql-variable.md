---
title: SQL :variable() (fonction) (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
caps.latest.revision: 36
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 66e71e9748d143eb338d612046f97c50db014107
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xquery-extension-functions---sqlvariable"></a>Fonctions de XQuery Extension - SQL :variable()
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Expose une variable qui contient une valeur relationnelle SQL dans une expression XQuery.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>Notes  
 Comme décrit dans la rubrique [de liaison de données relationnelles à l’intérieur de XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), vous pouvez utiliser cette fonction lorsque vous utilisez [méthodes du type de données XML](../t-sql/xml/xml-data-type-methods.md) pour exposer une valeur relationnelle dans XQuery.  
  
 Par exemple, le [méthode query()](../t-sql/xml/query-method-xml-data-type.md) est utilisée pour spécifier une requête sur une instance XML qui est stockée dans un **xml** variable ou une colonne de type de données. En outre, vous pouvez parfois souhaiter que la requête utilise des valeurs d'une variable ou d'un paramètre [!INCLUDE[tsql](../includes/tsql-md.md)] afin de rassembler des données relationnelles et XML. Pour ce faire, vous utilisez la **SQL : variable** (fonction).  
  
 La valeur SQL sera mappée à une valeur XQuery correspondante et son type vont être un type de base XQuery équivalent au type SQL correspondant.  
  
 Vous pouvez uniquement faire référence à un **xml** instruction d’insertion de l’instance dans le contexte de l’expression source d’un XML-DML ; sinon, vous ne peut pas faire référence à des valeurs qui sont de type **xml** ou un type common language runtime (CLR) définis par l’utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>A. Utilisation de la fonction sql:variable() pour insérer une valeur variable Transact-SQL dans XML  
 L'exemple suivant construit une instance XML composée des éléments suivants :  
  
-   une valeur (`ProductID`) issue d'une colonne non-XML. Le [fonction SQL :Column()](../xquery/xquery-extension-functions-sql-column.md) est utilisée pour lier cette valeur dans le document XML.  
  
-   une valeur (`ListPrice`) issue d'une colonne non-XML d'une autre table. Là encore, `sql:column()` permet de lier cette valeur au document XML ;  
  
-   une valeur (`DiscountPrice`) issue d'une variable [!INCLUDE[tsql](../includes/tsql-md.md)]. La méthode `sql:variable()` permet de lier cette valeur au document XML ;  
  
-   Une valeur (`ProductModelName`) à partir d’un **xml** colonne de type pour rendre la requête plus intéressante.  
  
 Voici la requête :  
  
```  
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La requête XQuery dans la méthode `query()` construit le document XML.  
  
-   Le `namespace` est utilisé pour définir un préfixe d’espace de noms dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md). Cette opération est réalisée car la valeur d'attribut `ProductModelName` est extraite de la colonne de type `CatalogDescription xml`, à laquelle un schéma est associé.  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions d’Extension XQuery SQL Server](http://msdn.microsoft.com/library/4bc5d499-5fec-4c3f-b11e-5ab5ef9d8f97)   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Créer des instances de données XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [Méthodes des types de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
