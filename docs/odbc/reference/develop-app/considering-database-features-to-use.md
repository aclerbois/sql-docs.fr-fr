---
title: Prise en compte les fonctionnalités de base de données à utiliser | Documents Microsoft
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
- interoperability [ODBC], database features
ms.assetid: 59760114-508e-46c5-81d2-8f2498c0d778
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 264acbea3c73679cf14e9459aea98c0a2a646b0a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="considering-database-features-to-use"></a>Prise en compte les fonctionnalités de base de données à utiliser
Une fois le niveau de base d’interopérabilité est connu, les fonctionnalités de base de données utilisées par l’application doivent être examinées. Par exemple, les instructions SQL que l’application exécute ? L’application va utiliser des curseurs de défilement ? Transactions ? Procédures ? Données de type long ? Pour les idées sur les fonctionnalités ne peuvent pas être pris en charge par tous les SGBD, consultez le [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md), [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), et [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md) une description, de fonction et [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md). Les fonctionnalités requises par une application peuvent éliminer certains SGBD à partir de la liste des cibles SGBD. Ils peuvent également afficher que l’application peut cibler facilement de nombreux SGBD.  
  
 Par exemple, si les fonctionnalités requises sont simples, elles peuvent être implémentées généralement avec un degré élevé d’interopérabilité. Une application qui exécute un simple **sélectionnez** instruction et récupère les résultats avec un curseur avant uniquement est susceptible d’être hautement interopérable en vertu de sa simplicité : presque tous les pilotes et SGBD prennent en charge les fonctionnalités dont il a besoin.  
  
 Toutefois, si les fonctionnalités requises sont plus complexes, tels que les curseurs de défilement, mise à jour positionnée instructions delete et des procédures, des compromis doivent souvent être effectuées. Il existe plusieurs possibilités :  
  
-   **Interopérabilité inférieure, davantage de fonctionnalités.** L’application comprend les fonctionnalités, mais fonctionne uniquement avec les SGBD qui les prennent en charge.  
  
-   **Interopérabilité plus élevée, moins de fonctionnalités.** L’application supprime les fonctionnalités, mais fonctionne avec plusieurs SGBD.  
  
-   **Interopérabilité plus élevée, les fonctionnalités facultatives.** L’application comprend les fonctionnalités, mais les rend disponibles uniquement avec les SGBD qui les prennent en charge.  
  
-   **Interopérabilité plus élevée, plus de fonctionnalités.** L’application utilise les fonctionnalités avec les SGBD qui les prennent en charge et les émule pour les SGBD qui ne le font pas.  
  
 Les deux premiers cas sont relativement simples à implémenter, car les fonctionnalités sont utilisées avec tous les SGBD pris en charge ou none. En revanche, les deux derniers cas, sont plus complexes. Il est nécessaire dans les deux cas, pour vérifier si le SGBD prend en charge les fonctionnalités et dans le dernier cas pour écrire une quantité volumineuse de code pour émuler ces fonctionnalités. Par conséquent, ces schémas sont susceptibles de nécessiter plus de temps de développement et peuvent être plus lentes au moment de l’exécution.  
  
 Considérez une application de requêtes générique qui peut se connecter à une source de données. L’application accepte une requête à partir de l’utilisateur et affiche les résultats dans une fenêtre. Maintenant que cette application possède une fonctionnalité qui permet aux utilisateurs d’afficher les résultats de plusieurs requêtes simultanément. Autrement dit, ils peuvent exécuter une requête Examinons quelques-unes des résultats, exécuter une requête différente et examiner certaines de ses résultats et puis revenez à la première requête. Cela pose un problème d’interopérabilité, car certains pilotes prennent en charge qu’une seule instruction active.  
  
 L’application possède un nombre de choix, en fonction de ce que le pilote retourne pour l’option SQL_MAX_CONCURRENT_ACTIVITIES **SQLGetInfo**:  
  
-   **Toujours prendre en charge plusieurs requêtes.** Après la connexion à un pilote, l’application vérifie le nombre d’instructions actives. Si le pilote prend en charge qu’une seule instruction active, l’application ferme la connexion et indique que le pilote ne prend pas en charge les fonctionnalités nécessaires à l’utilisateur. L’application est facile à implémenter et toutes les fonctionnalités, mais a l’interopérabilité inférieure.  
  
-   **Ne prend en charge de plusieurs requêtes.** L’application supprime la fonctionnalité complètement. Il est facile à implémenter et a une interopérabilité élevée mais moins de fonctionnalités.  
  
-   **Prend en charge plusieurs requêtes uniquement si le pilote ne.** Après la connexion à un pilote, l’application vérifie le nombre d’instructions actives. L’application permet à l’utilisateur démarrer une nouvelle instruction lorsque l’une est déjà active uniquement si le pilote prend en charge plusieurs instructions actives. L’application a des fonctionnalités plus élevées et interopérabilité, mais est plus difficile à implémenter.  
  
-   **Toujours prendre en charge plusieurs requêtes et émuler les lorsque cela est nécessaire.** Après la connexion à un pilote, l’application vérifie le nombre d’instructions actives. Toujours l’application permet à l’utilisateur démarrer une nouvelle instruction lorsque l’une est déjà active. Si le pilote prend en charge qu’une seule instruction active, l’application ouvre une connexion supplémentaire à ce pilote et exécute l’instruction de nouveau sur cette connexion. L’application a toutes les fonctionnalités et l’interopérabilité haute mais est plus difficile à implémenter.
