---
title: Vérifications de la Transition d’état | Documents Microsoft
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
- diagnostic information [ODBC], driver manager error checking
- state transition checks [ODBC]
- driver manager [ODBC], error checking
ms.assetid: 0706db7d-e125-4845-a13a-7fe4308f7360
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 23f2b87fc943535075e1456bb31366ff9667b07b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="state-transition-checks"></a>Vérifications de la Transition d’état
Le Gestionnaire de pilotes vérifie que l’état de l’environnement, une connexion ou une instruction est approprié pour la fonction appelée. Par exemple, une connexion doit être dans un alloué état lorsque **SQLConnect** est appelé ; une instruction doit être dans la préparation d’une état lorsque **SQLExecute** est appelée. Le Gestionnaire de pilotes retourne SQL_ERROR pour les erreurs de transition d’état.
