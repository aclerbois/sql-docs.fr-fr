---
title: BufferWithTolerance (type de données geography) | Microsoft Docs
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
- BufferWithTolerance_TSQL
- BufferWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- BefferWithTolerance method
ms.assetid: f1783e6b-0f17-464f-b1c7-1c3f7d8aa042
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d0aa4291944b92dd83f2ce1d5d39704c171d8c76
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bufferwithtolerance-geography-data-type"></a>BufferWithTolerance (type de données geography)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne un objet géométrique qui représente l’union de toutes les valeurs de points dont la distance par rapport à une instance **geography** est inférieure ou égale à une valeur spécifique, en tenant compte d’une tolérance spécifique.  
  
 Cette méthode de type de données geography prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
## <a name="arguments"></a>Arguments  
 *distance*  
 Expression **float** spécifiant la distance de l’instance **geography** autour de laquelle calculer la mémoire tampon.  
  
 La distance maximale du tampon ne peut pas dépasser 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre) ou le globe complet.  
  
 *tolerance*  
 Expression **float** spécifiant la tolérance de la distance de mémoire tampon.  
  
 La valeur *tolerance* fait référence à la variation maximale dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée.  
  
 Par exemple, la distance de mémoire tampon idéale d'un point est un cercle, mais elle doit être exprimée de façon à se rapprocher d'un polygone. Plus la tolérance est petite, plus le polygone aura de points, ce qui augmente la complexité du résultat mais décroît l'erreur.  
  
 La limite minimum est de 0,1 pour cent de la distance, plus toute tolérance inférieure à la valeur qui sera arrondie à la limite minimale.  
  
 *relative*  
 **bit** spécifiant si la valeur de *tolerance* est relative ou absolue. Si la valeur est « TRUE » ou 1, la tolérance est relative et calculée sous la forme du produit du paramètre *tolerance* et de l’étendue angulaire \* le rayon équatorial de l’ellipsoïde. Si la valeur est « FALSE » ou 0, la tolérance est absolue et la valeur de *tolerance* représente la variation maximale absolue dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes   
 Cette méthode lève **ArgumentException** si *distance* est une valeur NAN (n’est pas un nombre), ou si *distance* est un infini positif ou négatif.  Cette méthode lève également **ArgumentException** si *tolerance* est égal à zéro (0), est une valeur NAN (n’est pas un nombre), est une valeur négative, ou est un infini positif ou négatif.  
  
 `STBuffer()` retourne une instance **FullGlobe** dans certains cas ; par exemple `STBuffer()` retourne une instance **FullGlobe** sur deux pôles quand la distance de mémoire tampon est supérieure à la distance entre l’équateur et les pôles.  
  
 Cette méthode lève **ArgumentException** dans les instances **FullGlobe** où la distance de mémoire tampon dépasse la limite suivante :  
  
 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre)  
  
 L’erreur entre la mémoire tampon théorique et la mémoire tampon calculée correspond à max(tolerance, étendues \* 1E-7), où tolerance représente la valeur du paramètre *tolerance*. Pour plus d’informations sur les étendues, consultez [Référence de méthodes de type de données geography](http://msdn.microsoft.com/library/028e6137-7128-4c74-90a7-f7bdd2d79f5e).  
  
 Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `Point` et utilise `BufferWithTolerance()` pour obtenir une estimation grossière de la mémoire tampon autour d'elle.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [STBuffer &#40;type de données geography&#41;](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)   
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
