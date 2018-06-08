---
title: CurvePolygon | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: spatial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-spatial
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e000a1d8-a049-4542-bfeb-943fd6ab3969
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 429719efec72ea5b05f9ea2daf0e0d8eac751917
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="curvepolygon"></a>CurvePolygon
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Un **CurvePolygon** est une surface topologiquement fermée définie par un anneau englobant extérieur et zéro ou plusieurs anneaux intérieurs.  
  
> [!IMPORTANT]  
>  Pour obtenir une description détaillée et des exemples des fonctionnalités spatiales introduites dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], y compris le sous-type **CurvePolygon** , téléchargez le livre blanc [Nouvelles fonctionnalités spatiales dans SQL Server 2012](http://go.microsoft.com/fwlink/?LinkId=226407).  
  
 Les critères suivants définissent les attributs d'une instance **CurvePolygon** :  
  
-   La limite de l'instance **CurvePolygon** est définie par l'anneau extérieur et tous les anneaux intérieurs.  
  
-   L'intérieur de l'instance **CurvePolygon** désigne l'espace situé entre l'anneau extérieur et tous les anneaux intérieurs.  
  
 Une instance **CurvePolygon** diffère d'une instance **Polygon** en cela qu'une instance **CurvePolygon** peut contenir les segments d'arc de cercle suivants : **CircularString** et **CompoundCurve**.  
  
## <a name="compoundcurve-instances"></a>Instances CompoundCurve  
 L'illustration ci-dessous montre des figures de **CurvePolygon** valides :  
  
### <a name="accepted-instances"></a>Instances acceptées  
 Pour qu'une instance **CurvePolygon** soit acceptée, elle doit être soit vide, soit contenir uniquement des anneaux d'arc circulaires acceptés. Un anneau d'arc circulaire accepté satisfait les exigences suivantes.  
  
1.  Être une instance acceptée **LineString**, **CircularString**ou **CompoundCurve** . Pour plus d'informations sur les instances acceptées, consultez [LineString](../../relational-databases/spatial/linestring.md), [CircularString](../../relational-databases/spatial/circularstring.md)et [CompoundCurve](../../relational-databases/spatial/compoundcurve.md).  
  
2.  Contenir au moins quatre points.  
  
3.  Le point de début et le point de fin ont les mêmes coordonnées X et Y.  
  
    > [!NOTE]  
    >  Les valeurs Z et M sont ignorées.  
  
 L'exemple suivant illustre des instances **CurvePolygon** acceptées.  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0, 0 0))';  
DECLARE @g3 geometry = 'CURVEPOLYGON((0 0 1, 0 0 2, 0 0 3, 0 0 3))'  
DECLARE @g4 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
DECLARE @g5 geography = 'CURVEPOLYGON((-122.3 47, 122.3 -47, 125.7 -49, 121 -38, -122.3 47))';  
```  
  
 `@g3` est accepté bien que les points de début et de fin aient des valeurs Z différentes, car les valeurs Z sont ignorées. `@g5` est accepté même si l’instance de type **geography** n’est pas valide.  
  
 Les exemples suivants lèvent une `System.FormatException`.  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON((0 5, 0 0, 0 0, 0 0))';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0))';  
```  
  
 `@g1` n'est pas accepté parce que les points de début et de fin n'ont pas la même valeur Y. `@g2` n'est pas accepté car la boucle n'a pas assez de points.  
  
### <a name="valid-instances"></a>Instances valides  
 Pour qu'une instance **CurvePolygon** soit valide, les anneaux extérieur et intérieur doivent répondre aux critères suivants :  
  
1.  Ils peuvent se toucher uniquement à des points de tangentes uniques.  
  
2.  Ils ne peuvent pas se croiser l'un l'autre.  
  
3.  Chaque anneau doit contenir au moins quatre points.  
  
4.  Chaque anneau doit être un type de courbe acceptable.  
  
 Les instances**CurvePolygon** doivent également répondre à des critères spécifiques, selon qu'il s'agisse de types de données **geometry** ou **geography** .  
  
#### <a name="geometry-data-type"></a>Type de données geometry  
 Une instance **geometryCurvePolygon** valide doit avoir les attributs suivants :  
  
1.  Tous les anneaux intérieurs doivent être contenus dans l'anneau extérieur.  
  
2.  Elle peut contenir plusieurs anneaux intérieurs, mais un anneau intérieur ne peut pas contenir un autre anneau intérieur.  
  
3.  Aucun anneau ne peut se croiser lui-même ni croiser un autre anneau.  
  
4.  Les anneaux ne peuvent se toucher qu'à des points de tangentes uniques (nombre de points où le contact des anneaux doit être fini).  
  
5.  L'intérieur du polygone doit être connecté.  
  
 L’exemple suivant montre des instances **geometryCurvePolygon** valides.  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
 Les instances CurvePolygon ont les mêmes règles de validité que les instances Polygon, à l'exception près que les instances CurvePolygon peuvent accepter les nouveaux types de segment d'arc de cercle. Pour obtenir davantage d'exemples d'instances valides ou non valides, consultez [Polygon](../../relational-databases/spatial/polygon.md).  
  
#### <a name="geography-data-type"></a>Type de données geography  
 Une instance **geographyCurvePolygon** valide doit avoir les attributs suivants :  
  
1.  L'intérieur du polygone est connecté à l'aide de la règle gauche.  
  
2.  Aucun anneau ne peut se croiser lui-même ni croiser un autre anneau.  
  
3.  Les anneaux ne peuvent se toucher qu'à des points de tangentes uniques (nombre de points où le contact des anneaux doit être fini).  
  
4.  L'intérieur du polygone doit être connecté.  
  
 L'exemple suivant montre une instance geography CurvePolygon valide.  
  
```  
DECLARE @g geography = 'CURVEPOLYGON((-122.3 47, 122.3 47, 125.7 49, 121 38, -122.3 47))';  
SELECT @g.STIsValid();  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-curvepolygon"></a>A. Instanciation d'une instance geometry avec un CurvePolygon Vide  
 Cet exemple indique comment créer une instance **CurvePolygon** vide :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON EMPTY');  
```  
  
### <a name="b-declaring-and-instantiating-a-geometry-instance-with-a-curvepolygon-in-the-same-statement"></a>B. Déclaration et instanciation d'une instance geometry avec un CurvePolygon dans la même instruction  
 Cet extrait de code indique comment déclarer et initialiser une instance geometry avec un **CurvePolygon** dans la même instruction :  
  
```sql  
DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))'  
```  
  
### <a name="c-instantiating-a-geography-instance-with-a-curvepolygon"></a>C. Instanciation d'une instance geography avec un CurvePolygon  
 Cet extrait de code indique comment déclarer et initialiser une instance **geography** avec un **CurvePolygon**:  
  
```sql  
DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
```  
  
### <a name="d-storing-a-curvepolygon-with-only-an-exterior-bounding-ring"></a>D. Stockage d'un CurvePolygon uniquement avec un anneau englobant extérieur  
 Cet exemple indique comment stocker un cercle simple dans une instance **CurvePolygon** (seul un anneau englobant extérieur est utilisé pour définir le cercle) :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
### <a name="e-storing-a-curvepolygon-containing-interior-rings"></a>E. Stockage d'un CurvePolygon contenant des anneaux intérieurs  
 Cet exemple crée une bouée dans une instance **CurvePolygon** (à la fois un anneau englobant extérieur et un anneau intérieur sont utilisés pour définir la bouée) :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 4, 4 0, 8 4, 4 8, 0 4), CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
 Cet exemple montre à la fois une instance **CurvePolygon** valide et une instance non valide lors de l'utilisation d'anneaux intérieurs :  
  
```sql  
DECLARE @g1 geometry, @g2 geometry;  
SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (-2 2, 2 2, 2 -2, -2 -2, -2 2))');  
IF @g1.STIsValid() = 1  
  BEGIN  
     SELECT @g1.STArea();  
  END  
SET @g2 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (0 5, 5 0, 0 -5, -5 0, 0 5))');  
IF @g2.STIsValid() = 1  
  BEGIN  
     SELECT @g2.STArea();  
  END  
SELECT @g1.STIsValid() AS G1, @g2.STIsValid() AS G2;  
```  
  
 @g1 et @g2 utilisent tous les deux le même anneau englobant extérieur (un cercle avec un rayon de 5) et un carré comme anneau intérieur.  Toutefois, l’instance @g1 est valide, mais l’instance @g2 n’est pas valide.  La raison pour laquelle @g2 n’est pas valide est que l’anneau intérieur fractionne l’espace intérieur englobé par l’anneau extérieur en quatre régions distinctes.  Le dessin suivant montre ce qui s'est produit :  
  
## <a name="see-also"></a> Voir aussi  
 [Polygon](../../relational-databases/spatial/polygon.md)   
 [CircularString](../../relational-databases/spatial/circularstring.md)   
 [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)   
 [Référence de méthodes de type de données geometry](http://msdn.microsoft.com/library/d88e632b-6b2f-4466-a15f-9fbef1a347a7)   
 [Référence de méthodes de type de données geography](http://msdn.microsoft.com/library/028e6137-7128-4c74-90a7-f7bdd2d79f5e)   
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  
