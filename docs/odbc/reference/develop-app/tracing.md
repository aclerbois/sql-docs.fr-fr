---
title: Le traçage | Documents Microsoft
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
- tracing options [ODBC], about tracing
- driver manager [ODBC], tracing
ms.assetid: 77ed4c6c-d976-4eb2-8526-a12697b0ef83
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8ff139b0074a22adc14903174ec031cd4176dca6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tracing"></a>Suivi
Le Gestionnaire de pilotes ODBC comporte une fonctionnalité de trace qui permet la séquence d’appels de fonction effectués par une application ODBC être enregistrés et transcrite dans un fichier journal. Le traçage est effectué par une DLL de traçage qui capture les appels entre l’application et le Gestionnaire de pilotes et entre le Gestionnaire de pilotes et le pilote. Cette méthode de suivi remplace le suivi effectué par l’API ODBC 2 *.x* du Gestionnaire de pilotes et le suivi effectuent dans ODBC 2 *.x* par ODBC Spy.  
  
 Cette section contient les rubriques suivantes.  
  
-   [DLL de traçage](../../../odbc/reference/develop-app/trace-dll.md)  
  
-   [Fichier de trace](../../../odbc/reference/develop-app/trace-file.md)  
  
-   [Activation du traçage](../../../odbc/reference/develop-app/enabling-tracing.md)  
  
-   [Traçage dynamique](../../../odbc/reference/develop-app/dynamic-tracing.md)
