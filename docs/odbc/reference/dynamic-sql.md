---
title: Instructions SQL dynamiques | Documents Microsoft
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
- SQL [ODBC], embedded SQL
- SQL statements [ODBC], dynamic SQL
- SQL statements [ODBC], embedded SQL
- dynamic SQL [ODBC]
- SQL [ODBC], dynamic SQL
- embedded SQL [ODBC]
ms.assetid: 0bfb9ab7-9c15-4433-93bc-bad8b6c9d287
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 465f9785436880fce74136dd293abab33d1d2b4c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dynamic-sql"></a>Instructions SQL dynamiques
Bien que SQL statique fonctionne dans de nombreuses situations, il est une classe d’applications dans lequel l’accès aux données ne peut pas être déterminé à l’avance. Par exemple, qu'une feuille de calcul permet à un utilisateur à entrer une requête, puis envoie à la feuille de calcul au SGBD pour récupérer des données. Évidemment, le contenu de cette requête ne peut pas être connu au programmeur lorsque le programme de feuille de calcul est écrit.  
  
 Pour résoudre ce problème, la feuille de calcul utilise une forme de SQL incorporée appelée SQL dynamique. Contrairement aux instructions SQL statiques, qui sont codés en dur dans le programme, les instructions SQL dynamiques peuvent être générées au moment de l’exécution et placées dans une variable d’hôte de chaîne. Ils sont ensuite envoyées au SGBD pour traitement. Étant donné que le SGBD doit générer un plan d’accès au moment de l’exécution pour les instructions SQL dynamiques, SQL dynamique est généralement plus lent que SQL statique. Lorsqu’un programme contenant des instructions SQL dynamiques est compilé, les instructions SQL dynamiques ne sont pas supprimées du programme, comme dans les instructions SQL statiques. Au lieu de cela, ils sont remplacés par un appel de fonction qui passe l’instruction au DBMS ; les instructions SQL statiques dans le même programme sont traitées normalement.  
  
 Il est plus simple d’exécuter une instruction SQL dynamique avec une instruction EXECUTE IMMEDIATE. Cette instruction transmet l’instruction SQL au SGBD pour la compilation et l’exécution.  
  
 L’un des inconvénients de l’instruction EXECUTE IMMEDIATE sont que le SGBD doit passer par chacune des cinq étapes de traitement d’une instruction SQL chaque fois que l’instruction est exécutée. Les surcharges impliquées dans ce processus peuvent être significatif si de nombreuses instructions sont exécutées de manière dynamique, et il est inutile si ces instructions sont similaires. Pour résoudre cette situation, SQL dynamique offre une forme optimisée d’exécution appelé l’exécution préparée, qui utilise les étapes suivantes :  
  
1.  Le programme construit une instruction SQL dans une mémoire tampon, comme il le fait pour l’instruction EXECUTE IMMEDIATE. Au lieu de variables d’hôte, un point d’interrogation ( ?) peuvent être remplacée par une constante n’importe où dans le texte de l’instruction pour indiquer qu’une valeur pour la constante est fournie ultérieurement. Le point d’interrogation est appelé un marqueur de paramètre.  
  
2.  Le programme transmet l’instruction SQL au SGBD avec une instruction PREPARE, les demandes que le SGBD analyse, valider et d’optimiser l’instruction et générer une exécution planifier. Le programme utilise ensuite une instruction EXECUTE (pas une instruction EXECUTE IMMEDIATE) pour exécuter l’instruction PREPARE ultérieurement. Il transmet les valeurs de paramètre pour l’instruction via une structure de données spéciale appelée SQLDA ou zone de données SQL.  
  
3.  Le programme peut utiliser l’instruction EXECUTE à plusieurs reprises, en fournissant des valeurs de paramètre différentes chaque fois que cette instruction est exécutée.  
  
 L’exécution préparée n’est toujours pas le même que SQL statique. Dans les instructions SQL statiques, les quatre premières étapes de traitement d’une instruction SQL lieu au moment de la compilation. Dans l’exécution préparée, ces étapes ont toujours lieu au moment de l’exécution, mais elles sont exécutées une seule fois ; exécution du plan a lieu uniquement lorsque EXECUTE est appelée. Cela permet d’éliminer certains inconvénients de performances inhérentes à l’architecture d’instructions SQL dynamiques. L’illustration suivante montre les différences entre SQL statique, SQL dynamique avec l’exécution immédiate et SQL dynamique avec l’exécution préparée.
