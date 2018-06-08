---
title: ODBC et l’interface CLI Standard | Documents Microsoft
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
- ODBC [ODBC], CLI
- CLI [ODBC]
- CLI [ODBC], about CLI
- call-level interface [ODBC]
- call-level interface [ODBC], about call-level interface
ms.assetid: 79b9c268-16ac-4b80-b451-f9dcd8c02ca4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2335d0eb5033ca6b32130503b4bd9a11f180c9c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-and-the-standard-cli"></a>ODBC et l’interface CLI Standard
ODBC est alignée avec les spécifications et les normes qui traitent avec l’Interface de niveau d’appel (CLI) suivantes. (Les fonctions ODBC sont un sur-ensemble de chacun de ces normes.)  
  
-   La spécification CAE Open Group « gestion des données : Interface de niveau d’appel SQL (CLI) »  
  
-   ISO/IEC 9075-3:1995 (E) Interface de niveau d’appel (SQL/CLI)  
  
 À la suite de cette association, les conditions suivantes sont remplies :  
  
-   Une application écrite aux spécifications de l’ISO CLI Open Group et fonctionne avec un ODBC 3. *x* pilote ou un pilote conforme aux normes lorsqu’il est compilé avec ODBC 3. *x* en-tête des fichiers et liés par ODBC 3. *x* bibliothèques, et quand il accède au pilote via l’ODBC 3. *x* du Gestionnaire de pilotes.  
  
-   Un pilote écrit dans les spécifications Open Group et ISO CLI fonctionne avec un ODBC 3 *.x* application ou une application conforme aux normes lorsqu’il est compilé avec ODBC 3 *.x* en-tête des fichiers et liés par ODBC 3 *.x* bibliothèques, et lorsque l’application accède au pilote via l’ODBC 3 *.x* du Gestionnaire de pilotes. (Pour plus d’informations, consultez [conforme aux normes des Applications et des pilotes](../../odbc/reference/develop-app/standards-compliant-applications-and-drivers.md).  
  
 Niveau de conformité de l’interface Core englobe toutes les fonctionnalités de l’ISO CLI et toutes les fonctionnalités obligatoires dans l’interface CLI de groupe ouvert. Fonctionnalités facultatives de l’interface CLI de groupe ouvert s’affichent dans les niveaux de conformité interface supérieurs. Étant donné que tous les ODBC 3. *x* pilotes sont requis pour prendre en charge les fonctionnalités de niveau de conformité de l’interface de Core, les conditions suivantes sont remplies :  
  
-   Une application ODBC 3. *x* pilote prend en charge toutes les fonctionnalités utilisées par une application conforme aux normes.  
  
-   Une application ODBC 3. *x* application en utilisant uniquement les fonctionnalités de ISO CLI et les fonctionnalités obligatoires de l’interface CLI de groupe ouvert fonctionnera avec n’importe quel pilote conforme aux normes.  
  
 Outre les spécifications d’interface au niveau de l’appel contenues dans les normes ISO/CEI et ouvrez groupe CLI, ODBC implémente les fonctionnalités suivantes. (Certaines de ces fonctionnalités existaient dans les versions d’ODBC avant ODBC 3. *x*.)  
  
-   Extractions multilignes par un appel de fonction unique  
  
-   Liaison à un tableau de paramètres  
  
-   Prise en charge des signets, y compris l’extraction par le signet, les signets de longueur variable et bulk update et delete en opérations de signet sur les lignes  
  
-   Liaison selon les lignes  
  
-   Les décalages de liaison  
  
-   Prise en charge pour les lots d’instructions SQL, dans une procédure stockée ou comme une séquence d’instructions SQL exécutées via **SQLExecute** ou **SQLExecDirect**  
  
-   Le nombre de lignes du curseur exact ou approximatif  
  
-   Positionné de mise à jour et suppressions et mises à jour par lot et supprime par appel de fonction (**SQLSetPos**)  
  
-   Fonctions de catalogue qui extraient des informations à partir du schéma d’informations sans avoir besoin pour prendre en charge les vues de schémas d’informations  
  
-   Séquences d’échappement pour les jointures externes, les fonctions scalaires, les littéraux datetime, les littéraux de l’intervalle et les procédures stockées  
  
-   Bibliothèques de traduction de page de codes  
  
-   Création de rapports de niveau de conformité de l’ANSI et la prise en charge SQL d’un pilote  
  
-   À la demande le remplissage automatique de descripteur de paramètre d’implémentation  
  
-   Diagnostics améliorés et les tableaux de statut de ligne et de paramètre  
  
-   DateTime, intervalle, numérique/décimal, entier 64 bits en mémoire tampon types d’applications et  
  
-   Exécution asynchrone  
  
-   Prise en charge de la procédure stockée, y compris les séquences d’échappement, les mécanismes de liaison de paramètre de sortie et des fonctions de catalogue  
  
-   Améliorations de la connexion, y compris la prise en charge pour les attributs de connexion et de navigation de l’attribut
