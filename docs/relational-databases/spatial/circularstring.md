---
title: CircularString | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: spatial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-spatial
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9fe06b03-d98c-4337-9f89-54da98f49f9f
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5de99f558b60fb2657542d6c9352bc3b7ce24f66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="circularstring"></a>CircularString
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Un **CircularString** est une collection de zéro ou plusieurs segments d'arc de cercle continus. Un segment d'arc de cercle est un segment courbé défini par trois points dans un plan à deux dimensions ; le premier point doit être différent du troisième point. Si les trois points d'un segment d'arc de cercle sont colinéaires, le segment d'arc est traité comme un segment de ligne.  
  
> [!IMPORTANT]  
>  Pour obtenir une description détaillée et des exemples des nouvelles fonctionnalités spatiales introduites dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], notamment le sous-type **CircularString** , téléchargez le livre blanc intitulé [New Spatial Features in SQL Server 2012](http://go.microsoft.com/fwlink/?LinkId=226407)(Nouvelles fonctionnalités spatiales dans SQL Server 2012).  
  
## <a name="circularstring-instances"></a>Instances CircularString  
 Le dessin suivant montre des instances **CircularString** valides :  
  
 ![5ff17e34-b578-4873-9d33-79500940d0bc](../../relational-databases/spatial/media/5ff17e34-b578-4873-9d33-79500940d0bc.gif)
  
### <a name="accepted-instances"></a>Instances acceptées  
 Une instance **CircularString** est acceptée si elle est vide ou si elle contient un nombre impair de points, n, où n > 1. Les instances **CircularString** suivantes sont acceptées.  
  
```  
DECLARE @g1 geometry = 'CIRCULARSTRING EMPTY';  
DECLARE @g2 geometry = 'CIRCULARSTRING(1 1, 2 0, -1 1)';  
DECLARE @g3 geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 2 0, 1 1)';  
```  
  
 `@g3` montre que l’instance **CircularString** peut être acceptée, mais pas valide. La déclaration suivante d'instance de CircularString n'est pas acceptée. Cette déclaration lève une `System.FormatException`.  
  
```  
DECLARE @g geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 1 1)';  
```  
  
### <a name="valid-instances"></a>Instances valides  
 Une instance **CircularString** valide doit être vide ou avoir les attributs suivants :  
  
-   Elle doit contenir au moins un segment d'arc de cercle (autrement dit, voir un minimum de trois points).  
  
-   Le dernier point de terminaison de chaque segment d'arc de cercle de la séquence, à l'exception du dernier segment, doit correspondre au premier point de terminaison du segment suivant dans la séquence.  
  
-   Elle doit comporter un nombre impair de points.  
  
-   Elle ne peut pas se chevaucher sur un intervalle.  
  
-   Bien que les instances **CircularString** puissent contenir des segments de ligne, ces segments de ligne doivent être définis par trois points colinéaires.  
  
 L'exemple suivant montre des instances **CircularString** valides.  
  
```  
DECLARE @g1 geometry = 'CIRCULARSTRING EMPTY';  
DECLARE @g2 geometry = 'CIRCULARSTRING(1 1, 2 0, -1 1)';  
DECLARE @g3 geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 1 1, 0 1)';  
DECLARE @g4 geometry = 'CIRCULARSTRING(1 1, 2 2, 2 2)';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(),@g4.STIsValid();  
```  
  
 Une instance **CircularString** doit contenir au moins deux segments d'arc de cercle pour définir un cercle complet. Une instance **CircularString** ne peut pas utiliser un seul segment d’arc de cercle (tel que (1 1, 3 1, 1 1)) pour définir un cercle complet. Utilisez (1 1, 2 2, 3 1, 2 0, 1 1) pour définir le cercle.  
  
 L'exemple suivant montre des instances CircularString qui ne sont pas valides.  
  
```  
DECLARE @g1 geometry = 'CIRCULARSTRING(1 1, 2 0, 1 1)';  
DECLARE @g2 geometry = 'CIRCULARSTRING(0 0, 0 0, 0 0)';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
### <a name="instances-with-collinear-points"></a>Instances avec des points colinéaires  
 Dans les cas suivants un segment d'arc de cercle sera traité comme un segment de ligne :  
  
-   lorsque les trois points sont colinéaires (par exemple, (1 3, 4 4, 7 5)) ;  
  
-   lorsque le premier point et le point central sont les mêmes, mais que le troisième point est différent (par exemple, (1 3, 1 3, 7 5)) ;  
  
-   lorsque le point central et le dernier point sont les mêmes, mais que le premier point est différent (par exemple, (1 3, 4 4, 4 4)).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-circularstring"></a>A. Instanciation d'une instance géométrique à l'aide d'un CircularString vide  
 Cet exemple indique comment créer une instance **CircularString** vide :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CIRCULARSTRING EMPTY');  
```  
  
### <a name="b-instantiating-a-geometry-instance-using-a-circularstring-with-one-circular-arc-segment"></a>B. Instanciation d'une instance géométrique à l'aide d'un CircularString avec un segment d'arc de cercle  
 L’exemple suivant montre comment créer une instance **CircularString** avec un segment d’arc de cercle unique (demi-cercle) :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry:: STGeomFromText('CIRCULARSTRING(2 0, 1 1, 0 0)', 0);  
SELECT @g.ToString();  
```  
  
### <a name="c-instantiating-a-geometry-instance-using-a-circularstring-with-multiple-circular-arc-segments"></a>C. Instanciation d'une instance géométrique à l'aide d'un CircularString avec plusieurs segments d'arc de cercle  
 L’exemple suivant montre comment créer une instance **CircularString** avec plusieurs segments d’arc de cercle (cercle complet) :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CIRCULARSTRING(2 1, 1 2, 0 1, 1 0, 2 1)');  
SELECT 'Circumference = ' + CAST(@g.STLength() AS NVARCHAR(10));    
```  
  
 Ce code produit la sortie suivante :  
  
```  
Circumference = 6.28319  
```  
  
 Comparez la sortie lorsque **LineString** est utilisé à la place de **CircularString**:  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(2 1, 1 2, 0 1, 1 0, 2 1)', 0);  
SELECT 'Perimeter = ' + CAST(@g.STLength() AS NVARCHAR(10));  
```  
  
 Ce code produit la sortie suivante :  
  
```  
Perimeter = 5.65685  
```  
  
 Remarquez que la valeur de l'exemple de **CircularString** est proche de 2∏, ce qui correspond à la circonférence réelle du cercle.  
  
### <a name="d-declaring-and-instantiating-a-geometry-instance-with-a-circularstring-in-the-same-statement"></a>D. Déclaration et instanciation d'une instance géométrique avec un CircularString dans la même instruction  
 Cet extrait de code indique comment déclarer et instancier une instance **geometry** avec un **CircularString** dans la même instruction :  
  
```sql  
DECLARE @g geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
```  
  
### <a name="e-instantiating-a-geography-instance-with-a-circularstring"></a>E. Instanciation d'une instance géographique avec un CircularString  
 L'exemple suivant indique comment déclarer et instancier une instance **geography** avec un **CircularString**:  
  
```sql  
DECLARE @g geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
```  
  
### <a name="f-instantiating-a-geometry-instance-with-a-circularstring-that-is-a-straight-line"></a>F. Instanciation d'une instance géométrique avec un CircularString qui est une ligne droite  
 L'exemple suivant indique comment créer une instance **CircularString** qui est une ligne droite :  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('CIRCULARSTRING(0 0, 1 2, 2 4)', 0);  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)   
 [MakeValid &#40;type de données geography&#41;](../../t-sql/spatial-geography/makevalid-geography-data-type.md)   
 [MakeValid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)   
 [STIsValid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [STIsValid &#40;type de données geography&#41;](../../t-sql/spatial-geography/stisvalid-geography-data-type.md)   
 [STLength &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STStartPoint &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)   
 [STEndPoint &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)   
 [STPointN &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)   
 [STNumPoints &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STIsRing &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)   
 [STIsClosed &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STPointOnSurface &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [LineString](../../relational-databases/spatial/linestring.md)  
  
  
