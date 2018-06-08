---
title: la fonction de local-name (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- fn:local-name function
- local-name function
ms.assetid: c901ef5d-89c5-482a-bf64-3eefbcf3098d
caps.latest.revision: 14
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: a5cdd64e6c283a41a4a51f71f84381b584d03f4d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-nodes---local-name"></a>Fonctions sur les nœuds - local-name
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la partie locale du nom de *$arg* en tant que xs : String qui sera la chaîne de longueur nulle ou qui ont la forme lexicale de xs : NCName. Si l'argument n'est pas spécifié, la valeur par défaut est le nœud du contexte.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn:local-name() as xs:string  
fn:local-name($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nom du nœud dont la partie local-name sera extraite.  
  
## <a name="remarks"></a>Notes  
  
-   Dans SQL Server, **fn :local-name()** sans argument peut uniquement être utilisé dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets (`[ ]`).  
  
-   Si l'argument est fourni et qu'il correspond à la séquence vide, la fonction retourne la chaîne de longueur zéro.  
  
-   Si le nœud cible n'a pas de nom car il s'agit d'un nœud de document, de commentaire ou de texte, la fonction retourne la chaîne de longueur zéro.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-retrieve-local-name-of-a-specific-node"></a>A. Extraction du nom local d'un nœud spécifique  
 La requête suivante est spécifiée sur une instance XML non typée. L'expression de requête, `local-name(/ROOT[1])`, extrait la partie locale du nom du nœud spécifié.  
  
```  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('local-name(/ROOT[1])')  
-- result = ROOT  
```  
  
 La requête suivante est définie sur la colonne Instructions, une colonne xml typée, de la table ProductModel. L'expression `local-name(/AWMI:root[1]/AWMI:Location[1])` retourne le nom local, `Location`, du nœud spécifié.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     local-name(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
-- result = Location  
```  
  
### <a name="b-using-local-name-without-argument-in-a-predicate"></a>B. Utilisation de local-name sans argument dans un prédicat  
 La requête suivante porte sur la colonne Instructions, tapée **xml** colonne, de la table ProductModel. L'expression retourne tous les enfants de l'élément <`root`> dont la partie locale de QName est « Location ». Le **dépourvue** fonction n’est spécifiée dans le prédicat et il n’a aucun argument le nœud de contexte est utilisé par la fonction.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
  /AWMI:root//*[local-name() = "Location"]') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 La requête retourne tous les enfants <`Location`> de l'élément <`root`>.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions sur les nœuds](http://msdn.microsoft.com/library/09a8affa-3341-4f50-aebc-fdf529e00c08)   
 [Fonction namespace-uri &#40;XQuery&#41;](../xquery/functions-on-nodes-namespace-uri.md)  
  
  
