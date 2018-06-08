---
title: MakeValid (type de données geometry) | Microsoft Docs
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
- MakeValid
- MakeValid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MakeValid (geometry Data Type)
ms.assetid: 38673010-ab77-4ebb-9c4d-b26b79e3b7ea
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cd5a5702ff7ae3580158e574b0c84a7abb74842f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="makevalid-geometry-data-type"></a>MakeValid (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

Convertit une instance **geometry** non valide en instance **geometry** ayant un type OGC (Open Geospatial Consortium) valide.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.MakeValid ()  
```  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Notes   
 Cette méthode peut entraîner un changement de type de l’instance **geometry**, ainsi qu’un léger décalage des points d’une instance **geometry**.  
  
## <a name="examples"></a>Exemples  
 Le premier exemple crée une instance `LineString` non valide qui se chevauche elle-même et utilise `STIsValid()` pour confirmer qu'il s'agit d'une instance non valide. `STIsValid()` retourne la valeur 0 pour une instance non valide.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 1 1, 1 0, 1 1, 2 2)', 0);  
SELECT @g.STIsValid();  
```  
  
 Le deuxième exemple utilise `MakeValid()` pour rendre l'instance valide et tester que l'instance est effectivement valide. `STIsValid()` retourne la valeur 1 pour une instance valide.  
  
```  
SET @g = @g.MakeValid();  
SELECT @g.STIsValid();  
```  
  
 Le troisième exemple vérifie comment l'instance a été modifiée pour en faire une instance valide.  
  
```  
SELECT @g.ToString();  
```  
  
 Dans cet exemple, lorsque l'instance `LineString` est sélectionnée, les valeurs sont retournées en tant qu'instance `MultiLineString` valide.  
  
```  
MULTILINESTRING ((0 2, 1 1, 2 2), (1 1, 1 0))  
```  
  
 L'exemple suivant convertit l'instance CircularString en une instance Point.  
  
```  
DECLARE @g geometry = 'CIRCULARSTRING(1 1, 1 1, 1 1)';  
SELECT @g.MakeValid().ToString();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [STIsValid &#40;Type de données geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [Méthodes étendues sur des instances geometry](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

