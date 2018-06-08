---
title: Types de données dans Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3b7ac934afdc086a50020e9ba22ca2d236ee091b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-types-in-analysis-services"></a>Types de données dans Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Pour toutes les <xref:Microsoft.AnalysisServices.DataItem> objets, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] prend en charge le sous-ensemble de **System.Data.OleDb.OleDbType**. Pour définir ou lire le type de données, utilisez [Type de données DataItem &#40;ASSL&#41;](../../../analysis-services/scripting/data-type/dataitem-data-type-assl.md).  
  
## <a name="supported-data-types"></a>Types de données pris en charge  
  
|||  
|-|-|  
|BigInt|Entier signé de 64 bits. Le *BigInt* type valeur représente des entiers dont les valeurs comprises entre négatif 9,223,372,036,854,775,808 à 9,223,372,036,854,775,807 positif.|  
|Binaire|Un flux de données binaires de **octets** type. **Octets** est un type valeur qui représente des entiers non signés avec des valeurs comprises entre 0 et 255.|  
|Booléen|Instances de ce type ont des valeurs **true** ou **false**.|  
|Monétaire (Currency)|A *devise* valeur comprise entre -922,337,203,685,477.5808 et + 922,337,203,685,477.5807 avec une précision au dix-millième d’unité monétaire (quatre positions décimales).|  
|Date|Date et données de temps, stockées comme un double. La partie entière correspond au nombre de jours depuis le 30 décembre 1899 tandis que la partie fractionnaire désigne une fraction d'un jour ou l'heure.|  
|Double|Nombre à virgule flottante compris entre -1,79769313486232E +308 et 1,79769313486232E +308. Une valeur Double stocke les informations de nombre jusqu'à 15 chiffres décimaux de précision.|  
|Entier|Entier signé 32 bits représentant des entiers signés avec des valeurs qui varient entre 2 147 483 648 (négatif) et 2 147 483 647 (positif).|  
|Unique|Nombre à virgule flottante compris entre - 3,4028235E +38 et 3,4028235E +38. Une valeur Single stocke les informations de nombre jusqu'à 7 chiffres décimaux de précision.|  
|Smallint|Entier signé 16 bits. Le *Smallint* type valeur représente des entiers signés avec des valeurs comprises entre moins 32 768 et 32 767 positif.|  
|Tinyint|Entier signé 8 bits. Le type de valeur Tinyint représente des entiers dont la valeur varie entre 128 (négatif) et 127 (positif).|  
|UnsignedBigInt|Entier non signé 64 bits. Le *UnsignedBigInt* type valeur représente des entiers non signés avec des valeurs comprises entre 0 et 18,446,744,073,709,551,615.|  
|UnsignedInt|Entier non signé 32 bits. Le *UnsignedInt* type valeur représente des entiers non signés avec des valeurs comprises entre 0 et 4 294 967 295.|  
|UnsignedSmallInt|Entier non signé 16 bits. Le *UnsignedSmallInt* type valeur représente des entiers non signés avec des valeurs comprises entre 0 et 65535.|  
|UnsignedTinyInt|Entier non signé 8 bits. Le *UnsignedTinyInt* type valeur représente des entiers non signés avec des valeurs comprises entre 0 et 255|  
|WChar|Flux de caractères Unicode terminé par le caractère NULL. A *WChar* est une collection séquentielle de caractères Unicode qui est utilisée pour représenter du texte.|  
  
## <a name="amo-validations-on-data-types"></a>Validations AMO sur les types Data  
 Le tableau suivant contient les validations supplémentaires effectuées par AMO (Analysis Management Objects) pour certaines liaisons :  
  
|Objet|Binding|Types de données autorisés|  
|------------|-------------|------------------------|  
|DimensionAttribute|KeyColumns|Tous à l'exception de Binary|  
||NameColumn|WChar uniquement|  
||SkippedLevelsColumn|Types entiers uniquement : BigInt, Integer, SmallInt, TinyInt, UnsignedBigInt, UnsignedInt, UnsignedSmallInt, UnsignedTinyInt|  
||CustomRollupColumn|WChar uniquement|  
||CustomRollupPropertiesColumn|WChar uniquement|  
||UnaryOperatorColumn|WChar uniquement|  
||ValueColumn|Tous|  
|AttributeTranslation|CaptionColumn|WChar uniquement|  
|ScalarMiningStructureColumn|KeyColumns|Tous à l'exception de Binary|  
||NameColumn|WChar uniquement|  
|TableMiningStructureColumn|ForeignKeyColumns|Tous à l'exception de Binary|  
|MeasureGroupAttribute|KeyColumns|Tous à l'exception de Binary|  
|Mesure de comptage de valeurs|Source|BigInt, Currency, Double, Integer, Single, SmallInt, TinyInt, UnsignedBigInt, UnsignedInt, UnsignedSmallInt, UnsignedTinyInt|  
  
  
