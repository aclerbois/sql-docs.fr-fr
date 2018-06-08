---
title: SUBSTRING (fonction) (XQuery) | Documents Microsoft
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
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
caps.latest.revision: 42
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4bf01599d3144ca6eb3ebbfa74435ab16b25176a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-string-values---substring"></a>Fonctions sur des valeurs de chaîne - sous-chaîne
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une partie de la valeur de *$sourceString*, en commençant à la position indiquée par la valeur de *$startingLoc,* et se poursuit pendant le nombre de caractères indiqué par la valeur de *$length*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc  as as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>Arguments  
 *$sourceString*  
 Chaîne source.  
  
 *$startingLoc*  
 Point de départ dans la chaîne source à partir duquel la sous-chaîne commence. Si cette valeur est inférieure ou égale à 0, seuls les caractères dans des positions supérieures à zéro sont retournés. Si elle est supérieure à la longueur de la *$sourceString*, la chaîne de longueur nulle est retournée.  
  
 *$length*  
 [facultatif] Nombre de caractères à extraire. Si non spécifié, il retourne tous les caractères à partir de l’emplacement spécifié dans *$startingLoc* jusqu'à la fin de chaîne.  
  
## <a name="remarks"></a>Notes  
 La version à trois arguments de la fonction retourne les caractères de `$sourceString` dont la position `$p` obéit à :  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 La valeur de *$length* peut être supérieur au nombre de caractères dans la valeur de *$sourceString* après la position de début. Dans ce cas, la sous-chaîne retourne les caractères jusqu'à la fin de *$sourceString*.  
  
 Le premier caractère d'une chaîne se trouve à la position 1.  
  
 Si la valeur de *$sourceString* est la séquence vide, elle est gérée en tant que la chaîne de longueur nulle. Sinon, si le paramètre *$startingLoc* ou *$length* est la séquence vide, la séquence vide est retournée.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « XQuery fonctions sont substitut prenant en charge » dans la rubrique [modifications avec rupture des fonctionnalités du moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41; ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et [prise en charge Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 SQL Server requiert le *$startingLoc* et *$length paramètres* de type xs : decimal plutôt que xs : double.  
  
 SQL Server permet de *$startingLoc* et *$length* à la séquence vide, étant donné que la séquence vide est une valeur possible suite à des erreurs dynamiques en cours de mappage ().  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** type des colonnes dans le [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>A. Utilisation de la fonction XQuery substring() pour extraire une synthèse partielle des descriptions de modèles de produits.  
 La requête extrait les 50 premiers caractères du texte qui décrit le modèle de produit, l'élément <`Summary`> dans le document.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le **string()** fonction retourne la valeur de chaîne de la <`Summary`> élément. Cette fonction est utilisée car l'élément <`Summary`> contient à la fois le texte et les sous-éléments (éléments de mise en forme html) et car vous ignorerez ces éléments et récupérerez tout le texte.  
  
-   Le **substring()** fonction extrait les 50 premiers caractères à partir de la valeur de chaîne récupérée par le **string()**.  
  
 Voici un extrait du résultat :  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
