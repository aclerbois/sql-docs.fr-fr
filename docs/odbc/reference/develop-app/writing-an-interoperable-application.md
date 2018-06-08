---
title: Écrivez une Application interopérable | Documents Microsoft
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
- interoperability [ODBC], feature support and variability
- interoperability [ODBC], writing interoperable applications
- feature support in interoperable applications [ODBC]
- feature variability in interoperable applications [ODBC]
ms.assetid: 8b42b8ae-7862-4b63-a0b3-2a204e0c43a5
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6aae50c316072c0970ffea4eb953f4e0ee86c5d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="writing-an-interoperable-application"></a>Écrivez une Application interopérable
Chaque fois qu’une application utilise le même code sur plusieurs pilotes, ce code doit être interopérable parmi ces pilotes. Dans la plupart des cas, il s’agit d’une tâche facile. Par exemple, le code pour extraire les lignes avec un curseur avant uniquement est identique pour tous les pilotes. Dans certains cas, cela peut être plus difficile. Par exemple, le code pour construire des identificateurs pour une utilisation dans les instructions SQL doit prendre en compte la casse de l’identificateur, des guillemets et des conventions de dénomination en trois parties, en deux parties et une seule partie.  
  
 En règle générale, les code interopérable doit faire face à des problèmes de prise en charge de la fonctionnalité et la variabilité de la fonctionnalité. *Prise en charge des fonctionnalités* fait référence à ou non une fonctionnalité particulière est prise en charge. Par exemple, pas tous les SGBD prennent en charge les transactions et code interopérable doit fonctionner correctement, quelle que soit la prise en charge de la transaction. *Fonctionnalité variabilité* fait référence à la variation de la manière qui prend en charge une fonctionnalité particulière. Par exemple, les noms de catalogues sont placés au début des identificateurs dans certains SGBD et à la fin des identificateurs dans d’autres.  
  
 Peuvent traiter les applications avec prise en charge de la fonctionnalité et la variabilité de la fonctionnalité au moment du design ou au moment de l’exécution. Pour gérer la prise en charge de la fonctionnalité et la variabilité au moment du design, un développeur examine la cible SGBD et les pilotes et permet de s’assurer que le même code seront interopérable entre eux. Il s’agit généralement de la méthode dans laquelle les applications avec l’interopérabilité faible ou limitée traitent ces problèmes.  
  
 Par exemple, si le développeur garantit qu’une application verticale fonctionne qu’avec les quatre SGBD particulière et si chacun de ces SGBD prend en charge les transactions, l’application est inutile code de vérification de la prise en charge de la transaction en cours d’exécution. Il peut toujours supposer que les transactions sont disponibles en raison de la décision au moment du design à utiliser seulement quatre SGBD, chacun d’eux prend en charge les transactions.  
  
 Pour gérer la prise en charge de la fonctionnalité et la variabilité au moment de l’exécution, l’application doit tester différentes fonctions en cours d’exécution et agir en conséquence. Il s’agit généralement de la méthode dans laquelle des applications hautement interopérables traitent ces problèmes. Pour les problèmes de prise en charge de fonctionnalité, cela signifie que l’écriture de code qui rend la fonctionnalité facultative ou l’écriture de codes qui émule la fonctionnalité lorsqu’il n’est pas disponible. Pour les problèmes de variabilité de fonctionnalité, cela signifie que l’écriture de code qui prend en charge toutes les variations possibles.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Vérification de la prise en charge et de la variabilité des fonctionnalités](../../../odbc/reference/develop-app/checking-feature-support-and-variability.md)  
  
-   [Fonctionnalités à considérer avec attention](../../../odbc/reference/develop-app/features-to-watch-for.md)
