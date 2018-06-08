---
title: STBuffer (type de données geometry) | Microsoft Docs
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
- STBuffer (geometry Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geometry Data Type)
ms.assetid: ca6bf2dc-1d38-4503-b87e-f2ea033d36ba
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f202f87ba6909ea8331ec08b32bf3dd35a252d74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stbuffer-geometry-data-type"></a>STBuffer (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Retourne un objet géométrique qui représente l’union de tous les points dont la distance par rapport à une instance **geometry** est inférieure ou égale à une valeur spécifique.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STBuffer ( distance )  
```  
  
## <a name="arguments"></a>Arguments  
 *distance*  
 Valeur de type **float** (**double** dans le .NET Framework), qui spécifie la distance de l’instance geometry autour de laquelle calculer la mémoire tampon.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Notes   
 `STBuffer()` calcule une mémoire tampon de la même manière que [BufferWithTolerance](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md), en spécifiant *tolerance* = distance \* 0,001, et *relative* = **false**.  
  
 Quand *distance* > 0, une instance **Polygon** ou **MultiPolygon** est retournée.  
  
> [!NOTE]  
>  Dans la mesure où distance est de type **float**, une valeur très petite peut être équivalente à zéro dans les calculs.  Quand cela se produit, une copie de l’instance **geometry** appelante est retournée.  Consultez [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)  
  
 Quand *distance* = 0, une copie de l’instance **geometry** appelante est retournée.  
  
 Quand *distance* < 0, alors  
  
-   une instance **GeometryCollection** vide est retournée quand les dimensions de l’instance sont 0 ou 1.  
  
-   une mémoire tampon négative est retournée lorsque les dimensions de l'instance sont de 2 ou plus.  
  
    > [!NOTE]  
    >  Une mémoire tampon négative peut également créer une instance **GeometryCollection** vide.  
  
 Une mémoire tampon négative supprime tous les points situés dans la distance donnée de la limite de la géométrie.  
  
 L’erreur entre la mémoire tampon théorique et la mémoire tampon calculée correspond à max (tolerance, étendues * 1E-7), où tolerance = distance \* 0,001. Pour plus d’informations sur les étendues, consultez [Référence de méthodes de type de données geometry](http://msdn.microsoft.com/library/d88e632b-6b2f-4466-a15f-9fbef1a347a7).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-stbuffer-with-parametervalue--0-on-one-dimensional-geometry-instance"></a>A. Appel de STBuffer() avec parameter_value < 0 sur une instance géométrique unidimensionnelle  
 L'exemple suivant retourne une instance `GeometryCollection` vide :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="b-calling-stbuffer-with-parametervalue--0-on-a-polygon-instance"></a>B. Appel de STBuffer() avec parameter_value < 0 sur une instance Polygon  
 L'exemple suivant retourne une instance `Polygon` avec une mémoire tampon négative :  
  
```
 DECLARE @g geometry = 'POLYGON((1 1, 1 5, 5 5, 5 1, 1 1))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="c-calling-stbuffer-with-parametervalue--0-on-a-curvepolygon-instance"></a>C. Appel de STBuffer() avec parameter_value < 0 sur une instance CurvePolygon  
 L'exemple suivant retourne une instance `Polygon` avec une mémoire tampon négative d'une instance `CurvePolygon` :  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
> [!NOTE]  
>  Une instance `Polygon` est retournée au lieu d'une instance `CurvePolygon`.  Pour retourner une instance `CurvePolygon`, consultez [BufferWithCurves &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
### <a name="d-calling-stbuffer-with-a-negative-parameter-value-that-returns-an-empty-instance"></a>D. Appel de STBuffer() avec une valeur de paramètre négative qui retourne une instance vide  
 L’exemple suivant montre ce qui se passe quand le paramètre *distance* est égal à -2 pour l’exemple précédent.  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-2).ToString();
 ```  
  
 Cette instruction **SELECT** retourne `GEOMETRYCOLLECTION EMPTY.`  
  
### <a name="e-calling-stbuffer-with-parametervalue--0"></a>E. Appel de STBuffer() avec parameter_value = 0  
 L'exemple suivant retourne une copie de l'instance `geometry` appelante :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(0).ToString();
 ```  
  
### <a name="f-calling-stbuffer-with-a-non-zero-parameter-value-that-is-extremely-small"></a>F. Appel de STBuffer() avec une valeur de paramètre non nulle extrêmement petite  
 L'exemple suivant retourne également une copie de l'instance `geometry` appelante :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.STBuffer(@distance).ToString();
 ```  
  
### <a name="g-calling-stbuffer-with-parametervalue--0"></a>G. Appel de STBuffer() avec parameter_value > 0  
 L'exemple suivant retourne une instance `Polygon` :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(2).ToString();
 ```  
  
### <a name="h-calling-stbuffer-with-a-string-parameter-value"></a>H. Appel de STBuffer() avec une valeur de paramètre de chaîne  
 L'exemple suivant retourne la même instance `Polygon` qu'indiqué précédemment, mais un paramètre de chaîne est passé à la méthode :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('2').ToString();
 ```  
  
 L'exemple suivant génère une erreur :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('a').ToString();
 ```  
  
> [!NOTE]  
>  Les deux exemples précédents ont passé un littéral de chaîne au `STBuffer()`.  Le premier exemple fonctionne car le littéral de chaîne peut être converti en valeur numérique. Toutefois, le deuxième exemple lève un `ArgumentException`.  
  
### <a name="i-calling-stbuffer-on-a-multipoint-instance"></a>I. Appel de STBuffer() sur une instance MultiPoint  
 L'exemple suivant retourne deux instances `MultiPolygon` et une instance `Polygon` :  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.STBuffer(1).ToString(); 
 SELECT @g.STBuffer(1.5).ToString(); 
 SELECT @g.STBuffer(1.6).ToString();
 ```  
  
 Les deux premières instructions **SELECT** retournent une instance `MultiPolygon`, car le paramètre *distance* est inférieur ou égal à 1/2 de la distance entre les deux points (1 1) et (1 4). La troisième instruction **SELECT** retourne une instance `Polygon`, car les instances mises en mémoire tampon des deux points (1 1) et (1 4) se chevauchent.  
  
## <a name="see-also"></a> Voir aussi  
 [BufferWithTolerance &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

