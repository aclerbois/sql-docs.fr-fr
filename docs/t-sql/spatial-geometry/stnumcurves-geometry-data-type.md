---
title: STNumCurves (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|spatial-geography
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- STNumCurves method (geometry)
ms.assetid: 20c2fa0b-656b-4519-b34c-cc8f094290d4
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: eec3bd116f3d8f3a5a6dd485cbd02c5de5201fe3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stnumcurves-geometry-data-type"></a>STNumCurves (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Cette méthode retourne le nombre de courbes d’une instance **geometry** quand l’instance est d’un type de données spatiales unidimensionnel. Les types de données spatiales unidimensionnels incluent **LineString**, **CircularString** et **CompoundCurve**. `STNumCurves`() fonctionne uniquement sur les types simples. Il ne fonctionne pas avec les collections **geometry** telles que **MultiLineString**.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STNumCurves()  
```  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Notes   
 Une instance **geometry** unidimensionnelle vide retourne 0. **NULL** est retourné quand l’instance **geometry** n’est pas une instance unidimensionnelle ou une instance initialisée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-stnumcurves-on-a-circularstring-instance"></a>A. Utilisation de STNumCurves() sur une instance CircularString  
 L'exemple suivant indique comment obtenir le nombre de courbes dans une instance `CircularString` :  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');  
 SELECT @g.STNumCurves();
 ```  
  
### <a name="b-using-stnumcurves-on-a-compoundcurve-instance"></a>B. Utilisation de STNumCurves() sur une instance CompoundCurve  
 L'exemple suivant utilise `STNumCurves()` pour retourner le nombre de courbes dans une instance `CompoundCurve`.  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))');  
 SELECT @g.STNumCurves();
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

