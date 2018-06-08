---
title: Séquence d’enregistrements d’état | Documents Microsoft
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
- diagnostic information [ODBC], diagnostic records
- status records [ODBC]
- diagnostic records [ODBC]
ms.assetid: 0e0436cc-230f-44b0-b373-04a57e83ee76
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c4b1e2ceea30ecb62dd96be283bb150d2e43385a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sequence-of-status-records"></a>Séquence d’enregistrements d’état
Si deux ou plusieurs enregistrements d’état sont retournés, le Gestionnaire de pilotes et le pilote les classement selon les règles suivantes. L’enregistrement avec le rang le plus élevé est le premier enregistrement. La source d’un enregistrement (Gestionnaire de pilotes, pilotes, passerelle et ainsi de suite) n’est pas constitue lorsque les enregistrements de classement.  
  
-   **Erreurs** enregistrements d’état qui décrivent les erreurs ont le rang le plus élevé. Parmi les enregistrements d’erreur, les enregistrements qui indiquent un échec de la transaction ou l’échec de la transaction possible outrank tous les autres enregistrements. Si deux ou plusieurs enregistrements décrivent la même condition d’erreur, SQLSTATE définis par la spécification CLI de groupe ouvert (classes 03 via HZ) outrank SQLSTATE définies par ODBC et définies par le pilote.  
  
-   **Aucune des valeurs de données défini par l’implémentation** enregistrements d’état qui décrivent les valeurs d’absence de données définies par le pilote (classe 02) ont la deuxième rang le plus élevé.  
  
-   **Avertissements** enregistrements d’état qui décrivent les avertissements (classe 01) ont le rang le plus bas. Si deux ou plusieurs enregistrements décrivent la même condition d’avertissement, avertissement SQLSTATE définis par la spécification CLI de groupe ouvert outrank SQLSTATE définies par ODBC et définies par le pilote.  
  
 S’il existe deux ou plusieurs enregistrements avec le rang le plus élevé, il n’est pas l’enregistrement est le premier enregistrement. L’ordre de tous les autres enregistrements n’est pas défini. En particulier, étant donné que les avertissements peuvent apparaître avant les erreurs, les applications doivent vérifier tous les enregistrements d’état lorsqu’une fonction retourne une valeur autre que SQL_SUCCESS.
