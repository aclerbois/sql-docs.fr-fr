---
title: "STNumCurves (Type de données geometry) | Documents Microsoft"
ms.custom: 
ms.date: 08/03/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- STNumCurves method (geometry)
ms.assetid: 20c2fa0b-656b-4519-b34c-cc8f094290d4
caps.latest.revision: 15
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: 718289c6785bec8f9bba15ccec76507ac2501f47
ms.contentlocale: fr-fr
ms.lasthandoff: 09/01/2017

---
# <a name="stnumcurves-geometry-data-type"></a>STNumCurves (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Cette méthode retourne le nombre de courbes dans une **geometry** instance lorsque l’instance est un type de données spatiales unidimensionnelles. Types de données spatiales unidimensionnelles incluent **LineString**, **CircularString**, et **CompoundCurve**. `STNumCurves`() fonctionne uniquement sur les types simples ; Il ne fonctionne pas avec **geometry** comme des collections **MultiLineString**.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STNumCurves()  
```  
  
## <a name="return-types"></a>Types de retour  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]type de retour : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Notes  
 Unidimensionnel vide **geometry** instance retourne 0. **NULL** est retourné lorsque la **geometry** instance n’est pas une instance unidimensionnelle ou est une instance non initialisée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-stnumcurves-on-a-circularstring-instance"></a>A. Utilisation de STNumCurves() sur une instance CircularString  
 L'exemple suivant indique comment obtenir le nombre de courbes dans une instance `CircularString` :  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');`  
  
 `SELECT @g.STNumCurves();`  
  
### <a name="b-using-stnumcurves-on-a-compoundcurve-instance"></a>B. Utilisation de STNumCurves() sur une instance CompoundCurve  
 L'exemple suivant utilise `STNumCurves()` pour retourner le nombre de courbes dans une instance `CompoundCurve`.  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))');`  
  
 `SELECT @g.STNumCurves();`  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [Méthodes OGC sur les Instances géométriques](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

