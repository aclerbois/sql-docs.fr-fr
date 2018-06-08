---
title: AsTextZM (type de données geography) | Microsoft Docs
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
- AsTextZM (geography Data Type)
- AsTextZM_TSQL
- AsTextZM
- AsTextZM_(geography_Data_Type)_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsTextZM method
ms.assetid: e9dc27f6-e945-4457-8498-7644db34008e
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f908630a4fe4016282c977aa5257bf87d0076f4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="astextzm-geography-data-type"></a>AsTextZM (type de données geography)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne la représentation OGC (Open Geospatial Consortium) WKT (Well-Known Text) d’une instance **geography**, à laquelle s’ajoutent les valeurs **Z** (élévation) et **M** (mesure) apportées par l’instance.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.AsTextZM ()  
```  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **nvarchar(max)**  
  
 Type de retour CLR : **SqlChars**  
  
## <a name="remarks"></a>Notes   
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une instance `Point` qui contient les valeurs **Z** (élévation) et **M** (mesure). `STAsText()` sélectionne les valeurs WKT (-122.34900 47.65100) ; `AsTextZM()` sélectionne les mêmes valeurs WKT et retourne également les valeurs pour **Z** et **M**, ce qui donne (-122.34900 47.65100 10.3 12).  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100 10.3 12)', 4326);  
SELECT @g.STAsText();  
SELECT @g.AsTextZM();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes étendues sur des instances Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [M &#40;type de données geography&#41;](../../t-sql/spatial-geography/m-geography-data-type.md)   
 [Z &#40;type de données geography&#41;](../../t-sql/spatial-geography/z-geography-data-type.md)  
  
  
