---
title: Créer, construire et interroger des instances geometry | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: spatial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-spatial
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- planar spatial data [SQL Server], getting started
- geometry data type [SQL Server], getting started
ms.assetid: c6b5c852-37d2-48d0-a8ad-e43bb80d6514
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 236a8f0c292e6bc491d84ae4532509e345713b37
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-construct-and-query-geometry-instances"></a>Créer, construire et interroger des instances geometry
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le type de données spatiales planaire, **geometry**, représente les données dans un système de coordonnées euclidien (plat). Ce type est implémenté en tant que type de données CLR (Common Language Runtime) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le type **geometry** est prédéfini et disponible dans chaque base de données. Vous pouvez créer des colonnes de table de type **geometry** et opérer sur les données **geometry** comme vous le feriez avec d'autres types CLR.  
  
 Le type de données **geometry** (planaire) pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est conforme à la spécification Open Geospatial Consortium (OGC) Simple Features for SQL version 1.1.0.  
  
 Pour plus d'informations sur les spécifications OGC, reportez-vous aux sites Web suivants :  
  
-   [OGC Specifications, Simple Feature Access Part 1 - Common Architecture](http://go.microsoft.com/fwlink/?LinkId=93628)  
  
-   [OGC Specifications, Simple Feature Access Part 2 – SQL Options (en anglais)](http://go.microsoft.com/fwlink/?LinkId=93629)  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un sous-ensemble de la norme GML 3.1, qui est définie dans le schéma suivant : [http://schemas.microsoft.com/sqlserver/profiles/gml/SpatialGML.xsd](http://go.microsoft.com/fwlink/?LinkId=230959).  
  
##  <a name="creating"></a> Création ou construction d'une nouvelle instance geometry  
  
###  <a name="existing"></a> Création d'une instance geometry à partir d'une instance existante  
 Le type de données **geometry** fournit de nombreuses méthodes intégrées que vous pouvez utiliser pour créer des instances **geometry** basées sur des instances existantes.  
  
 **Pour créer une mémoire tampon autour d'une géométrie**  
 [STBuffer &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)  
  
 [BufferWithTolerance &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)  
  
 **Pour créer une version simplifiée d'une géométrie**  
 [Reduce &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/reduce-geometry-data-type.md)  
  
 **Pour créer la forme convexe d'une géométrie**  
 [STConvexHull &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stconvexhull-geometry-data-type.md)  
  
 **Pour créer une géométrie à partir de l'intersection de deux géométries**  
 [STIntersection &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stintersection-geometry-data-type.md)  
  
 **Pour créer une géométrie à partir de l'union de deux géométries**  
 [STUnion &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stunion-geometry-data-type.md)  
  
 **Pour créer une géométrie à partir des points où une géométrie n'en chevauche pas une autre**  
 [STDifference &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stdifference-geometry-data-type.md)  
  
 **Pour créer une géométrie à partir des points où deux géométries ne se chevauchent pas**  
 [STSymDifference &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stsymdifference-geometry-data-type.md)  
  
 **Pour créer une instance Point arbitraire qui repose sur une géométrie existante**  
 [STPointOnSurface &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
  
###  <a name="wkt"></a> Construction d'une instance geometry à partir d'une entrée WKT (Well-Known Text)  
 Le type de données **geometry** fournit plusieurs méthodes intégrées qui génèrent une géométrie à partir de la représentation WKT OGC (Open Geospatial Consortium). La norme WKT est une chaîne de texte qui autorise l'échange de données géométriques sous forme textuelle.  
  
 **Pour construire tout type d'instance geometry à partir d'une entrée WKT**  
 [STGeomFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeomfromtext-geometry-data-type.md)  
  
 [Parse &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/parse-geometry-data-type.md)  
  
 **Pour construire une instance Point géométrique à partir d'entrée WKT**  
 [STPointFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointfromtext-geometry-data-type.md)  
  
 **Pour construire une instance MultiPoint geometry à partir d'une entrée WKT**  
 [STMPointFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmpointfromtext-geometry-data-type.md)  
  
 **Pour construire une instance LineString geometry à partir d'une entrée WKT**  
 [STLineFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stlinefromtext-geometry-data-type.md)  
  
 **Pour construire une instance MultiLineString geometry à partir d'une entrée WKT**  
 [STMLineFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmlinefromtext-geometry-data-type.md)  
  
 **Pour construire une instance Polygon geometry à partir d'une entrée WKT**  
 [STPolyFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpolyfromtext-geometry-data-type.md)  
  
 **Pour construire une instance MultiPolygon geometry à partir d'une entrée WKT**  
 [STMPolyFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmpolyfromtext-geometry-data-type.md)  
  
 **Pour construire une instance GeometryCollection geometry à partir d'une entrée WKT**  
 [STGeomCollFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeomcollfromtext-geometry-data-type.md)  
  
  
###  <a name="wkb"></a> Construction d'une instance geometry à partir d'une entrée WKB (Well-Known Binary)  
 WKB est un format binaire spécifié par l’OGC (Open Geospatial Consortium) qui autorise l’échange de données **geometry** entre une application cliente et une base de données SQL. Les fonctions suivantes acceptent l'entrée WKB pour construire des géométries :  
  
 **Pour construire tout type d'instance geometry à partir d'une entrée WKB**  
 [STGeomFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeomfromwkb-geometry-data-type.md)  
  
 **Pour construire une instance Point geometry à partir d'une entrée WKB**  
 [STPointFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointfromwkb-geometry-data-type.md)  
  
 **Pour construire une instance MultiPoint geometry à partir d'une entrée WKB**  
 [STMPointFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmpointfromwkb-geometry-data-type.md)  
  
 **Pour construire une instance LineString geometry à partir d'une entrée WKB**  
 [STLineFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stlinefromwkb-geometry-data-type.md)  
  
 **Pour construire une instance MultiLineString geometry à partir d'une entrée WKB**  
 [STMLineFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmlinefromwkb-geometry-data-type.md)  
  
 **Pour construire une instance Polygon geometry à partir d'une entrée WKB**  
 [STPolyFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpolyfromwkb-geometry-data-type.md)  
  
 **Pour construire une instance MultiPolygon geometry à partir d'une entrée WKB**  
 [STMPolyFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stmpolyfromwkb-geometry-data-type.md)  
  
 **Pour construire une instance GeometryCollection geometry à partir d'une entrée WKB**  
 [STGeomCollFromWKB &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeomcollfromwkb-geometry-data-type.md)  
  
  
###  <a name="gml"></a> Construction d'une instance geometry à partir d'une entrée texte GML  
 Le type de données **geometry** fournit une méthode qui génère une instance **geometry** à partir de GML, représentation XML d’objets géométriques. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un sous-ensemble de GML.  
  
 **Pour construire tout type d'instance geometry à partir d'une entrée GML**  
 [GeomFromGml &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/geomfromgml-geometry-data-type.md)  
  
  
##  <a name="returning"></a> Renvoi de données WKT et WKB à partir d'une instance geometry  
 Vous pouvez utiliser les méthodes suivantes pour retourner le format WKT ou WKB d'une instance **geometry** :  
  
 **Pour retourner la représentation WKT d'une instance geometry**  
 [STAsText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stastext-geometry-data-type.md)  
  
 [ToString &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/tostring-geometry-data-type.md)  
  
 **Pour retourner la représentation WKT d'une instance geometry incluant des valeurs Z et M**  
 [AsTextZM &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/astextzm-geometry-data-type.md)  
  
 **Pour retourner la représentation WKB d'une instance geometry**  
 [STAsBinary &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stasbinary-geometry-data-type.md)  
  
 **Pour retourner une représentation GML d'une instance geometry**  
 [AsGml &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/asgml-geometry-data-type.md)  
  
  
##  <a name="querying"></a> Interrogation des propriétés et comportements des instances geometry  
 Toutes les instances **geometry** ont plusieurs propriétés qui peuvent être extraites par le biais des méthodes fournies par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les rubriques suivantes définissent les propriétés et comportements de types geometry et les méthodes permettant de les interroger.  
  
###  <a name="valid"></a> Informations sur la validité, le type d'instance et GeometryCollection  
 Une fois qu’une instance **geometry** est construite, vous pouvez utiliser les méthodes suivantes pour déterminer si elle est formée correctement, retourner le type d’instance ou, s’il s’agit d’une instance de collection, retourner une instance **geometry** spécifique.  
  
 **Pour retourner le type d'instance d'une géométrie**  
 [STGeometryType &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
 **Pour déterminer si une géométrie est un type d'instance donné**  
 [InstanceOf &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/instanceof-geometry-data-type.md)  
  
 **Pour déterminer si une instance geometry est de forme correcte pour son type d'instance**  
 [STIsValid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)  
  
 **Pour convertir une instance geometry en une instance geometry de forme correcte avec un type d'instance**  
 [MakeValid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)  
  
 **Pour retourner le nombre de géométries dans une instance de collection geometry**  
 [STNumGeometries &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumgeometries-geometry-data-type.md)  
  
 Pour retourner une géométrie spécifique dans une instance de collection géométrique  
 [STGeometryN &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeometryn-geometry-data-type.md)STGeometryN (type de données geometry)  
  
  
###  <a name="number"></a> Nombre de points  
 Toutes les instances **geometry** non vides sont constituées de *points*. Ces points représentent les coordonnées X et Y de latitude et de longitude du plan sur lequel les géométries sont dessinées. **geometry** fournit de nombreuses méthodes intégrées pour interroger les points d’une instance.  
  
 **Pour retourner le nombre de points qui composent une instance**  
 [STNumPoints &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)  
  
 **Pour retourner un point spécifique dans une instance**  
 [STPointN](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **Pour retourner un point arbitraire qui repose sur une instance**  
 [STPointOnSurface](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
 **Pour retourner le point de départ d'une instance**  
 [STStartPoint](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)  
  
 **Pour retourner le point de terminaison d'une instance**  
 [STEndpoint](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)  
  
 **Pour retourner la coordonnée X d'une instance Point**  
 [STX &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)  
  
 **Pour retourner la coordonnée Y d'une instance Point**  
 [STY](../../t-sql/spatial-geometry/sty-geometry-data-type.md)  
  
 **Pour retourner le point central géométrique d'une instance Polygone, CurvePolygon ou MultiPolygon**  
 [STCentroid](../../t-sql/spatial-geometry/stcentroid-geometry-data-type.md)  
  
  
###  <a name="dimension"></a> Dimension  
 Une instance **geometry** non vide peut avoir 0, 1 ou 2 dimensions. Les instances **geometry**à zéro dimension, telles que **Point** et **MultiPoint**, n’ont aucune longueur ou surface. Les objets unidimensionnels, tels que **LineString, CircularString, CompoundCurve**et **MultiLineString**ont une longueur. Les instances à deux dimensions, telles que **Polygon**, **CurvePolygon**et **MultiPolygon**, ont une surface et une longueur. Les instances vides indiquent une dimension de -1 et une instance **GeometryCollection** indique une surface en fonction des types de son contenu.  
  
 **Pour retourner la dimension d'une instance**  
 [STDimension](../../t-sql/spatial-geometry/stdimension-geometry-data-type.md)  
  
 **Pour retourner la longueur d'une instance**  
 [STLength](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)  
  
 **Pour retourner la surface d'une instance**  
 [STArea](../../t-sql/spatial-geometry/starea-geometry-data-type.md)  
  
  
###  <a name="empty"></a> Vide  
 Une instance *vide***geometry** n’a aucun point. La longueur des instances **LineString, CircularString**, **CompoundCurve**et **MultiLineString** vides est nulle. La surface des instances **Polygon**, **CurvePolygon**et **MultiPolygon** vides est 0.  
  
 **Pour déterminer si une instance est vide**  
 [STIsEmpty](../../t-sql/spatial-geometry/stisempty-geometry-data-type.md).  
  
  
###  <a name="simple"></a> Simple  
 Pour qu’une **géometrie** de l’instance soit *simple*, elle doit remplir ces deux conditions :  
  
-   Chaque graphique de l'instance ne doit pas se croiser lui-même, sauf à ses points de terminaison.  
  
-   Deux graphiques de l'instance ne peuvent se croiser l'un l'autre à un point qui n'est pas dans leurs limites.  
  
> [!NOTE]  
>  Les géométries vides sont toujours simples.  
  
 **Pour déterminer si une instance est simple**  
 [STIsSimple](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md).  
  
  
###  <a name="boundary"></a> Limite, intérieur et extérieur  
 L’ *intérieur* d’une instance **geometry** est l’espace occupé par l’instance et l’ *extérieur* est l’espace qu’elle n’occupe pas.  
  
 Une limite (*Boudary* à est définie par l’OGC comme suit :  
  
-   Les instances**Point** et **MultiPoint** n'ont pas de limite.  
  
-   Les limites de**LineString** et **MultiLineString** boundaries are formed by the start points et end points, removing those that occur an even number of times.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 1, 0 0, 1 0, 0 1), (1 1, 1 0))');  
SELECT @g.STBoundary().ToString();  
```  
  
 La limite d'une instance **Polygon** ou **MultiPolygon** est l'ensemble de ses anneaux.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0), (1 1, 1 2, 2 2, 2 1, 1 1))');  
SELECT @g.STBoundary().ToString();  
```  
  
 **Pour retourner la limite d'une instance**  
 [STBoundary](../../t-sql/spatial-geometry/stboundary-geometry-data-type.md)  
  
  
###  <a name="envelope"></a> Enveloppe  
 L’ *enveloppe* d’une instance **geometry** , aussi appelée *cadre englobant*, est le rectangle aligné sur l’axe formé par les coordonnées minimales et maximales (X, Y) de l’instance.  
  
 **Pour retourner l'enveloppe d'une instance**  
 [STEnvelope](../../t-sql/spatial-geometry/stenvelope-geometry-data-type.md)  
  
  
###  <a name="closure"></a> Fermeture  
 Une instance *fermée***geometry** est un graphique dont les points de début et de fin sont identiques. Les instances**Polygon** sont considérées comme fermées. Les instances**Point** ne sont pas fermées.  
  
 Un anneau est une instance **LineString** simple et fermée.  
  
 **Pour déterminer si une instance est fermée**  
 [STIsClosed](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)  
  
 **Pour déterminer si une instance est un anneau**  
 [STIsRing](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)  
  
 **Pour retourner l'anneau extérieur d'une instance Polygon**  
 [STExteriorRing](../../t-sql/spatial-geometry/stexteriorring-geometry-data-type.md)  
  
 **Pour retourner le nombre d'anneaux intérieurs dans un Polygon**  
 [STNumInteriorRing](../../t-sql/spatial-geometry/stnuminteriorring-geometry-data-type.md)  
  
 **Pour retourner un anneau intérieur spécifié d'un Polygon**  
 [STInteriorRingN](../../t-sql/spatial-geometry/stinteriorringn-geometry-data-type.md)  
  
  
###  <a name="srid"></a> ID de référence spatial (SRID)  
 L’ID de référence spatial (SRID) est un identificateur spécifiant dans quel système de coordonnées l’instance **geometry** est représentée. Deux instances avec différents SRID ne peuvent pas être comparées.  
  
 **Pour définir ou retourner le SRID d'une instance**  
 [STSrid](../../t-sql/spatial-geometry/stsrid-geometry-data-type.md)  
  
 Cette propriété peut être modifiée.  
  
  
##  <a name="rel"></a> Détermination de relations entre des instances geometry  
 Le type de données **geometry** fournit de nombreuses méthodes intégrées que vous pouvez utiliser pour déterminer les relations entre deux instances **geometry** .  
  
 **Pour déterminer si deux instances comprennent le même ensemble de points**  
 [STEquals](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **Pour déterminer si deux instances sont disjointes**  
 [STDisjoint](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **Pour déterminer si deux instances se croisent**  
 [STIntersects](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **Pour déterminer si deux instances se touchent**  
 [STTouches](../../t-sql/spatial-geometry/sttouches-geometry-data-type.md)  
  
 **Pour déterminer si deux instances se chevauchent**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **Pour déterminer si deux instances se croisent**  
 [STCrosses](../../t-sql/spatial-geometry/stcrosses-geometry-data-type.md)  
  
 **Pour déterminer si une instance est dans une autre instance**  
 [STWithin](../../t-sql/spatial-geometry/stwithin-geometry-data-type.md)  
  
 **Pour déterminer si une instance en contient une autre**  
 [STContains](../../t-sql/spatial-geometry/stcontains-geometry-data-type.md)  
  
 **Pour déterminer si une instance en chevauche une autre**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **Pour déterminer si deux instances sont liées spatialement**  
 [STRelate](../../t-sql/spatial-geometry/strelate-geometry-data-type.md)  
  
 **Pour déterminer la distance la plus courte entre des points dans deux géométries**  
 [STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
  
##  <a name="defaultsrid"></a> Les instances geometry ont un SRID par défaut de zéro  
 Le SRID par défaut pour les instances **geometry** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est 0. Avec les données spatiales **geometry** , le SRID spécifique de l'instance spatiale n'est pas requis pour effectuer des calculs ; par conséquent, les instances peuvent résider dans un espace planaire indéfini. Pour indiquer un espace planaire indéfini dans les calculs des méthodes du type de données **geometry** , le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilise SRID 0.  
  
##  <a name="examples"></a> Exemples  
 Les deux exemples suivants montrent comment ajouter et interroger des données géométriques.  
  
-   Le premier exemple crée une table avec une colonne d'identité et une colonne `geometry` `GeomCol1`. Une troisième colonne restitue la colonne `geometry` dans sa représentation OGC (Open Geospatial Consortium) WKT (Well-Known Text) et utilise la méthode `STAsText()` . Deux lignes sont ensuite insérées : une ligne contient une instance `LineString` de `geometry`et une ligne contient une instance `Polygon` .  
  
    ```  
    IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
        DROP TABLE dbo.SpatialTable;  
    GO  
  
    CREATE TABLE SpatialTable   
        ( id int IDENTITY (1,1),  
        GeomCol1 geometry,   
        GeomCol2 AS GeomCol1.STAsText() );  
    GO  
  
    INSERT INTO SpatialTable (GeomCol1)  
    VALUES (geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0));  
  
    INSERT INTO SpatialTable (GeomCol1)  
    VALUES (geometry::STGeomFromText('POLYGON ((0 0, 150 0, 150 150, 0 150, 0 0))', 0));  
    GO  
    ```  
  
-   Le deuxième exemple utilise la méthode `STIntersection()` pour retourner les points où les deux instances `geometry` précédemment insérées se croisent.  
  
    ```  
    DECLARE @geom1 geometry;  
    DECLARE @geom2 geometry;  
    DECLARE @result geometry;  
  
    SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
    SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
    SELECT @result = @geom1.STIntersection(@geom2);  
    SELECT @result.STAsText();  
    ```  
  
  
## <a name="see-also"></a> Voir aussi  
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
