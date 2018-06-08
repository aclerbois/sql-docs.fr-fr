---
title: Applications | Documents Microsoft
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
- off-the-shelf applications [ODBC]
- ODBC architecture [ODBC], applications
- shrink-wrapped applications [ODBC]
- application development [ODBC], types
- custom applications [ODBC]
- virtual applications [ODBC]
- generic applications [ODBC]
ms.assetid: 39d6461f-0d24-4b7d-a723-843ade15ad73
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ac9d98b3b7f6261333626d888c6a8b1d750f2d1b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="applications"></a>Applications
Un *application* est un programme qui appelle l’API ODBC pour accéder aux données. Bien que de nombreux types d’applications sont possibles, la plupart sont classées en trois catégories, qui sont utilisés comme exemples dans ce guide.  
  
-   **Applications génériques** ceux-ci sont également appelés applications emballées ou prêtes à l’emploi. Applications génériques sont conçues pour fonctionner avec une variété de SGBD différents. Une feuille de calcul ou un package de statistiques qui utilise ODBC pour importer des données pour une analyse plus approfondie et un traitement de texte qui utilise ODBC pour obtenir une liste de distribution à partir d’une base de données sont des exemples.  
  
     Une sous-catégorie importante d’applications génériques est environnements de développement, tels que PowerBuilder ou Microsoft® Visual Basic®. Bien que les applications construites avec ces environnements probablement fonctionnent uniquement avec un SGBD unique, l’environnement lui-même doit fonctionner avec plusieurs SGBD.  
  
     Ce que toutes les applications génériques ont en commun est qu’ils sont très interopérables entre SGBD et ils doivent utiliser ODBC de manière relativement générique. Pour plus d’informations sur l’interopérabilité, consultez [en choisissant un niveau d’interopérabilité](../../odbc/reference/develop-app/choosing-a-level-of-interoperability.md).  
  
-   **Les Applications verticales** applications verticales effectuer un seul type de tâche, telle que l’entrée de commande ou le suivi des données de fabrication et travailler avec un schéma de base de données est contrôlé par le développeur de l’application. Pour un client particulier, l’application fonctionne avec un SGBD unique. Par exemple, une petite entreprise peut utiliser l’application avec dBase, pendant une grande entreprise peut l’utiliser avec Oracle.  
  
     L’application utilise ODBC de manière à ce que l’application n’est pas liée à n’importe quel un SGBD, bien qu’il peut être lié à un nombre limité de SGBD qui fournissent des fonctionnalités similaires. Par conséquent, le développeur d’applications peut vendre l’application indépendamment à partir de SGBD. Les applications verticales sont interopérables lorsqu’elles sont développées, mais sont parfois modifiées pour inclure du code noninteroperable une fois que le client a choisi un SGBD.  
  
-   **Applications personnalisées** Custom applications sont utilisées pour effectuer une tâche spécifique dans une entreprise unique. Par exemple, une application dans une grande entreprise peut collecter des données de ventes à partir de plusieurs divisions (chacun d’eux utilise un autre SGBD) et créer un rapport unique. ODBC est utilisée, car elle est une interface commune et les programmeurs qui évite d’avoir à apprendre plusieurs interfaces. Ces applications sont généralement pas interopérables et sont écrites dans le SGBD et des pilotes spécifiques.  
  
 Un nombre de tâches est commun à toutes les applications, quel que soit la manière dont ils utilisent ODBC. Dans leur ensemble, ils définissent en grande partie le flux d’une application ODBC. Les tâches sont :  
  
-   Sélection d’une source de données et la connexion à celui-ci.  
  
-   Soumission d’une instruction SQL pour l’exécution.  
  
-   La récupération des résultats (le cas échéant).  
  
-   Erreurs de traitement.  
  
-   Valider ou restaurer la transaction englobant de l’instruction SQL.  
  
-   Déconnexion de la source de données.  
  
 Étant donné que la plupart du travail accès aux données s’effectue avec SQL, la tâche principale pour les applications qui utilisent ODBC consiste à envoyer des instructions SQL et récupérer les résultats (le cas échéant) générées par ces instructions. Autres tâches pour les applications qui utilisent ODBC comprennent déterminer et de réglage pour les fonctionnalités du pilote et parcourent le catalogue de base de données.
