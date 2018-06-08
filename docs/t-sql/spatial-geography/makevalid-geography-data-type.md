---
title: MakeValid (type de données geography) | Microsoft Docs
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
dev_langs:
- TSQL
helpviewer_keywords:
- MakeValid method (geography)
ms.assetid: f67038e3-4f62-4465-994e-e95ac27d8ada
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 50b3cd5285acce9673079a1592dc9bcd17ef3282
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="makevalid-geography-data-type"></a>MakeValid (type de données geography)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Convertit une instance **geography** non valide en instance **geography** valide avec un type OGC (Open Geospatial Consortium) valide.  
  
 Si un objet d’entrée retourne False pour STIsValid(), `MakeValid()` convertit l’instance non valide en instance valide.  
  
 Cette méthode de type de données geography prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.MakeValid ()  
```  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes   
 Cette méthode peut changer le type de l’instance **geography**. De plus, les points d’une instance **geography** peuvent se déplacer légèrement. Les résultats de certaines méthodes telles que NumPoint() peuvent changer.  
  
 Dans les cas où l’instance spatiale non valide croise l’équateur et où EnvelopeAngle() = 180, une instance **FullGlobe** est retournée. La méthode de type de données de `MakeValid()`**geography** tente de retourner des instances valides, mais l’exactitude ou la précision des résultats n’est pas garantie.  
  
> [!NOTE]  
>  Les objets qui ne sont pas valides peuvent être stockés dans la base de données. Les méthodes qui peuvent être exécutées sur des instances non valides (les instances pour lesquelles STIsValid() retourne False) sont des méthodes qui vérifient la validité ou permettent l’exportation : STIsValid(), MakeValid(), STAsText(), STAsBinary(), ToString(), AsTextZM() et AsGml().  
  
 Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
 Le premier exemple crée une instance `LineString` non valide qui se chevauche elle-même et utilise `STIsValid()` pour confirmer qu'il s'agit d'une instance non valide. `STIsValid()` retourne la valeur 0 pour une instance non valide.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(0 2, 1 1, 1 0, 1 1, 2 2)', 4326);  
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
  
## <a name="see-also"></a> Voir aussi  
 [STIsValid &#40;Type de données geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
