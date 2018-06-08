---
title: MultiLineString | Microsoft Docs
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
- MultiLineString geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: 95deeefe-d6c5-4a11-b347-379e4486e7b7
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: afb12ee323ca4ddec2e569a127fe90b80f359e40
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="multilinestring"></a>MultiLineString
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Un objet **MultiLineString** est une collection de zéro ou de plusieurs instances de **geometry** ou **geographyLineString** .  
  
## <a name="multilinestring-instances"></a>Instances MultiLineString  
 L’illustration suivante montre des exemples d’instances **MultiLineString** .  
  
 ![Exemples d’instances MultiLineString géométriques](../../relational-databases/spatial/media/multilinestring.gif "Exemples d’instances MultiLineString géométriques")  
  
 Comme indiqué par l'illustration :  
  
-   La Figure 1 est une instance simple de **MultiLineString** dont la limite est constituée des quatre points de terminaison de ses deux éléments **LineString** .  
  
-   La Figure 2 est une instance simple de **MultiLineString** , car seuls les points de terminaison des éléments **LineString** se croisent. La limite est constituée des deux points de terminaison non chevauchants.  
  
-   La Figure 3 est une instance non simple de **MultiLineString** , car l’intérieur de l’un de ses éléments **LineString** est croisé. La limite de cette instance **MultiLineString** est constituée des quatre points de terminaison.  
  
-   La Figure 4 est une instance **MultiLineString** non simple et non fermée.  
  
-   La Figure 5 est une instance **MultiLineString**simple et non fermée. Elle n’est pas fermée, car ses éléments **LineStrings** ne sont pas fermés. Elle est simple, car aucun des intérieurs des instances **LineStrings** ne se croise.  
  
-   La Figure 6 est une instance **MultiLineString** simple et fermée. Elle est fermée car tous ses éléments sont fermés. Elle est simple car aucun de ses éléments ne se croise aux intérieurs.  
  
### <a name="accepted-instances"></a>Instances acceptées  
 Pour qu’une instance **MultiLineString** soit acceptée, elle doit être vide ou contenir uniquement les instances **LineString** acceptées. Pour plus d’informations sur les instances **LineString** acceptées, consultez [LineString](../../relational-databases/spatial/linestring.md). Les exemples suivants illustrent des instances **MultiLineString** acceptées.  
  
```  
DECLARE @g1 geometry = 'MULTILINESTRING EMPTY';  
DECLARE @g2 geometry = 'MULTILINESTRING((1 1, 3 5), (-5 3, -8 -2))';  
DECLARE @g3 geometry = 'MULTILINESTRING((1 1, 5 5), (1 3, 3 1))';  
DECLARE @g4 geometry = 'MULTILINESTRING((1 1, 3 3, 5 5),(3 3, 5 5, 7 7))';  
```  
  
 L’exemple suivant lève une exception `System.FormatException` , car la deuxième instance de **LineString** n’est pas valide.  
  
```  
DECLARE @g geometry = 'MULTILINESTRING((1 1, 3 5),(-5 3))';  
```  
  
### <a name="valid-instances"></a>Instances valides  
 Pour qu’une instance **MultiLineString** soit valide, elle doit répondre aux critères suivants :  
  
1.  Toutes les instances comprenant l’instance **MultiLineString** doivent être des instances **LineString** valides.  
  
2.  Deux instances **LineString** comprenant l’instance **MultiLineString** ne peuvent pas se chevaucher sur un intervalle. Les instances **LineString** peuvent uniquement se croiser, se toucher ou toucher d’autres instances **LineString** à un nombre fini de points.  
  
 L’exemple suivant illustre trois instances **MultiLineString** valides et une instance **MultiLineString** non valide.  
  
```  
DECLARE @g1 geometry = 'MULTILINESTRING EMPTY';  
DECLARE @g2 geometry = 'MULTILINESTRING((1 1, 3 5), (-5 3, -8 -2))';  
DECLARE @g3 geometry = 'MULTILINESTRING((1 1, 5 5), (1 3, 3 1))';  
DECLARE @g4 geometry = 'MULTILINESTRING((1 1, 3 3, 5 5),(3 3, 5 5, 7 7))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(), @g4.STIsValid();  
```  
  
 `@g4` n’est pas valide, car la deuxième instance **LineString** chevauche la première instance **LineString** à un intervalle. Elles se touchent à un nombre infini de points.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une instance `geometry``MultiLineString` simple qui contient deux éléments `LineString` avec le SRID 0.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 2, 1 1), (1 0, 1 1))');  
```  
  
 Pour instancier cette instance avec un SRID différent, utilisez `STGeomFromText()` ou `STMLineStringFromText()`. Vous pouvez également utiliser `Parse()` puis modifier le SRID, comme indiqué dans l'exemple suivant.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 2, 1 1), (1 0, 1 1))');  
SET @g.STSrid = 13;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [STLength &#40;Type de données geometry&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STIsClosed &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [LineString](../../relational-databases/spatial/linestring.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
