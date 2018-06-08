---
title: Reduce (type de données geometry) | Microsoft Docs
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
f1_keywords:
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: 132184bf-c4d2-4a27-900d-8373445dce2a
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 77443179047fe35f643d4728d3c09bf423b26be6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reduce-geometry-data-type"></a>Reduce (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Retourne une approximation de l’instance **geometry** donnée en exécutant une extension de l’algorithme de Douglas-Peucker sur l’instance, avec la tolérance donnée.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.Reduce ( tolerance )  
```  
  
## <a name="arguments"></a>Arguments  
 *tolerance*  
 Valeur de type **float**. *tolerance* est la tolérance à entrer pour l’algorithme d’approximation.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Notes   
 Pour les types de collection, cet algorithme fonctionne indépendamment sur chaque **geometry** contenu dans l’instance.  
  
 Cet algorithme ne modifie pas les instances **Point**.  
  
 Sur les instances **LineString**, **CircularString** et **CompoundCurve**, l’algorithme d’approximation conserve les points de début et de fin d’origine de l’instance. Il rajoute ensuite de manière itérative le point de l’instance d’origine qui dévie le plus du résultat, jusqu’à ce qu’il n’existe plus aucun point déviant de la tolérance donnée.  
  
 `Reduce()` retourne une instance **LineString**, **CircularString** ou **CompoundCurve** pour les instances **CircularString**.  `Reduce()` retourne une instance **CompoundCurve** ou **LineString** pour les instances **CompoundCurve**.  
  
 Sur les instances **Polygon**, l’algorithme d’approximation est appliqué indépendamment à chaque anneau. La méthode produit `FormatException` si l’instance **Polygon** retournée est non valide. Par exemple, une instance **MultiPolygon** non valide est créée si `Reduce()` est appliquée pour simplifier chaque anneau de l’instance et si les anneaux résultants se chevauchent.  Sur les instances **CurvePolygon** avec un anneau extérieur et sans anneau intérieur, `Reduce()` retourne une instance **CurvePolygon**, **LineString** ou **Point**.  Si **CurvePolygon** a des anneaux intérieurs, une instance **CurvePolygon** ou **MultiPoint** est retournée.  
  
 Lorsqu'un segment d'arc de cercle est rencontré, l'algorithme d'approximation vérifie si l'arc peut se rapprocher par sa pression simultanée dans la moitié de la tolérance donnée.  Si la pression simultanée satisfait à ce critère, l'arc circulaire est remplacé dans les calculs par la pression simultanée. S'il ne respecte pas ce critère, l'arc circulaire est conservé et l'algorithme d'approximation est appliqué aux segments restants.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-reduce-to-simplify-a-linestring"></a>A. Utilisation de Reduce() pour simplifier un LineString  
 L'exemple suivant crée une instance `LineString` et utilise `Reduce()` afin de simplifier l'instance.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0, 2 1, 3 0, 4 1)', 0);  
SELECT @g.Reduce(.75).ToString();  
```  
  
### <a name="b-using-reduce-with-varying-tolerance-levels-on-a-circularstring"></a>B. Utilisation de Reduce() avec variation de niveaux de tolérance sur un CircularString  
 L’exemple suivant utilise `Reduce()` avec trois niveaux de tolérance sur une instance **CircularString** :  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0)'; 
 SELECT @g.Reduce(5).ToString(); 
 SELECT @g.Reduce(15).ToString(); 
 SELECT @g.Reduce(16).ToString();
 ```  
  
 Cet exemple produit la sortie suivante :  
  
 ```
 CIRCULARSTRING (0 0, 8 8, 16 0, 20 -4, 24 0) 
 COMPOUNDCURVE (CIRCULARSTRING (0 0, 8 8, 16 0), (16 0, 24 0)) 
 LINESTRING (0 0, 24 0)
 ```  
  
 Chacune des instances retournées contient les points de terminaison (0 0) et (24 0).  
  
### <a name="c-using-reduce-with-varying-tolerance-levels-on-a-compoundcurve"></a>C. Utilisation de Reduce() avec variation des niveaux de tolérance sur un CompoundCurve  
 L’exemple suivant utilise `Reduce()` avec deux niveaux de tolérance sur une instance **CompoundCurve** :  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0),(24 0, 20 4, 16 0))';  
 SELECT @g.Reduce(15).ToString();  
 SELECT @g.Reduce(16).ToString();
 ```  
  
 Dans cet exemple, notez que la deuxième instruction **SELECT** retourne l’instance **LineString** : `LineString(0 0, 16 0)`.  
  
### <a name="showing-an-example-where-the-original-start-and-end-points-are-lost"></a>Affichage d'un exemple où les points de début et de fin d'origine sont perdus  
 L'exemple suivant affiche comment les points de début et de fin d'origine peuvent ne pas être conservés par l'instance résultante. Cela se produit, car la conservation des points de début et de fin d’origine donne lieu à une instance **LineString** non valide.  
  
```  
DECLARE @g geometry = 'LINESTRING(0 0, 4 0, 2 .01, 1 0)';  
DECLARE @h geometry = @g.Reduce(1);  
SELECT @g.STIsValid() AS Valid  
SELECT @g.ToString() AS Original, @h.ToString() AS Reduced;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes geometry statiques étendues](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
  

