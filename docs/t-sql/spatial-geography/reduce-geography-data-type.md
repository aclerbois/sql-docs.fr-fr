---
title: Reduce (type de données geography) | Microsoft Docs
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
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: c5dfa8c1-6764-41d8-9150-f3cb30633d3e
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fbe5db3d3c98246777a3980071ff7b1434c252a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reduce-geography-data-type-"></a>Reduce (type de données geography)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une approximation de l’instance **geography** donnée en exécutant l’algorithme de Douglas-Peucker sur l’instance, avec la tolérance donnée.  
  
 Cette méthode de type de données **geography** prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.Reduce ( tolerance )  
```  
  
## <a name="arguments"></a>Arguments  
  
|||  
|-|-|  
|Terme|Définition|  
|*tolerance*|Valeur de type **float**. *tolerance* est la tolérance à entrer pour l’algorithme Douglas-Peucker. *tolerance* doit être un nombre positif.|  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes   
 Pour les types de collection, cet algorithme fonctionne indépendamment sur chaque **geography** contenu dans l’instance. Cet algorithme ne modifie pas les instances **Point**.  
  
 Cette méthode tente de conserver les points de terminaison des instances **LineString**, mais elle peut ne pas y parvenir si elle a pour but de conserver un résultat valide.  
  
 Si `Reduce()` est appelé avec une valeur négative, cette méthode lève **ArgumentException**. Les tolérances utilisées dans `Reduce()` doivent être des nombres positifs.  
  
 L’algorithme Douglas-Peucker fonctionne sur chaque courbe ou anneau de l’instance **geography** en supprimant tous les points, à l’exception du point de départ et du point de terminaison. Chaque point supprimé est ensuite rajouté, en commençant par le point excentré le plus lointain, jusqu’à ce qu’aucun point ne soit à plus de la valeur *tolerance* du résultat. Le résultat est ensuite rendu valide si nécessaire, dans la mesure où un résultat valide est garanti.  
  
 Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cette méthode a été étendue aux instances **FullGlobe**.  
  
 Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` et utilise `Reduce()` afin de simplifier l'instance.  
  
```  
DECLARE @g geography = 'LineString(120 45, 120.1 45.1, 199.9 45.2, 120 46)'  
SELECT @g.Reduce(10000).ToString()  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
