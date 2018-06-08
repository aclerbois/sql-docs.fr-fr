---
title: Quand utiliser les procédures | Documents Microsoft
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
- SQL statements [ODBC], procedures
- procedures [ODBC], about procedures
ms.assetid: 7dc9e327-dd54-4b10-9f66-9ef5c074f122
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dbc53507bf2cdf3333e0d36763ad7ecf7d359155
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="when-to-use-procedures"></a>Quand utiliser des procédures
Il existe de nombreux avantages à l’aide des procédures, tous les repose sur le fait qu’à l’aide de procédures déplace les instructions SQL à partir de l’application à la source de données. Ce qui est conservé dans l’application est un appel de procédure interopérable. Ces avantages :  
  
-   **Performances** procédures sont généralement plus rapide d’exécuter des instructions SQL. Comme préparé à l’exécution, l’instruction est compilée et exécutée en deux étapes distinctes. Contrairement à l’exécution préparée, les procédures sont exécutées uniquement au moment de l’exécution. Elles sont compilées à un autre moment.  
  
-   **Les règles d’entreprise** A *règle d’entreprise* est une règle sur la façon dont dans laquelle une société fait des affaires. Par exemple, seul un utilisateur avec le titre commercial peut autorisé pour ajouter de nouvelles commandes client. Permet de placer ces règles dans les procédures des entreprises de personnaliser des applications verticales par la réécriture de procédures appelées par l’application sans avoir à modifier le code d’application. Par exemple, une application de saisie de commandes peut appeler la procédure **InsertOrder** avec un nombre fixe de paramètres ; exactement comment **InsertOrder** est implémentée peut varier d’une entreprise à l’autre.  
  
-   **Replaceability** étroitement liées à placer des règles d’entreprise dans les procédures est le fait que les procédures peuvent être remplacés sans avoir à recompiler l’application. Si une règle d’entreprise est modifié après qu’une société a acheté et installé une application, la société peut modifier la procédure qui la contient. Point de vue de l’application, rien n’a été modifiée ; Il appelle toujours une procédure particulière pour accomplir une tâche particulière.  
  
-   **Propres au SGBD SQL** procédures fournissent un moyen aux applications exploiter propres au SGBD SQL et toujours être interopérable. Par exemple, une procédure, dans un SGBD qui prend en charge les instructions de flux de contrôle dans SQL peut intercepter et résoudre des erreurs, lors d’une procédure, dans un SGBD qui ne prend pas en charge les instructions de contrôle de flux peut simplement retourner une erreur.  
  
-   **Procédures survivent aux transactions** sur certaines sources de données, les plans d’accès pour les instructions préparées tout sur une connexion sont supprimés lors d’une transaction est validée ou restaurée. En plaçant des instructions SQL dans les procédures, qui sont stockés définitivement dans la source de données, les instructions après la transaction. Si les procédures qui survivent dans préparée partiellement préparé, soit d’un état non préparé propres au SGBD.  
  
-   **Séparation du développement** procédures peuvent être développés séparément du reste de l’application. Dans les grandes entreprises, cela peut constituer un moyen pour exploiter davantage les compétences de programmeurs hautement spécialisées. En d’autres termes, les programmeurs d’applications peuvent écrire du code de l’interface utilisateur et les programmeurs de base de données peuvent écrire des procédures.  
  
 Les procédures sont généralement utilisés par les applications verticales et personnalisées. Ces applications ont tendance à effectuer des tâches fixes, et il est possible d’appels de procédure de coder en dur dans les. Par exemple, une application d’entrée de commande peut appeler les procédures **InsertOrder**, **DeleteOrder**, **UpdateOrder**, et **GetOrders**.  
  
 Il est recommandé d’appeler des procédures à partir d’applications génériques. Les procédures sont généralement écrits pour effectuer une tâche dans le contexte d’une application particulière et par conséquent, n’ont aucune utilité pour les applications génériques. Par exemple, une feuille de calcul n’a aucune raison d’appeler le **InsertOrder** procédure précité. En outre, les applications génériques ne doivent pas construire de procédures en cours d’exécution période en fournissant l’exécution des instructions plus rapide ; est non seulement ce susceptibles d’être plus lente que l’exécution préparée ou directe, elle requiert également des instructions propres au SGBD SQL.  
  
 Une exception concerne les environnements de développement d’applications, et souvent permettent aux programmeurs de créer des instructions SQL qui exécutent des procédures et peuvent fournir un moyen pour les programmeurs de tester les procédures. Appel de ces environnements **SQLProcedures** aux procédures disponibles de liste et **SQLProcedureColumns** pour répertorier l’entrée, d’entrée/sortie et les paramètres de sortie, valeur de retour de la procédure et les colonnes de n’importe quel produit jeux créés par une procédure. Toutefois, ces procédures doivent être développées au préalable sur chaque source de données ; Cette opération requiert des instructions propres au SGBD SQL.  
  
 Il existe trois inconvénients majeurs à l’aide de procédures. La première est que les procédures doivent être écrits et compilés pour chaque SGBD avec lequel l’application doit s’exécuter. Bien que cela ne soit pas un problème pour les applications personnalisées, il peut augmenter considérablement développement et l’heure de maintenance pour les applications verticales conçus pour s’exécuter avec un nombre de SGBD.  
  
 Le deuxième inconvénient que nombreux SGBD ne prennent pas en charge les procédures. Là encore, il s’agit probablement d’un problème pour les applications verticales conçus pour s’exécuter avec un nombre de SGBD. Pour déterminer si les procédures sont prises en charge, une application appelle **SQLGetInfo** avec l’option SQL_PROCEDURES.  
  
 L’inconvénient de tiers, ce qui concerne particulièrement aux environnements de développement, est que ODBC ne définit pas une grammaire standard pour créer des procédures. Autrement dit, bien que les applications peuvent appeler des procédures plus, ils ne peuvent pas les créer plus.
