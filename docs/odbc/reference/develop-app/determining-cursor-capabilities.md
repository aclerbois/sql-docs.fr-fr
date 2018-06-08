---
title: Détermination des fonctionnalités de curseur | Documents Microsoft
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
- scrollable cursors [ODBC]
- cursors [ODBC], capabilities
- cursors [ODBC], scrollable
ms.assetid: 35be486c-8f2d-4cec-beb8-df14151abfef
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3974fb30a9151cdcf681a106ddcf3073b63f66b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="determining-cursor-capabilities"></a>Détermination des fonctionnalités de curseur
Les quatre options suivantes dans **SQLGetInfo** décrivent les types de curseurs sont pris en charge et leurs capacités :  
  
-   SQL_CURSOR_SENSITIVITY. Indique si un curseur est sensible aux modifications apportées par un autre curseur.  
  
-   SQL_SCROLL_OPTIONS. Répertorie les types de curseurs pris en charge (avant uniquement, statiques, commandé par keyset, dynamiques ou mixtes). Toutes les sources de données doivent prendre en charge les curseurs avant uniquement.  
  
-   SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur). Répertorie les types d’extraction prises en charge par les curseurs de défilement. Les bits de la valeur de retour correspondent aux types de fetch dans **SQLFetchScroll**.  
  
-   SQL_KEYSET_CURSOR_ATTRIBUTES2 ou SQL_STATIC_CURSOR_ATTRIBUTES2 (selon le type du curseur). Indique si les curseurs statiques et pilotés par peuvent détecter leurs propres mises à jour, suppressions et insertions.  
  
 Une application peut déterminer les fonctionnalités de curseur en cours d’exécution en appelant **SQLGetInfo** avec ces options. Cela est généralement effectué par les applications génériques. Fonctions de curseur également peuvent être déterminées pendant le développement d’applications et de leur utilisation codé en dur dans l’application. Cela est généralement effectué par les applications verticales et personnalisées, mais peut également être effectuée par les applications génériques qui utilisent une implémentation d’un curseur côté client telles que la bibliothèque de curseurs ODBC.
