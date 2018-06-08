---
title: Configurer le curseur | Documents Microsoft
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
- cursors [ODBC], scrollable
- cursors [ODBC], creating
ms.assetid: b80afb0e-ef2f-408f-86f5-a392edd99a56
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9ae6f687a09658121955f1e1d8b258ae7233657e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-up-the-cursor"></a>Configuration du curseur
L’application peut spécifier le type de curseur avant l’exécution d’une instruction qui crée un résultat défini. Il effectue cette opération avec l’attribut d’instruction SQL_ATTR_CURSOR_TYPE. Si l’application ne spécifie pas explicitement un type, un curseur avant uniquement est utilisé. Pour obtenir un curseur mixte, une application spécifie un curseur keyset, mais déclare une jeu de clés taille inférieure à la taille du jeu de résultats.  
  
 Pour les curseurs pilotés par jeu de clés et mixtes, l’application peut également spécifier la taille du jeu de clés. Il effectue cette opération avec l’attribut d’instruction SQL_ATTR_KEYSET_SIZE. Si la taille du jeu de clés est définie sur 0, ce qui est la valeur par défaut, la taille du jeu de clés est définie à la taille du jeu de résultats et un curseur keyset est utilisé. La taille de jeu de clés peut être modifiée une fois que le curseur a été ouvert.  
  
 L’application peut également définir la taille de l’ensemble de lignes ; Pour plus d’informations, consultez [à l’aide de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md), plus haut dans cette section.
