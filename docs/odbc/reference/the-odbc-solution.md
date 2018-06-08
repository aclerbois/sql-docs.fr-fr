---
title: La Solution ODBC | Documents Microsoft
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
- ODBC [ODBC], database access
- SQL [ODBC], database access
- database access [ODBC]
- standardizing database access [ODBC], using ODBC
ms.assetid: 34b80790-e010-4b90-8eaa-03189f5d8986
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a071ce8c5fc767b3cb7dc998562328e4a6b94d53
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-odbc-solution"></a>La Solution d’ODBC
Ensuite, la question est comment ODBC normaliser les accès de base de données ? Il existe deux spécifications d’architecture :  
  
-   Applications doivent être en mesure d’accéder à l’aide du même code source sans avoir à recompiler ou réédition de SGBD plusieurs.  
  
-   Applications doivent être en mesure d’accéder à plusieurs systèmes SGBD simultanément.  
  
 Et une question de plus, en raison d’une réalité de marketplace :  
  
-   Les fonctionnalités SGBD doit exposer ODBC ? Uniquement les fonctionnalités qui sont communes à tous les SGBD ou n’importe quelle fonctionnalité est disponible dans n’importe quel SGBD ?  
  
 ODBC résout ces problèmes de la manière suivante :  
  
-   **ODBC est une interface de niveau d’appel.** Pour résoudre le problème des applications comment accéder à plusieurs systèmes SGBD à l’aide du même code source, ODBC définit une interface CLI standard. Il contient toutes les fonctions dans les spécifications de l’interface CLI à partir d’Open Group et de la norme ISO/IEC et fournit des fonctions supplémentaires généralement requises par les applications.  
  
     Une autre bibliothèque, ou le pilote, est requis pour chaque SGBD qui prend en charge d’ODBC. Le pilote implémente les fonctions de l’API ODBC. Pour utiliser un autre pilote, l’application n’a pas besoin d’être recompilées ni lié à nouveau. Au lieu de cela, l’application charge le nouveau pilote simplement et appelle les fonctions qu’elle contient. Pour accéder à plusieurs systèmes SGBD simultanément, l’application charge plusieurs pilotes. Comment les pilotes sont prises en charge est spécifique au système d’exploitation. Par exemple, sur le système d’exploitation Microsoft® Windows®, les pilotes sont des bibliothèques de liens dynamiques (DLL).  
  
-   **ODBC définit une grammaire SQL standard.** Outre une interface de niveau d’appel standard, ODBC définit une grammaire SQL standard. Cette grammaire est basée sur la spécification de groupe SQL CAE ouvert. Différences entre les deux grammaires mineures et principalement en raison de différences entre la grammaire SQL requis par embedded SQL (Open Group) et une interface CLI (ODBC). Il existe également des extensions à la grammaire d’exposer des fonctionnalités de langage couramment disponible non couvertes par la grammaire Open Group.  
  
     Les applications peuvent envoyer des instructions à l’aide de la grammaire ODBC ou propres au SGBD. Si une instruction utilise la grammaire ODBC qui est différente de la grammaire des SGBD spécifiques, le pilote convertit avant de l’envoyer à la source de données. Toutefois, ces conversions sont rares, car la plupart des SGBD utilisent déjà la grammaire SQL standard.  
  
-   **ODBC fournit un gestionnaire de pilote pour gérer l’accès simultané à plusieurs systèmes SGBD.** Bien que l’utilisation des pilotes a résolu le problème d’accéder simultanément à plusieurs systèmes SGBD, le code peut être complexe. Les applications qui sont conçues pour fonctionner avec tous les pilotes ne peuvent pas être liées statiquement pour tous les pilotes. Au lieu de cela, ils doivent charger les pilotes en cours d’exécution et appeler les fonctions dans les via un tableau de pointeurs de fonction. La situation devient plus complexe si l’application utilise plusieurs pilotes simultanément.  
  
     Au lieu d’obliger chaque application pour ce faire, ODBC fournit un gestionnaire de pilotes. Le Gestionnaire de pilotes implémente toutes les fonctions ODBC — principalement comme des appels directs aux fonctions ODBC dans les pilotes et est statiquement liée à l’application ou chargées par l’application au moment de l’exécution. Par conséquent, l’application appelle les fonctions ODBC par nom dans le Gestionnaire de pilotes, plutôt que par pointeur dans chaque pilote.  
  
     Lorsqu’une application a besoin d’un pilote spécifique, il demande tout d’abord un handle de connexion permettant d’identifier le pilote, puis les demandes que le Gestionnaire de pilotes charger le pilote. Le Gestionnaire de pilotes charge le pilote et stocke l’adresse de chaque fonction dans le pilote. Pour appeler une fonction ODBC dans le pilote, l’application appelle cette fonction dans le Gestionnaire de pilotes et passe le handle de connexion pour le pilote. Le Gestionnaire de pilotes appelle ensuite la fonction à l’aide de l’adresse stockées précédemment.  
  
-   **ODBC expose un nombre important de fonctionnalités du SGBD, mais ne nécessite pas de prendre en charge tous les pilotes.** Si ODBC exposé uniquement les fonctionnalités qui sont communes à tous les SGBD, il serait de rares cas d’utilisation ; Après tout, afin de nombreux SGBD différent existe aujourd'hui parce qu’ils ont des fonctionnalités différentes. Si ODBC exposé toutes les fonctionnalités disponibles dans n’importe quel système SGBD, il est impossible pour les pilotes à mettre en œuvre.  
  
     Au lieu de cela, ODBC expose un nombre important de fonctionnalités : plusieurs sont prises en charge par la plupart des SGBD, mais nécessite des pilotes pour implémenter uniquement un sous-ensemble de ces fonctionnalités. Pilotes implémentent les fonctions restantes uniquement si elles sont prises en charge par le SGBD sous-jacent ou si elle choisit d’émuler les. Par conséquent, les applications peuvent être écrites pour exploiter les fonctionnalités d’un SGBD unique tel qu’exposé par le pilote pour ce système SGBD, utiliser uniquement les fonctionnalités utilisées par tous les SGBD, ou de vérifier la prise en charge d’une fonctionnalité particulière et réagir en conséquence.  
  
     Afin qu’une application peut déterminer quelles fonctionnalités d’un pilote et SGBD prennent en charge, ODBC fournit deux fonctions (**SQLGetInfo** et **SQLGetFunctions**) qui retournent des informations générales sur le pilote et les fonctionnalités du SGBD et une liste des fonctions le pilote prend en charge. ODBC définit également des API et SQL grammaire niveaux de conformité, qui spécifient les grandes plages de fonctionnalités prises en charge par le pilote. Pour plus d’informations, consultez [niveaux de conformité](../../odbc/reference/develop-app/conformance-levels.md).  
  
     Il est important de se rappeler qu’ODBC définit une interface commune pour toutes les fonctionnalités qu’il expose. Pour cette raison, les applications contient du code spécifique à la fonctionnalité, pas de code propres au SGBD et peuvent utiliser les pilotes qui exposent ces fonctionnalités. Sont de l’un des avantages de ce que les applications n’avez pas besoin d’être mis à jour lorsque les fonctionnalités prises en charge par un SGBD ont été améliorées ; au lieu de cela, lorsqu’un pilote mis à jour est installé, l’application utilise automatiquement les fonctionnalités, car son code spécifique aux fonctions ni spécifiques au pilote ni propres au SGBD.
