---
title: Créer, construire et interroger des instances geography | Microsoft Docs
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
- geography data type [SQL Server]
- geodetic data type [SQL Server]
- geography data type [SQL Server], about geography data type
ms.assetid: b585851e-d15b-411f-adeb-aeabeb777c0b
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1263d38ff8f3693ab42129975ea584960f557d63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-construct-and-query-geography-instances"></a>Créer, construire et interroger des instances geography
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le type de données spatiales géographiques, **geography**, représente des données dans un système de coordonnées de monde sphérique. Ce type est implémenté en tant que type de données CLR (Common Language Runtime) .NET dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **geography** stocke des données ellipsoïdes, telles que des coordonnées de latitude et de longitude GPS.  
  
 Le type **geography** est prédéfini et disponible dans chaque base de données. Vous pouvez créer des colonnes de table de type **geography** et opérer sur les données **geography** comme vous le feriez avec d’autres types fournis par le système.  
  
##  <a name="creating"></a> Création ou construction d'une nouvelle instance geography  
  
###  <a name="existing"></a> Création d'une nouvelle instance geography à partir d'une instance existante  
 Le type de données **geography** fournit de nombreuses méthodes intégrées que vous pouvez utiliser pour créer des instances **geography** basées sur des instances existantes.  
  
 **Pour créer une mémoire tampon autour d'une géographie**  
 [STBuffer &#40;type de données geography&#41;](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)  
  
 **Pour créer une mémoire tampon autour d'une géographie, en tenant compte d'une tolérance**  
 [BufferWithTolerance &#40;type de données geography&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)  
  
 **Pour créer une géographie à partir de l'intersection de deux instances géographiques**  
 [STIntersection &#40;type de données geography&#41;](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Pour créer une géographie à partir de l'union de deux instances géographiques**  
 [STUnion &#40;type de données geography&#41;](../../t-sql/spatial-geography/stunion-geography-data-type.md)  
  
 **Pour créer une géographie à partir des points où une géographie n'en chevauche pas une autre**  
 [STDifference &#40;type de données geography&#41;](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
###  <a name="wkt"></a> Construction d'une instance geography à partir d'une entrée WKT (Well-Known Text)  
 Le type de données **geography** fournit plusieurs méthodes intégrées qui génèrent une géographie à partir de la représentation WKT OGC (Open Geospatial Consortium). La norme WKT est une chaîne de texte qui autorise l'échange de données geography sous forme textuelle.  
  
 **Pour construire tout type d'instance geography à partir d'une entrée WKT**  
 [STGeomFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
 [Parse &#40;type de données geography&#41;](../../t-sql/spatial-geography/parse-geography-data-type.md)  
  
 **Pour construire une instance Point geography à partir d'une entrée WKT**  
 [STPointFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stpointfromtext-geography-data-type.md)  
  
 **Pour construire une instance MultiPoint geography à partir d'une entrée WKT**  
 [STMPointFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmpointfromtext-geography-data-type.md)  
  
 **Pour construire une instance LineString geography à partir d'une entrée WKT**  
 STLineFromText (type de données geography)  
  
 **Pour construire une instance MultiLineString geography à partir d'une entrée WKT**  
 [STMLineFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmlinefromtext-geography-data-type.md)  
  
 **Pour construire une instance Polygon geography à partir d'une entrée WKT**  
 [STPolyFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stpolyfromtext-geography-data-type.md)  
  
 **Pour construire une instance MultiPolygon geography à partir d'une entrée WKT**  
 [STMPolyFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmpolyfromtext-geography-data-type.md)  
  
 **Pour construire une instance GeometryCollection geography à partir d'une entrée WKT**  
 [STGeomCollFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeomcollfromtext-geography-data-type.md)  
  
###  <a name="wkb"></a> Construction d'une instance geography à partir d'une entrée WKB (Well-Known Binary)  
 WKB est un format binaire spécifié par l’OGC qui autorise l’échange de données **Geography** entre une application cliente et une base de données SQL. Les fonctions suivantes acceptent l'entrée WKB pour construire des instances geography :  
  
 **Pour construire tout type d'instance geography à partir d'une entrée WKB**  
 [STGeomFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeomfromwkb-geography-data-type.md)  
  
 **Pour construire une instance Point geography à partir d'une entrée WKB**  
 [STPointFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stpointfromwkb-geography-data-type.md)  
  
 **Pour construire une instance MultiPoint geography à partir d'une entrée WKB**  
 [STMPointFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmpointfromwkb-geography-data-type.md)  
  
 **Pour construire une instance LineString geography à partir d'une entrée WKB**  
 [STLineFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stlinefromwkb-geography-data-type.md)  
  
 **Pour construire une instance MultiLineString geography à partir d'une entrée WKB**  
 [STMLineFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmlinefromwkb-geography-data-type.md)  
  
 **Pour construire une instance Polygon geography à partir d'une entrée WKB**  
 [STPolyFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stpolyfromwkb-geography-data-type.md)  
  
 **Pour construire une instance MultiPolygon geography à partir d'une entrée WKB**  
 [STMPolyFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stmpolyfromwkb-geography-data-type.md)  
  
 **Pour construire une instance GeometryCollection geography à partir d'une entrée WKB**  
 [STGeomCollFromWKB &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeomcollfromwkb-geography-data-type.md)STGeomCollFromWKB (type de données geography)  
  
###  <a name="gml"></a> Construction d'une instance geography à partir d'une entrée texte GML  
 Le type de données **geography** fournit une méthode qui génère une instance **geography** à partir de GML, représentation XML d’une instance **geography** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un sous-ensemble de GML.  
  
 Pour plus d’informations sur le langage GML (Geography Markup Language), consultez la spécification OGC : [OGC Specifications, Geography Markup Language (en anglais)](http://go.microsoft.com/fwlink/?LinkId=93629).  
  
 **Pour construire tout type d'instance geography à partir d'une entrée GML**  
 [GeomFromGML &#40;type de données geography&#41;](../../t-sql/spatial-geography/geomfromgml-geography-data-type.md)  
  
##  <a name="returning"></a> Renvoi de données WKT et WKB à partir d'une instance geography  
 Vous pouvez utiliser les méthodes suivantes pour retourner le format WKT ou WKB d’une instance **geography** :  
  
 **Pour retourner la représentation WKT d'une instance geography**  
 [STAsText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stastext-geography-data-type.md)  
  
 [ToString &#40;type de données geography&#41;](../../t-sql/spatial-geography/tostring-geography-data-type.md)  
  
 **Pour retourner la représentation WKT d'une instance geography incluant des valeurs Z et M**  
 [AsTextZM &#40;type de données geography&#41;](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
 **Pour retourner la représentation WKB d'une instance geography**  
 [STAsBinary &#40;type de données geography&#41;](../../t-sql/spatial-geography/stasbinary-geography-data-type.md)  
  
 **Pour retourner une représentation GML d'une instance geography**  
 [AsGml &#40;type de données geography&#41;](../../t-sql/spatial-geography/asgml-geography-data-type.md)  
  
##  <a name="query"></a> Interrogation des propriétés et des comportements des instances geography  
 Toutes les instances **geography** ont plusieurs propriétés qui peuvent être extraites par le biais des méthodes fournies par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les rubriques suivantes définissent les propriétés et comportements de types géographiques et les méthodes permettant de les interroger.  
  
###  <a name="valid"></a> Informations sur la validité, le type d'instance et GeometryCollection  
 Après avoir construit une instance **geography** , vous pouvez utiliser les méthodes suivantes pour retourner le type d’instance ou, dans le cas d’une instance **GeometryCollection** , pour retourner une instance **geography** spécifique.  
  
 **Pour retourner le type d'instance d'une géographie**  
 [STGeometryType &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md)  
  
 **Pour déterminer si une géographie est un type d'instance donné**  
 [InstanceOf &#40;type de données geography&#41;](../../t-sql/spatial-geography/instanceof-geography-data-type.md)  
  
 **Pour déterminer si une instance géographique est de forme correcte pour son type d'instance**  
 [STNumGeometries &#40;type de données geography&#41;](../../t-sql/spatial-geography/stnumgeometries-geography-data-type.md)  
  
 **Pour retourner une géographie spécifique dans une instance GeometryCollection**  
 [STGeometryN &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeometryn-geography-data-type.md)STGeometryN (type de données geography)  
  
###  <a name="number"></a> Nombre de points  
 Toutes les instances **geography** non vides sont constituées de *points*. Ces points représentent les coordonnées de latitude et de longitude du monde sur lequel les instances **geography** sont dessinées. Le type de données **geography** fournit de nombreuses méthodes intégrées pour interroger les points d’une instance.  
  
 **Pour retourner le nombre de points qui composent une instance**  
 [STNumPoints &#40;type de données geography&#41;](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)  
  
 **Pour retourner un point spécifique dans une instance**  
 [STPointN &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **Pour retourner le point de départ d'une instance**  
 [STStartPoint &#40;type de données geography&#41;](../../t-sql/spatial-geography/ststartpoint-geography-data-type.md)  
  
 **Pour retourner le point de terminaison d'une instance**  
 [STEndpoint &#40;type de données geography&#41;](../../t-sql/spatial-geography/stendpoint-geography-data-type.md)  
  
###  <a name="dimension"></a> Dimension  
 Une instance **geography** non vide peut avoir 0, 1 ou 2 dimensions. Les instances **geography** à zéro dimension, telles que **Point** et **MultiPoint**, n’ont aucune longueur ou surface. Les objets unidimensionnels, tels que **LineString, CircularString**, **CompoundCurve**et **MultiLineString**ont une longueur. Les instances à deux dimensions, telles que **Polygon, CurvePolygon**et **MultiPolygon**, ont une surface et une longueur. Les instances vides indiquent une dimension de -1 et une instance **GeometryCollection** indique la dimension maximale de son contenu.  
  
 **Pour retourner la dimension d'une instance**  
 [STDimension &#40;type de données geography&#41;](../../t-sql/spatial-geography/stdimension-geography-data-type.md)  
  
 **Pour retourner la longueur d'une instance**  
 [STLength &#40;type de données geography&#41;](../../t-sql/spatial-geography/stlength-geography-data-type.md)  
  
 **Pour retourner la surface d'une instance**  
 [STArea &#40;type de données geography&#41;](../../t-sql/spatial-geography/starea-geography-data-type.md)  
  
###  <a name="empty"></a> Vide  
 Une instance *vide***geography** n’a aucun point. La longueur des instances **LineString, CircularString**, **CompoundCurve**et **MultiLineString** vides est nulle. La surface des instances **Polygon, CurvePolygon** et **MultiPolygon** vides est nulle.  
  
 **Pour déterminer si une instance est vide**  
 [STIsEmpty &#40;type de données geography&#41;](../../t-sql/spatial-geography/stisempty-geography-data-type.md)  
  
###  <a name="closure"></a> Fermeture  
 Une instance *fermée***geography** est un graphique dont les points de début et de fin sont identiques. Les instances**Polygon** sont considérées comme fermées. Les instances**Point** ne sont pas fermées.  
  
 Un anneau est une instance **LineString** simple et fermée.  
  
 **Pour déterminer si une instance est fermée**  
 [STIsClosed &#40;type de données geography&#41;](../../t-sql/spatial-geography/stisclosed-geography-data-type.md)  
  
 **Pour retourner le nombre d'anneaux dans une instance Polygon**  
 [NumRings &#40;type de données geography&#41;](../../t-sql/spatial-geography/numrings-geography-data-type.md)  
  
 **Pour retourner un anneau spécifié d'une instance géographique**  
 [RingN &#40;type de données geography&#41;](../../t-sql/spatial-geography/ringn-geography-data-type.md)  
  
###  <a name="srid"></a> ID de référence spatial (SRID)  
 L’ID de référence spatial (SRID) est un identificateur spécifiant dans quel système de coordonnées ellipsoïde l’instance **geography** est représentée. Deux instances **geography** avec différents SRID ne peuvent pas être comparées.  
  
 **Pour définir ou retourner le SRID d'une instance**  
 [STSrid &#40;type de données geography&#41;](../../t-sql/spatial-geography/stsrid-geography-data-type.md)  
  
 Cette propriété peut être modifiée.  
  
##  <a name="rel"></a> Détermination de relations entre des instances geography  
 Le type de données **geography** fournit de nombreuses méthodes intégrées que vous pouvez utiliser pour déterminer les relations entre deux instances **geography** .  
  
 **Pour déterminer si deux instances comprennent le même ensemble de points**  
 [STEquals &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **Pour déterminer si deux instances sont disjointes**  
 [STDisjoint &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **Pour déterminer si deux instances se croisent**  
 [STIntersects &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **Pour déterminer le ou les points où deux instances se croisent**  
 [STIntersection &#40;type de données geography&#41;](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Pour déterminer la distance la plus courte entre des points dans deux instances géographiques**  
 [STDistance &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
 **Pour déterminer la différence en points entre deux instances géographiques**  
 [STDifference &#40;type de données geography&#41;](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
 **Pour dériver la différence symétrique, ou points uniques, d'une instance geography comparée à une autre instance**  
 [STSymDifference &#40;type de données geography&#41;](../../t-sql/spatial-geography/stsymdifference-geography-data-type.md)  
  
##  <a name="supportedsrid"></a> Les instances geography doivent utiliser un SRID pris en charge  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les SRID basés sur les normes EPSG. Un SRID pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]pour les instances **geography** doit être utilisé pour l’exécution de calculs ou l’utilisation de méthodes avec des données spatiales géographiques. Le SRID doit correspondre à l’un des SRID présents dans l’affichage catalogue **sys.spatial_reference_systems** . Comme mentionné précédemment, quand vous effectuez des calculs sur vos données spatiales à l’aide du type de données **geography** , vos résultats dépendent de l’ellipsoïde qui a été utilisée pour créer vos données, car un identificateur de référence spatiale (SRID) spécifique est assigné à chaque ellipsoïde.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le SRID par défaut de 4326, qui se mappe au système de référence spatiale WGS 84, quand des méthodes sont utilisées dans des instances **geography** . Si vous utilisez des données d'un système de référence spatiale autre que WGS 84 (ou SRID 4326), vous devrez déterminer le SRID spécifique pour vos données spatiales geography.  
  
##  <a name="examples"></a> Exemples  
 Les exemples suivants montrent comment ajouter et interroger des données géographiques.  
  
-   Le premier exemple crée une table avec une colonne d'identité et une colonne `geography` `GeogCol1`. Une troisième colonne restitue la colonne `geography` dans sa représentation OGC (Open Geospatial Consortium) WKT (Well-Known Text) et utilise la méthode `STAsText()` . Deux lignes sont ensuite insérées : une ligne contient une instance `LineString` de `geography`et une ligne contient une instance `Polygon` .  
  
    ```  
    IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
        DROP TABLE dbo.SpatialTable;  
    GO  
  
    CREATE TABLE SpatialTable   
        ( id int IDENTITY (1,1),  
        GeogCol1 geography,   
        GeogCol2 AS GeogCol1.STAsText() );  
    GO  
  
    INSERT INTO SpatialTable (GeogCol1)  
    VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326));  
  
    INSERT INTO SpatialTable (GeogCol1)  
    VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
    GO  
    ```  
  
-   Le deuxième exemple utilise la méthode `STIntersection()` pour retourner les points où les deux instances `geography` précédemment insérées se croisent.  
  
    ```  
    DECLARE @geog1 geography;  
    DECLARE @geog2 geography;  
    DECLARE @result geography;  
  
    SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
    SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
    SELECT @result = @geog1.STIntersection(@geog2);  
    SELECT @result.STAsText();  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
