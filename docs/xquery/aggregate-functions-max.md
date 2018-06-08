---
title: nombre maximum de fonction (XQuery) | Documents Microsoft
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
- max function [XQuery]
- fn:max function
ms.assetid: 5ee625c0-044a-4cda-b210-02b64e619d65
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: a53b02bc682bf7b3c918a02d5a16dc326ca3a594
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="aggregate-functions---max"></a>Fonctions d’agrégation - maximales
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Renvoie, à partir d’une séquence de valeurs atomiques, *$arg*, le seul élément dont la valeur est supérieure à celle de toutes les autres.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:max($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence de valeurs atomiques à partir de laquelle la valeur maximale est renvoyée.  
  
## <a name="remarks"></a>Notes  
 Tous les types de valeurs atomisées transmises à **max()** doivent être des sous-types du même type de base. Types de base acceptés sont les types qui prennent en charge la **gt** opération. Ces types incluent les trois types numériques de base intégrés, les types de base date/heure et les types xs:string (chaîne), xs:boolean (booléen) et xdt:untypedAtomic (atomique non typé). Les valeurs de type xdt:untypedAtomic sont converties en xs:double. S’il existe un mélange de ces types, ou si d’autres valeurs d’autres types sont passés, une erreur statique est déclenchée.  
  
 Le résultat de **max()** reçoit le type de base des types transmis, tel que xs : double dans le cas de xdt : untypedAtomic. Si l'entrée est vide (valeur empty) de façon statique, « empty » est alors implicite et une erreur statique est émise.  
  
 Le **max()** fonction retourne la valeur de la séquence qui est supérieure à un autre dans la séquence d’entrée. Pour les valeurs xs:string, le classement par défaut des points de code Unicode est utilisé. Si une valeur xdt : untypedAtomic ne peut pas être convertie en xs : double, la valeur est ignorée dans la séquence d’entrée, *$arg*. Si l'entrée est une séquence vide calculée de manière dynamique, la séquence vide est renvoyée.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** type des colonnes dans le [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-using-the-max-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-that-have-the-most-labor-hours"></a>A. Utilisation de la fonction XQuery max() pour localiser les postes de travail du processus de fabrication enregistrant le plus d'heures de main-d'œuvre  
 La requête fournie dans [fonction min (XQuery)](../xquery/aggregate-functions-min.md) peut être réécrit pour utiliser le **max()** (fonction).  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **max (**) fonction mappe tous les entiers à xs : decimal.  
  
-   Le **max()** fonction sur des valeurs de type xs : Duration n’est pas pris en charge.  
  
-   Les séquences faisant intervenir plusieurs types dérivés de différents types de base ne sont pas prises en charge.  
  
-   L'option syntaxique fournissant un classement n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
