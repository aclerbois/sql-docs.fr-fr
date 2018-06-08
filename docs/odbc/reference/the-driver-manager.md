---
title: Le Gestionnaire de pilotes | Documents Microsoft
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
- driver manager [ODBC]
- ODBC architecture [ODBC], driver manager
- driver manager [ODBC], about driver manager
- ODBC driver manager [ODBC]
ms.assetid: 559e4de1-16c9-4998-94f5-6431122040cd
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d31dbfce3f5c5e3b09f43ec9a644e415b6d82d22
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-driver-manager"></a>Le Gestionnaire de pilotes
Le *du Gestionnaire de pilotes* est une bibliothèque qui gère la communication entre les applications et des pilotes. Par exemple, sur les plateformes Microsoft® Windows®, le Gestionnaire de pilotes est une bibliothèque de liens dynamiques (DLL) qui est écrit par Microsoft et peut être redistribuée par les utilisateurs du composant redistribuable MDAC 2.8 SP1 SDK.  
  
 Le Gestionnaire de pilotes existe principalement par souci de commodité pour les writers d’application et résout de nombreux problèmes communs à toutes les applications. Notamment déterminer quel pilote charger basé sur un nom de source de données, le chargement et déchargement de pilotes et appeler des fonctions dans les pilotes.  
  
 Pour afficher la raison pour laquelle ce dernier est un problème, envisagez ce qui se passerait si l’application appelée directement les fonctions dans le pilote. À moins que l’application a été liée directement à un pilote en particulier, cela aurait générer un tableau de pointeurs vers les fonctions dans le pilote et appeler ces fonctions en pointeur. Utilise le même code pour plus d’un pilote à la fois ajoute un autre niveau de complexité. L’application serait ont tout d’abord définir un pointeur de fonction pour pointer vers la fonction correcte dans le pilote approprié et ensuite appeler la fonction via ce pointeur.  
  
 Le Gestionnaire de pilotes résout ce problème en fournissant un emplacement unique pour appeler chaque fonction. L’application est liée aux fonctions ODBC Gestionnaire de pilotes et des appels dans le Gestionnaire de pilotes, pas le pilote. L’application identifie la source de données et le pilote cible avec un *handle de connexion*. Lors du chargement d’un pilote, le Gestionnaire de pilotes crée un tableau de pointeurs vers les fonctions dans le pilote. Il utilise le handle de connexion transmis par l’application pour trouver l’adresse de la fonction dans le pilote cible et appelle cette fonction par adresse.  
  
 Dans la plupart des cas, le Gestionnaire de pilotes transmet simplement les appels de fonction à partir de l’application pour le pilote approprié. Toutefois, il implémente également certaines fonctions (**SQLDataSources**, **SQLDrivers**, et **SQLGetFunctions**) et effectue une vérification d’erreur de base. Par exemple, le Gestionnaire de pilotes vérifie que les handles ne sont pas des pointeurs null, que les fonctions sont appelées dans l’ordre correct, ainsi que certains arguments de fonction sont valides. Pour obtenir une description complète des erreurs vérifiées par le Gestionnaire de pilotes, consultez la section de référence pour chaque fonction et [Tables de Transition d’état annexe b : ODBC](../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Le rôle principal final du Gestionnaire de pilotes est le chargement et déchargement de pilotes. L’application charge et décharge uniquement le Gestionnaire de pilotes. Quand il souhaite utiliser un pilote spécifique, il appelle une fonction de connexion (**SQLConnect**, **SQLDriverConnect**, ou **SQLBrowseConnect**) dans le Gestionnaire de pilotes et spécifie le nom de source de données particulière ou de pilote, tels que « Comptabilité » ou « SQL Server ». À l’aide de ce nom, le Gestionnaire de pilote recherche les informations de source de données pour le nom de fichier du pilote, telles que Sqlsrvr.dll. Il charge le pilote (en supposant qu’il n’est pas déjà chargé), stocke l’adresse de chaque fonction dans le pilote et appelle la fonction de connexion dans le pilote, puis s’initialise et se connecte à la source de données.  
  
 Lorsque l’application est terminée à l’aide du pilote, il appelle **SQLDisconnect** dans le Gestionnaire de pilotes. Le Gestionnaire de pilotes appelle cette fonction dans le pilote, se déconnecte de la source de données. Toutefois, le Gestionnaire de pilotes conserve le pilote en mémoire dans le cas où l’application se reconnecte à celui-ci. Il décharge le pilote uniquement lorsque l’application libère la connexion utilisée par le pilote utilise la connexion pour un autre pilote, et aucune autre connexion n’utilisent le pilote. Pour obtenir une description complète du rôle de gestionnaire de pilotes dans le chargement et déchargement de pilotes, consultez [rôle du Gestionnaire de pilotes dans le processus de connexion](../../odbc/reference/develop-app/driver-manager-s-role-in-the-connection-process.md).
