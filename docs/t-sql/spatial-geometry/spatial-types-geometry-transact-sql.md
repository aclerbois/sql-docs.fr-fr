---
title: geometry (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|spatial-geography
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- geometry
dev_langs:
- TSQL
helpviewer_keywords:
- spatial data types [SQL Server]
- geometry data type [SQL Server], Transact-SQL
ms.assetid: 3fefdf7b-f931-404c-821c-82c0375eaf51
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ee15f8090e5ef6fc329c6d04800ba2c6707f571f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spatial-types---geometry-transact-sql"></a>Types spatiaux - geometry (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Le type de données spatiales planaire, **geometry**, est implémenté en tant que type de données CLR (Common Language Runtime) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce type représente des données dans un système de coordonnées euclidien (plat).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un ensemble de méthodes pour le type de données spatiales **geometry**. Ces méthodes incluent des méthodes sur **geometry**, définies par la norme OGC (Open Geospatial Consortium), et un ensemble d’extensions [!INCLUDE[msCoName](../../includes/msconame-md.md)] de cette norme.  
 
 La tolérance d’erreur des méthodes geometry peut aller jusqu’à 1e-7 * étendues. Les étendues font référence à la distance maximale approximative entre les points de l’objet **geometry**.
  
## <a name="registering-the-geometry-type"></a>Inscription du type geometry  
 Le type **geometry** est prédéfini et disponible dans chaque base de données. Vous pouvez créer des colonnes de table de type **geometry** et opérer sur les données **geometry** comme vous le feriez avec d'autres types CLR. Peut être utilisé dans les colonnes calculées persistantes et non persistantes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-how-to-add-and-query-geometry-data"></a>A. Illustration de l'ajout et de l'interrogation des données géométriques  
 Les deux exemples suivants montrent comment ajouter et interroger des données géométriques. Le premier exemple crée une table avec une colonne d’identité et une colonne `geometry`, `GeomCol1`. Une troisième colonne restitue la colonne `geometry` dans sa représentation OGC (Open Geospatial Consortium) WKT (Well-Known Text) et utilise la méthode `STAsText()` . Deux lignes sont ensuite insérées : une ligne contient une instance `LineString` de `geometry`et une ligne contient une instance `Polygon` .  
  
```sql 
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
  
### <a name="b-returning-the-intersection-of-two-geometry-instances"></a>B. Retour de l'intersection de deux instances géométriques  
 Le deuxième exemple utilise la méthode `STIntersection()` pour retourner les points où les deux instances `geometry` précédemment insérées se croisent.  
  
```sql  
DECLARE @geom1 geometry;  
DECLARE @geom2 geometry;  
DECLARE @result geometry;  
  
SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geom1.STIntersection(@geom2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geometry-in-a-computed-column"></a>C. Utilisation du type géométrique dans une colonne calculée  
 L’exemple suivant crée une table avec une colonne calculée persistante à l’aide d’un type **geometry**.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geometry,  
    deliveryArea as location.STBuffer(10) persisted  
)  
```  
  
## <a name="see-also"></a> Voir aussi  
  [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
