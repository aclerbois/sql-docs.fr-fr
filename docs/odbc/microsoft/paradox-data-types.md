---
title: Types de données Paradox | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Paradox driver
- desktop database drivers [ODBC], Paradox driver
- Paradox data types [ODBC]
- Jet-based ODBC drivers [ODBC], Paradox driver
- data types [ODBC], Paradox driver
- Paradox driver [ODBC], data types
ms.assetid: 0c9e5d21-9321-49f8-a055-69459e1c9c85
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 43c117a9026c1d00b879ab88892cb2b234894646
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="paradox-data-types"></a>Types de données Paradox
Le pilote ODBC Paradox mappe les types de données de Corel aux types de données ODBC SQL. Le tableau suivant répertorie tous les types de données Paradox et montre le code SQL ODBC elles sont mappées à des types de données.  
  
|Type de données Paradox|Type de données ODBC|  
|-----------------------|--------------------|  
|ALPHANUMÉRIQUE|SQL_VARCHAR|  
|AUTOINCREMENT [1]|SQL_INTEGER|  
|BCD [1]|SQL_DOUBLE|  
|OCTETS [1]|SQL_BINARY|  
|DATE|SQL_DATE|  
|IMAGE [2]|SQL_LONGVARBINARY|  
|LOGIQUE [1]|SQL_BIT|  
|LONG [1]|SQL_INTEGER|  
|MÉMO [2]|SQL_LONGVARCHAR|  
|MONEY [1]|SQL_DOUBLE|  
|NUMBER|SQL_DOUBLE|  
|COURTE|SQL_SMALLINT|  
|HEURE [1]|SQL_TIMESTAMP|  
|TIMESTAMP [1]|SQL_TIMESTAMP|  
  
 Valide [1] uniquement pour les versions de Paradox 5. *x*.  
  
 [2] de valide uniquement pour les versions de Paradox 4. *x* et 5. *x*.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC SQL. Toutes les conversions dans l’annexe D de la *de référence du programmeur ODBC* sont pris en charge pour les types de données ODBC SQL répertoriées plus haut dans cette rubrique.  
  
 Le tableau suivant présente des limitations sur les types de données de Corel.  
  
|Type de données| Description|  
|---------------|-----------------|  
|ALPHANUMÉRIQUE|Création d’une colonne alphanumérique de zéro ou de longueur non spécifiée retourne en fait une colonne de 255 octets.|  
|BYTES|Si vous insérez la valeur NULL dans une colonne binaire avec le pilote Paradox5, il est modifié en 0.|  
|LONG|La valeur négative maximale prise en charge par le pilote Paradox pour le type de données de type Long dans Paradox 5. *x* n’est pas -2 ^ 31 (-2147483648), comme il convient depuis maps Long pour les données ODBC tapez SQL_INTEGER. La valeur négative maximale prise en charge de longue durée est en réalité -2 ^ 31 + 1 (-2147483647).|  
|TIMESTAMP|Lorsqu’une valeur est insérée dans une colonne TIMESTAMP par le pilote Paradox, puis extraites par la suite de la colonne, la valeur récupérée peut différer de la valeur insérée par autant que 1 seconde en raison de l’arrondi.|  
  
 Vous trouverez davantage de contraintes sur les types de données dans [Limitations du Type de données](../../odbc/microsoft/data-type-limitations.md).
