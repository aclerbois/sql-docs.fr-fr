---
title: SQL :Column() (fonction) (XQuery) | Documents Microsoft
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
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
caps.latest.revision: 38
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 19a427f43667718225120cdd72a571eba66cd041
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xquery-extension-functions---sqlcolumn"></a>Fonctions de XQuery Extension - SQL :Column()
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Comme décrit dans la rubrique [de liaison de données relationnelles à l’intérieur de XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), vous pouvez utiliser la **:Column()** fonctionnent lorsque vous utilisez [les méthodes de Type de données XML](../t-sql/xml/xml-data-type-methods.md) pour exposer une valeur relationnelle dans XQuery.  
  
 Par exemple, le [méthode query() (type de données XML)](../t-sql/xml/query-method-xml-data-type.md) est utilisée pour spécifier une requête sur une instance XML qui est stockée dans une variable ou une colonne de **xml** type. Il se peut également que vous vouliez que votre requête utilise des valeurs provenant d'une autre colonne non XML, afin de regrouper des données relationnelles et des données XML. Pour ce faire, vous utilisez la **SQL :Column()** (fonction).  
  
 La valeur SQL est alors mappée à sa valeur XQuery correspondante et son type à son type de base XQuery équivalent au type SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>Notes  
 Notez qu’une référence à une colonne spécifiée dans le **SQL :Column()** fonction à l’intérieur d’une requête XQuery fait référence à une colonne dans la ligne qui est en cours de traitement.  
  
 Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous pouvez uniquement faire référence à un **xml** instruction d’insertion de l’instance dans le contexte de l’expression source d’un XML-DML ; sinon, vous ne peut pas faire référence aux colonnes de type **xml** ou un type défini par l’utilisateur CLR.  
  
 Le **SQL :Column()** fonction n’est pas prise en charge dans les opérations de jointure. Vous pouvez utiliser l'opération APPLY à la place.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>A. Utilisation de sql:column() pour récupérer une valeur relationnelle dans du code XML  
 Concernant la construction XML, l'exemple suivant illustre la méthode à suivre pour récupérer des valeurs dans une colonne relationnelle non XML afin de lier les données XML et les données relationnelles.  
  
 La requête construit du code XML se présentant sous la forme suivante :  
  
```  
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 Il est à noter les points suivants dans le code XML ainsi construit :  
  
-   Le **ProductID**, **ProductName**, et **ProductPrice** valeurs d’attribut sont obtenues à partir de la **produit** table.  
  
-   Le **ProductModelID** valeur d’attribut est extraite de la **ProductModel** table.  
  
-   Pour rendre la requête plus intéressante, la **ProductModelName** valeur d’attribut est obtenue à partir de la **CatalogDescription** colonne de **type xml**. Comme les informations du catalogue de modèles de produits XML ne sont pas stockées pour tous les modèles de produits, l'instruction `if` ne permet de récupérer la valeur que si elle existe.  
  
    ```  
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Les valeurs étant issues de deux tables différentes, la clause FROM doit mentionner ces deux tables. La condition stipulée dans la clause WHERE filtre le résultat et ne récupère ainsi que les produits dont les modèles disposent d'une description mentionnée dans le catalogue.  
  
-   Le **espace de noms** mot clé dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit le préfixe d’espace de noms XML, « pd », qui est utilisé dans le corps de la requête. Il reste à noter que les alias de table, à savoir « P » et « PM », sont définis dans la clause FROM de la requête même.  
  
-   Le **SQL :Column()** fonction est utilisée pour afficher des valeurs non-XML dans du code XML.  
  
 Voici le résultat partiel :  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 La requête suivante construit du code XML contenant des informations spécifiques à chaque produit. Ces informations comprennent les colonnes ProductID (correspondant à l'identificateur du produit), ProductName (le nom du produit), ProductPrice (son prix) et le cas échéant, ProductModelName (le nom de son modèle) pour les produits appartenant à un modèle donné ; dans le cas présenté ci-dessous, ProductModelID = 19. Le code XML est ensuite assigné à la @x variable de **xml** type.  
  
```  
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions d’Extension XQuery SQL Server](http://msdn.microsoft.com/library/4bc5d499-5fec-4c3f-b11e-5ab5ef9d8f97)   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Créer des instances de données XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [Méthodes des types de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
