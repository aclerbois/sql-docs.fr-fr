---
title: contient la fonction (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
caps.latest.revision: 42
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: fceddcf918a99667e8c92fadc7aeddca59bb21a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-string-values---contains"></a>Fonctions sur les valeurs de chaîne - contient
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une valeur de type xs : Boolean indiquant si la valeur de *$arg1* contient une valeur de chaîne spécifiée par *$arg2*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>Arguments  
 *arg1 $*  
 Valeur de chaîne à tester.  
  
 *arg2 $*  
 Sous-chaîne à rechercher.  
  
## <a name="remarks"></a>Notes  
 Si la valeur de *$arg2* est une chaîne de longueur nulle, la fonction retourne **True**. Si la valeur de *$arg1* est une chaîne de longueur zéro et la valeur de *$arg2* n’est pas une chaîne de longueur nulle, la fonction retourne **False**.  
  
 Si la valeur de *$arg1* ou *$arg2* est la séquence vide, l’argument est traité comme chaîne de longueur nulle.  
  
 La fonction contains() utilise le classement des points de code Unicode par défaut de XQuery pour la comparaison des chaînes.  
  
 La valeur de sous-chaîne spécifiée pour *$arg2* doit être inférieur ou égal à 4 000 caractères. Si la valeur spécifiée est supérieure à 4 000 caractères, une condition d’erreur dynamique se produit et la fonction contains() retourne une séquence vide au lieu d’une valeur booléenne de **True** ou **False**. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne déclenche pas d'erreurs dynamiques sur les expressions XQuery.  
  
 Pour obtenir des comparaisons sans respecter la casse, le [majuscules](../xquery/functions-on-string-values-upper-case.md) ou en minuscules fonctions peuvent être utilisées.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « XQuery fonctions sont substitut prenant en charge » dans la rubrique [modifications avec rupture des fonctionnalités du moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41; ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et [prise en charge Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type xml dans la base de données AdventureWorks.  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>A. Utilisation de la fonction contains() XQuery pour rechercher une chaîne de caractères spécifique.  
 La requête suivante recherche des produits qui contiennent le mot Aerodynamic dans les descriptions résumées. La requête retourne l'ID de produit et l'élément <`Summary`> pour ces produits.  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 Résultats  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
