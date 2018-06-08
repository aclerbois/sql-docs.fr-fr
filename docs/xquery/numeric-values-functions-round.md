---
title: Round, fonction (XQuery) | Documents Microsoft
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
- fn:round function
- round function [XQuery]
ms.assetid: 320b572f-bd5b-4055-95a6-dec5718c0041
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 69c9e11d9cb6dda3aa50a2d49e3eb9c55b99a57b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="numeric-values-functions---round"></a>Fonctions de valeurs numériques - arrondir
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne le nombre n'ayant pas de partie décimale qui est le plus proche de l'argument. S'il y a plusieurs nombres qui correspondent, le plus proche de l'infini positif est retourné. Par exemple :  
  
 Si l’argument est 2.5, **round()** retourne 3.  
  
 Si l’argument est 2.4999, **round()** retourne 2.  
  
 Si l’argument est de -2,5, **round()** retourne -2.  
  
 Si l’argument est une séquence vide, **round()** renvoie la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:round ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nombre à laquelle s'applique la fonction.  
  
## <a name="remarks"></a>Notes  
 Si le type de *$arg* est un des trois types numériques de base, **xs : float**, **xs : double**, ou **xs : decimal**, le type de retour est identique à celui du *$arg* type. Si le type de *$arg* est un type qui est dérivé de l’un des types numériques, le type de retour est le type numérique de base.  
  
 Si d’entrée pour le **Floor**, **fn : Ceiling**, ou **fn : Round** fonctions est **xdt : untypedAtomic**, les données non typées, elle est implicitement convertie en **xs : double**.  
  
 Tout autre type génère une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
 Vous pouvez utiliser l’exemple fonctionnel de la [fonction ceiling (XQuery)](../xquery/numeric-values-functions-ceiling.md) pour le **round()** fonction XQuery. Il vous suffit de remplacer le **ceiling()** fonction dans la requête avec la **round()** (fonction).  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **round()** fonction mappe les valeurs entières à xs : decimal.  
  
-   Le **round()** fonction de xs : double et les valeurs comprises entre - 0.5e0 et - 0e0 sont mappées à 0e0 au lieu de - 0e0.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction FLOOR &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Fonction CEILING &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)  
  
  
