---
title: Exemple de Diagnostic du Gestionnaire de pilotes | Documents Microsoft
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
- driver manager [ODBC], diagnostic messages
- diagnostic information [ODBC], examples
- error messages [ODBC], diagnostic messages
ms.assetid: af8f2d35-d1bf-495c-af25-630654542b7d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8c383dda247d3309ed38744609afb5ca12e3c38f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-manager-diagnostic-example"></a>Exemple de Diagnostic du Gestionnaire de pilotes
Le Gestionnaire de pilotes peut également générer des messages de diagnostic. Par exemple, si une application a transmis une option non valide de direction pour **SQLDataSources**, le Gestionnaire de pilotes peut mettre en forme et renvoyer les valeurs suivantes à partir de **SQLGetDiagRec**:  
  
```  
SQLSTATE:         "HY103"  
Native Error:      0  
Diagnostic Msg:   "[Microsoft][ODBC Driver Manager]Direction option out of range"  
```  
  
 Étant donné que l’erreur s’est produite dans le Gestionnaire de pilotes, ajouté préfixes pour le message de diagnostic pour le fournisseur ([Microsoft]) et de son identificateur ([Gestionnaire de pilotes ODBC]).
