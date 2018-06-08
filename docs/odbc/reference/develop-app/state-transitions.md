---
title: Transitions d’état | Documents Microsoft
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
- state transitions [ODBC]
- unallocated state [ODBC]
- handles [ODBC], state transitions
- allocated state [ODBC]
- connection state [ODBC]
ms.assetid: fc741611-6535-43cc-8156-6d897d04664e
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 424c421b29dce6bedad9a73c1d27acb01b6e77c1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="state-transitions"></a>Transitions d’état
ODBC définit discrètes *états* pour chaque environnement, chaque connexion et chaque instruction. Par exemple, l’environnement a trois états possibles : non alloué (dans lequel aucun environnement n’est affectée), alloué (dans lequel un environnement est alloué, mais aucune connexion n’est allouées) et la connexion (dans lequel un environnement et une ou plusieurs connexions sont allouées). Les connexions ont sept États possibles ; les instructions avoir 13 états possibles.  
  
 Un élément particulier, tel qu’identifié par son handle, passe d’un état à un autre lorsque l’application appelle une certaine fonction ou des fonctions et passe le handle à cet élément. Déplacement de ce type est appelé un *transition d’état*. Par exemple, allouer un handle d’environnement avec **SQLAllocHandle** déplace l’environnement non alloué vers alloué et la libération de ce descripteur avec **SQLFreeHandle** quitte alloué à non alloué. ODBC définit un nombre limité de transitions d’état autorisées, qui est une autre façon de dire que les fonctions doivent être appelées dans un certain ordre.  
  
 Certaines fonctions, telles que **SQLGetConnectAttr**, n’affectent pas l’état du tout. Autres fonctions affectent l’état d’un élément unique. Par exemple, **SQLDisconnect** déplace une connexion à partir d’un état de la connexion à un état alloué. Enfin, certaines fonctions affectent l’état de plusieurs éléments. Par exemple, allouer un handle de connexion avec **SQLAllocHandle** déplace une connexion à partir d’une non alloué à un état alloué et le place à partir d’un alloué à un état de connexion.  
  
 Si une application appelle une fonction dans le désordre, la fonction retourne un *erreur de transition d’état*. Par exemple, si un environnement est dans un état de connexion et de l’application appelle **SQLFreeHandle** avec ce handle d’environnement, **SQLFreeHandle** retourne SQLSTATE HY010 (erreur de séquence de fonction), car elle peut être appelée uniquement lorsque l’environnement est dans un état alloué. En définissant ce comme une transition d’état non valide, ODBC empêche l’application à partir de la libération de l’environnement s’il existe des connexions actives.  
  
 Des transitions d’état sont inhérentes à la conception d’ODBC. Par exemple, il n’est pas possible d’allouer un handle de connexion sans premier allouer un handle d’environnement, car la fonction qui alloue un handle de connexion requiert un handle d’environnement. Autres transitions d’état sont appliquées par le Gestionnaire de pilotes et les pilotes. Par exemple, **SQLExecute** exécute une instruction préparée. Si le handle d’instruction passé à il n’est pas dans un état préparé, **SQLExecute** retourne SQLSTATE HY010 (erreur de séquence de fonction).  
  
 Du point de vue de l’application, les transitions d’état sont généralement simples : transitions d’état autorisées ont tendance à aller de pair avec le flux d’une application bien écrite. Transitions d’état sont plus complexes pour le Gestionnaire de pilotes et les pilotes, car ils doivent suivre l’état de l’environnement, chaque connexion et chaque instruction. La plupart de ce travail est effectuée par le Gestionnaire de pilotes ; la plupart du travail qui doit être effectuée par les pilotes se produit avec les instructions avec des résultats en attente.  
  
 Les parties 1 et 2 de ce manuel (« Introduction to ODBC » et « Développement des Applications et des pilotes ») sont généralement ne pas mentionner explicitement les transitions d’état. Au lieu de cela, elles décrivent l’ordre dans lequel les fonctions doivent être appelées. Par exemple, « L’exécution des instructions » indique qu’une instruction doit être préparée avec **SQLPrepare** avant de pouvoir être exécuté avec **SQLExecute**. Pour obtenir une description complète des États et transitions d’état, ainsi que les transitions sont vérifiées par le Gestionnaire de pilotes qui doivent être vérifié par les pilotes, consultez [Tables de Transition d’état annexe b : ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).
