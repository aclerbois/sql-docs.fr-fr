---
title: Architecture du pilote | Documents Microsoft
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
- ODBC architecture [ODBC], drivers
- drivers [ODBC], architecture
ms.assetid: c5003413-0cc1-4f41-b877-a64e2f5ab118
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d1fee82278af1597eefaf0e17ea3b9aa0dfce8e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-architecture"></a>Architecture du pilote
Architecture du pilote se divise en deux catégories, selon les instructions SQL de processus logiciel :  
  
-   **Pilotes basés sur le fichier** le pilote accède directement aux données physiques. Dans ce cas, le pilote joue le pilote et de source de données ; Autrement dit, il traite les appels ODBC et des instructions SQL. Par exemple, dBASE pilotes sont basés, car le fichier dBASE ne fournit pas de qu'un moteur de base de données autonome le pilote peut utiliser. Il est important de noter que les développeurs de pilotes basés sur le fichier doivent écrire leurs propres moteurs de base de données.  
  
-   **Pilotes basés sur SGBD** le pilote accède aux données physiques via un moteur de base de données distincte. Dans ce cas le pilote traite uniquement les appels ODBC ; Il passe les instructions SQL au moteur de base de données pour le traitement. Par exemple, Oracle pilotes sont basés sur SGBD, car Oracle possède un moteur de base de données autonome que le pilote utilise. Où se trouve le moteur de base de données est immatériel. Il peut résider sur le même ordinateur que le pilote ou un autre ordinateur sur le réseau ; Il peut même être accessible via une passerelle.  
  
 Architecture du pilote est généralement utile pour les rédacteurs de pilotes ; Autrement dit, architecture du pilote ne rend généralement aucune différence à l’application. Toutefois, l’architecture peut affecter si une application peut utiliser propres au SGBD SQL. Par exemple, Microsoft Access fournit un moteur de base de données autonome. Si un pilote Microsoft Access est basés sur SGBD, il accède aux données via ce moteur, l’application peut passer les instructions SQL à Microsoft Access pour le moteur pour le traitement.  
  
 Toutefois, si le pilote est basée sur le fichier, autrement dit, elle contient un moteur de propriétaire qui accède directement à du fichier .mdb de Microsoft® Access, toute tentative de transmettre des instructions spécifiques à Microsoft Access SQL au moteur est susceptibles d’entraîner des erreurs de syntaxe. La raison est que le moteur propriétaire est probablement que ODBC SQL.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Pilotes basés sur des fichiers](../../odbc/reference/file-based-drivers.md)  
  
-   [Pilotes basés sur le SGDB](../../odbc/reference/dbms-based-drivers.md)  
  
-   [Exemple de réseau](../../odbc/reference/network-example.md)  
  
-   [Autres architectures de pilote](../../odbc/reference/other-driver-architectures.md)
