---
title: STBuffer (type de données geography) | Microsoft Docs
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
- STBuffer (geography Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geography Data Type)
ms.assetid: cb4deab8-642b-44d9-b3d9-85114d64021e
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 496665ca0990d0c73725456987a7d764f8f0bb20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stbuffer-geography-data-type"></a>STBuffer (type de données geography)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne un objet geography qui représente l’union de tous les points dont la distance par rapport à une instance **geography** est inférieure ou égale à une valeur spécifique.  
  
 Cette méthode de type de données geography prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STBuffer ( distance )  
```  
  
## <a name="arguments"></a>Arguments  
 *distance*  
 Valeur de type **float** (**double** dans le .NET Framework), qui spécifie la distance de l’instance **geography** autour de laquelle calculer la mémoire tampon.  
  
 La distance maximale du tampon ne peut pas dépasser 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre) ou le globe complet.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes   
 STBuffer() calcule une mémoire tampon de la même manière que [BufferWithTolerance](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md), en spécifiant *tolerance* = abs(distance) \* 0,001 et *relative* = **false**.  
  
 Une mémoire tampon négative supprime tous les points dans la distance donnée de la limite de l’instance **geography**.  
  
 `STBuffer()` retourne une instance **FullGlobe** dans certains cas ; par exemple `STBuffer()` retourne une instance **FullGlobe** quand la distance de mémoire tampon est supérieure à la distance entre l’équateur et les pôles. Une mémoire tampon ne peut pas dépasser le globe complet.  
  
 Cette méthode lève **ArgumentException** dans les instances **FullGlobe** où la distance de mémoire tampon dépasse la limite suivante :  
  
 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre)  
  
 La limite de distance maximale permet à la construction de la mémoire tampon d'être aussi flexible que possible.  
  
 L’erreur entre la mémoire tampon théorique et la mémoire tampon calculée correspond à max (tolerance, étendues * 1E-7), où tolerance = distance \* 0,001. Pour plus d’informations sur les étendues, consultez [Référence de méthodes de type de données geography](http://msdn.microsoft.com/library/028e6137-7128-4c74-90a7-f7bdd2d79f5e).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une instance `LineString``geography`. Il utilise ensuite `STBuffer()` pour retourner la région située à une proximité d'un mètre de l'instance.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STBuffer(1).ToString();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BufferWithTolerance &#40;type de données geography&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
