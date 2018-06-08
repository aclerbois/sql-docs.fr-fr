---
title: STCurveN (type de données geometry) | Microsoft Docs
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
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveN method (geometry)
ms.assetid: 64adf1a1-3a41-41fb-b7d1-44390c3e4ea9
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c555027607e9327900e4fcfb3add2d86f9ad489d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stcurven-geometry-data-type"></a>STCurveN (type de données geometry)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

Retourne la courbe spécifiée à partir d’une instance **geometry** qui est **LineString**, **CircularString**, **CompoundCurve** ou **MultiLineString**.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STCurveN ( curve_index )  
```  
  
## <a name="arguments"></a>Arguments  
 *curve_index*  
 Expression **int** comprise entre 1 et le nombre de courbes de l’instance **geometry**.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="exceptions"></a>Exceptions  
 Si *curve_index* < 1, `ArgumentOutOfRangeException` est levé.  
  
## <a name="remarks"></a>Notes   
 **NULL** est retourné dans l’une des situations suivantes :  
  
-   L’instance **geometry** est déclarée, mais pas instanciée  
  
-   L’instance **geometry** est vide  
  
-   *curve_index* dépasse le nombre de courbes de l’instance **geometry**  
  
-   L’instance **geometry** est une instance **Point**, **MultiPoint**, **Polygon**, **CurvePolygon** ou **MultiPolygon**  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-stcurven-on-a-circularstring-instance"></a>A. Utilisation de STCurveN() sur une instance CircularString  
 L'exemple suivant retourne la deuxième courbe dans une instance `CircularString` :  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple précédent de cette rubrique retourne :  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve-instance-with-one-circularstring-instance"></a>B. Utilisation de STCurveN() sur une instance CompoundCurve avec une instance CircularString  
 L'exemple suivant retourne la deuxième courbe dans une instance `CompoundCurve` :  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple précédent de cette rubrique retourne :  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-instance-with-three-circularstring-instances"></a>C. Utilisation de STCurveN() sur une instance CompoundCurve avec trois instances CircularString  
 L'exemple suivant utilise une instance `CompoundCurve` qui combine trois instances `CircularString` distinctes dans la même séquence de courbes que l'exemple précédent :  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE (CIRCULARSTRING (0 0, 1 2.1082, 3 6.3246), CIRCULARSTRING(3 6.3246, 0 7, -3 6.3246), CIRCULARSTRING(-3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple précédent de cette rubrique retourne :  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
 Notez que les résultats sont les mêmes pour les trois exemples précédents. Quel que soit le format WKT (Well-known Text) utilisé pour entrer la même séquence de courbes, les résultats retournés par `STCurveN()` sont identiques lorsqu'une instance `CompoundCurve` est utilisée.  
  
### <a name="d-validating-the-parameter-before-calling-stcurven"></a>D. Validation du paramètre avant d'appeler STCurveN()  
 L’exemple suivant montre comment vérifier que `@n` est valide avant d’appeler la méthode `STCurveN()` :  
  
```
 DECLARE @g geometry;  
 DECLARE @n int;  
 SET @n = 3;  
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [STNumCurves &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumcurves-geometry-data-type.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

