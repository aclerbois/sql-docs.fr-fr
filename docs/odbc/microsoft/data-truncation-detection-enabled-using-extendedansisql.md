---
title: Détection de troncation de données activée à l’aide de ExtendedAnsiSQL | Documents Microsoft
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
- truncating data [ODBC]
- extendedANSISQL [ODBC], data truncation detection
ms.assetid: cec2359b-917d-4e1d-9625-5cd678b62f10
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 08183430384a7a5ced14871c7bd151cb21c40b2b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-truncation-detection-enabled-using-extendedansisql"></a>Détection de troncation de données activée à l’aide de ExtendedAnsiSQL
Lorsque l’indicateur ExtendedAnsiSQL est activé et l’application insère des données dans un char ou une colonne binaire des données sont tronquées, la troncation est détectée. Lorsque l’indicateur ExtendedAnsiSQL est désactivée, les données sont tronquées sans avertissement, tel qu’il était dans les versions précédentes des pilotes ODBC bureau de base de données.
