---
title: LineString | Microsoft Docs
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
helpviewer_keywords:
- LineString geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: e50d0b86-8b31-4285-be71-ad05c7712cbd
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7cc6d09964a8d2794bbc6f6faa5082a74858a823
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="linestring"></a>LineString
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Un **LineString** est un objet unidimensionnel qui représente une séquence de points et les segments de ligne qui les connectent.  
  
## <a name="linestring-instances"></a>Instances LINESTRING  
 L’illustration suivante montre des exemples d’instances **LineString** .  
  
 ![Exemples d’instances LineString géométriques](../../relational-databases/spatial/media/linestring.gif "Exemples d’instances LineString géométriques")  
  
 Comme indiqué par l'illustration :  
  
-   La Figure 1 représente une instance **LineString** simple et non fermée.  
  
-   La Figure 2 représente une instance **LineString** non simple et non fermée.  
  
-   La Figure 3 représente une instance **LineString** fermée et simple ; il s’agit par conséquent d’un anneau.  
  
-   La Figure 4 représente une instance **LineString** fermée et non simple ; il ne s’agit par conséquent pas d’un anneau.  
  
### <a name="accepted-instances"></a>Instances acceptées  
 Les instances **LineString** acceptées peuvent être introduites dans une variable geometry, mais elles peuvent ne pas être des instances **LineString** valides. Pour qu’une instance **LineString** soit acceptée, elle doit répondre aux critères suivants. L'instance doit être formée d'au moins deux points ou être vide. Les instances LineString suivantes sont acceptées.  
  
```  
DECLARE @g1 geometry = 'LINESTRING EMPTY';  
DECLARE @g2 geometry = 'LINESTRING(1 1,2 3,4 8, -6 3)';  
DECLARE @g3 geometry = 'LINESTRING(1 1, 1 1)';  
```  
  
 `@g3` montre qu’une instance **LineString** peut être acceptée, mais non valide.  
  
 L’instance **LineString** suivante n’est pas acceptée. Elle lèvera une `System.FormatException`.  
  
```  
DECLARE @g geometry = 'LINESTRING(1 1)';  
```  
  
### <a name="valid-instances"></a>Instances valides  
 Pour qu’une instance **LineString** soit valide, elle doit répondre aux critères ci-dessous.  
  
1.  L’instance **LineString** doit être acceptée.  
  
2.  Si une instance **LineString** n’est pas vide, elle doit contenir au moins deux points distincts.  
  
3.  L’instance **LineString** ne peut pas se chevaucher sur un intervalle de plusieurs points consécutifs.  
  
 Les instances **LineString** suivantes sont valides.  
  
```  
DECLARE @g1 geometry= 'LINESTRING EMPTY';  
DECLARE @g2 geometry= 'LINESTRING(1 1, 3 3)';  
DECLARE @g3 geometry= 'LINESTRING(1 1, 3 3, 2 4, 2 0)';  
DECLARE @g4 geometry= 'LINESTRING(1 1, 3 3, 2 4, 2 0, 1 1)';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(), @g4.STIsValid();  
  
```  
  
 Les instances **LineString** suivantes ne sont pas valides.  
  
```  
DECLARE @g1 geometry = 'LINESTRING(1 4, 3 4, 2 4, 2 0)';  
DECLARE @g2 geometry = 'LINESTRING(1 1, 1 1)';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
> [!WARNING]  
>  La détection de chevauchements d’instances **LineString** se base sur des calculs en virgule flottante, qui ne sont pas exacts.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment créer une instance `geometry``LineString` avec trois points et un SRID égal à 0 :  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(1 1, 2 4, 3 9)', 0);  
```  
  
 Chaque point dans l'instance `LineString` peut contenir des valeurs Z (élévation) et M (mesure). Cet exemple ajoute des valeurs M à l'instance `LineString` créée dans l'exemple ci-dessus. M et Z peuvent être des valeurs NULL.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(1 1 NULL 0, 2 4 NULL 12.3, 3 9 NULL 24.5)', 0);  
```  
  
 L'exemple suivant montre comment créer une instance `geometry LineString` avec deux points identiques. Un appel à `IsValid` indique que l’instance **LineString** n’est pas valide et un appel à `MakeValid` convertira l’instance **LineString** en **Point**.  
  
```sql  
DECLARE @g geometry  
SET @g = geometry::STGeomFromText('LINESTRING(1 3, 1 3)',0);  
IF @g.STIsValid() = 1  
  BEGIN  
     SELECT @g.ToString() + ' is a valid LineString.';    
  END  
ELSE  
  BEGIN  
     SELECT @g.ToString() + ' is not a valid LineString.';  
     SET @g = @g.MakeValid();  
     SELECT @g.ToString() + ' is a valid Point.';    
  END  
  
```  
  
 L'extrait de code ci-dessus retournera les éléments suivants :  
  
```  
LINESTRING(1 3, 1 3) is not a valid LineString  
POINT(1 3) is a valid Point.  
```  
  
## <a name="see-also"></a> Voir aussi  
 [STLength &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STStartPoint &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)   
 [STEndPoint &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)   
 [STPointN &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)   
 [STNumPoints &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STIsRing &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)   
 [STIsClosed &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STPointOnSurface &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
