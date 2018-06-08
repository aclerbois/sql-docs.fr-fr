---
title: Pilotes | Documents Microsoft
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
- ODBC architecture [ODBC], drivers
- drivers [ODBC]
- drivers [ODBC], about drivers
ms.assetid: d6795d92-877e-44e1-b7d5-2ff2fd3989bd
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1906e61847537d3c89a5f2c3529172a9634713c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drivers"></a>Pilotes
*Pilotes* sont des bibliothèques qui implémentent les fonctions de l’API ODBC. Chacun est spécifique à un SGBD particulier ; par exemple, un pilote pour Oracle ne peut pas accéder directement les données dans un DBMS Informix. Pilotes d’exposent les fonctionnalités des SGBD sous-jacent ; ils ne sont pas requis pour implémenter des fonctionnalités non prises en charge par le SGBD. Par exemple, si le SGBD sous-jacent ne prend pas en charge les jointures externes, alors aucune des deux doit être le pilote. La seule exception majeure à cela est que les pilotes pour les SGBD qui n’ont pas de moteurs de base de données autonome, telles que Xbase, doivent implémenter un moteur de base de données qui prend en charge au moins d’une quantité minimale de SQL.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Tâches des pilotes](../../odbc/reference/driver-tasks.md)  
  
-   [Architecture des pilotes](../../odbc/reference/driver-architecture.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Pilotes ODBC fournis par Microsoft](../../odbc/microsoft/microsoft-supplied-odbc-drivers.md)
