---
title: Fonction concat (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
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
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 8be65777bb65ad54735ad6bdf43ea88608355c5b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-string-values---concat"></a>Fonctions sur des valeurs de chaîne - concat
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Accepte zéro ou plusieurs chaînes comme arguments et renvoie une chaîne créée suite à la concaténation des valeurs de chacun de ces arguments.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$string*  
 Chaîne facultative à concaténer.  
  
## <a name="remarks"></a>Notes  
 La fonction requiert au moins deux arguments. Si un argument est une séquence vide, elle est traitée comme une chaîne de longueur zéro.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « XQuery fonctions sont substitut prenant en charge » dans la rubrique [modifications avec rupture des fonctionnalités du moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41; ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et [prise en charge Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>A. Utilisation de la fonction XQuery concat() pour concaténer des chaînes  
 Pour un modèle de produit spécifique, cette requête renvoie une chaîne obtenue d'après la concaténation de la période et de la description de la garantie. Dans le document de la description du catalogue, l'élément <`Warranty`> est constitué des éléments enfants <`WarrantyPeriod`> et <`Description`>.  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Dans la clause SELECT, CatalogDescription est une **xml** colonne de type. Par conséquent, le [méthode query() (type de données XML)](../t-sql/xml/query-method-xml-data-type.md), instructions.Query (), est utilisé. L'instruction XQuery est spécifiée comme argument de la méthode query.  
  
-   Le document sur lequel porte la requête utilise des espaces de noms. Par conséquent, le **espace de noms** mot clé est utilisé pour définir le préfixe pour l’espace de noms. Pour plus d’informations, consultez [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md).  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 La requête précédente récupère les informations se rapportant à un produit spécifique. La requête suivante récupère les mêmes informations mais pour tous les produits pour lesquels il existe des descriptions de catalogue XML. Le **exist()** méthode de la **xml** de type de données dans la clause WHERE renvoie la valeur True si le document XML dans les lignes a une <`ProductDescription`> élément.  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 Notez que la valeur booléenne retournée par le **exist()** méthode de la **xml** type est comparé à 1.  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **concat()** fonction dans SQL Server accepte seulement des valeurs de type xs : String. Les autres valeurs doivent être explicitement converties en xs:string ou en xdt:untypedAtomic.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
