---
title: Fonction de valeurs distinctes (XQuery) | Documents Microsoft
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
dev_langs:
- XML
helpviewer_keywords:
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
caps.latest.revision: 26
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 67534172dc879b7865af535e8a85f4a69d8dfc68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-sequences---distinct-values"></a>Fonctions sur les séquences - valeurs distinctes
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime les doublons de la séquence spécifiée par *$arg*. Si *$arg* est une séquence vide, la fonction retourne la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence de valeurs atomiques.  
  
## <a name="remarks"></a>Notes  
 Tous les types de valeurs atomisées transmises à **values** doivent être des sous-types du même type de base. Types de base acceptés sont les types qui prennent en charge la **eq** opération. Ces types incluent les trois types numériques de base intégrés, les types de base date/heure et les types xs:string (chaîne), xs:boolean (booléen) et xdt:untypedAtomic (atomique non typé). Les valeurs de type xdt:untypedAtomic sont converties en type xs:string. En cas de mélange de ces types ou si des valeurs d'autres types sont transmis, une erreur statique se produit.  
  
 Le résultat de **values** reçoit le type de base des types transmis, tel que xs : String dans le cas de xdt : untypedAtomic, avec la cardinalité d’origine. Si l'entrée est vide (valeur empty) de façon statique, « empty » est alors implicite et une erreur statique est émise.  
  
 Les valeurs de type xs:string sont comparées au classement de point de codes Unicode par défaut de XQuery.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>A. Utilisation de la fonction distinct-values() pour supprimer les valeurs en double d'une séquence  
 Dans cet exemple, une instance XML qui contient les numéros de téléphone est affectée à un **xml** variable de type. La requête XQuery sur cette variable utilise le **values** fonction pour compiler une liste de numéros de téléphone qui ne contiennent pas de doublons.  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 Voici le résultat obtenu :  
  
```  
111-111-1111 222-222-2222    
```  
  
 Dans la requête suivante, une séquence de nombres (1, 1, 2) est transmise à la **values** (fonction). La fonction supprime ensuite la valeur en double de la séquence et renvoie les deux autres.  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 La requête renvoie « 1 2 ».  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **values** fonction mappe les valeurs entières à xs : decimal.  
  
-   Le **values** fonction prend en charge les types précédemment mentionnés uniquement et ne prend pas en charge le mélange de types de base.  
  
-   Le **values** fonction sur les valeurs xs : Duration n’est pas pris en charge.  
  
-   L'option syntaxique fournissant un classement n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
