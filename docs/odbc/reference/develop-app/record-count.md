---
title: Nombre d’enregistrements | Documents Microsoft
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
- record count [ODBC]
- descriptors [ODBC], record count
ms.assetid: 46eec3cc-0ecc-4980-9020-fb74a9af5730
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b4ab002f144070c94a659f0b1d67d09de9ceed50
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="record-count"></a>Nombre d'enregistrements
Le champ d’en-tête SQL_DESC_COUNT d’un descripteur est l’index de base un de l’enregistrement le plus élevé numérotées qui contient les données. Ce champ n’est pas un nombre de toutes les colonnes ou les paramètres qui sont liés. Lorsqu’un descripteur est alloué, la valeur initiale de SQL_DESC_COUNT est 0.  
  
 Le pilote prend toute action nécessaire d’allouer et de gérer le stockage nécessaires pour conserver les informations de descripteur. L’application ne ne sont pas explicitement spécifier la taille d’un descripteur et allouer de nouveaux enregistrements. Lors de l’application fournit des informations pour un enregistrement de descripteur dont le numéro est supérieur à la valeur de SQL_DESC_COUNT, le pilote augmente automatiquement SQL_DESC_COUNT. Lorsque l’application annule l’enregistrement du descripteur de la plus élevée numérotées, le pilote diminue automatiquement SQL_DESC_COUNT pour contenir le nombre de l’enregistrement lié restante la plus élevée.
