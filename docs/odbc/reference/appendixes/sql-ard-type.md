---
title: SQL_ARD_TYPE | Documents Microsoft
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
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], SQL_ARD_TYPE
- SQL_ARD_TYPE [ODBC]
ms.assetid: 8d87ca10-f955-4284-8689-e9f4cc31e7ae
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4e3d3ba38947babc5e7335fd38a63db18943b31f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlardtype"></a>SQL_ARD_TYPE
L’identificateur de type SQL_ARD_TYPE est utilisé pour indiquer que les données dans une mémoire tampon sera du type spécifié dans le champ SQL_DESC_CONCISE_TYPE de la ARD. SQL_ARD_TYPE est entré dans le *TargetType* argument d’un appel à **SQLGetData** au lieu d’un type de données spécifique et permet à une application de modifier les données de type de la mémoire tampon en modifiant le champ de descripteur. Cette valeur s’associe le type de données de la  *\*TargetValuePtr* tampon pour le champ de descripteur. (SQL_ARD_TYPE n’est pas entrée dans un appel à **SQLBindCol** ou **SQLBindParameter** , car le type de la mémoire tampon liée est déjà lié aux champs SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE et peut être modifié à tout moment en modifiant un de ces champs.)  
  
 L’identificateur de type SQL_ARD_TYPE peut être utilisé pour spécifier les valeurs par défaut pour la précision de début et de précision en secondes de types de données interval et tapez des valeurs de précision et l’échelle pour les données SQL_C_NUMERIC. Pour plus d’informations, consultez [de substitution par défaut de début et de précision de secondes pour les Types de données Interval](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md) et [substitution de la précision par défaut et l’échelle pour les Types de données numériques](../../../odbc/reference/appendixes/overriding-default-precision-and-scale-for-numeric-data-types.md), plus loin dans cette annexe.
