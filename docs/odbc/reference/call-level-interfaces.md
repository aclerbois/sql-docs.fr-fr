---
title: Niveau d’appel Interfaces | Documents Microsoft
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
- SQL statements [ODBC], CLI
- CLI [ODBC], using CLI
- sending SQL statements to DBMS [ODBC]
- SQL [ODBC], CLI
- call-level interface [ODBC], using call-level interface
ms.assetid: 42257bb6-0bf1-4533-a4ef-4a6dd2aecb18
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6094e97d73dc11ec4c6507b6d5018353e2e345e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="call-level-interfaces"></a>Interfaces de niveau d’appel
La dernière technique pour l’envoi d’instructions SQL au SGBD est via une interface de niveau d’appel (CLI). Une interface de niveau d’appel fournit une bibliothèque de fonctions SGBD qui peut être appelée par l’application. Par conséquent, au lieu de tenter de fusion SQL avec un autre langage de programmation, une interface de niveau d’appel est similaire aux routine la plupart des programmeurs sont habitués à utiliser, par exemple, la chaîne, d’e/s, les bibliothèques ou des bibliothèques de mathématiques dans C. Notez que SGBD qui prennent en charge de SQL incorporé déjà ont une interface au niveau de l’appel, les appels à qui sont générés par le précompilés. Toutefois, ces appels sont non documenté et l’objet de modifications sans préavis.  
  
 Interfaces de niveau d’appel sont couramment utilisés dans les architectures client/serveur, dans lequel le programme d’application (client) réside sur un ordinateur et le SGBD (serveur) sur un autre ordinateur. L’application appelle les fonctions de l’interface CLI sur le système local, et ces appels sont envoyés sur le réseau au SGBD pour le traitement.  
  
 Une interface de niveau d’appel est similaire à SQL dynamique, dans la mesure où les instructions SQL sont passées au SGBD pour le traitement au moment de l’exécution, mais elle diffère embedded SQL ensemble dans la mesure où aucun relevé SQL incorporé et aucun précompilé n’est requis.  
  
 À l’aide d’une interface de niveau d’appel généralement implique les étapes suivantes :  
  
1.  L’application appelle une fonction CLI pour se connecter au SGBD.  
  
2.  L’application génère une instruction SQL et le place dans une mémoire tampon. Il appelle ensuite une ou plusieurs fonctions CLI pour envoyer l’instruction au SGBD pour la préparation et l’exécution.  
  
3.  Si l’instruction est une instruction SELECT, l’application appelle une fonction CLI pour retourner les résultats dans les mémoires tampon d’application. En règle générale, cette fonction retourne une ligne ou une colonne de données à la fois.  
  
4.  L’application appelle une fonction CLI pour vous déconnecter du SGBD.
