---
title: Accès de la base de données réseau | Documents Microsoft
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
- network database access [ODBC]
- standardizing database access [ODBC], network
ms.assetid: f31dd938-e992-436b-b613-145c23973064
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 39e49ab6e5aba7852968d9ee22ffc4873891be08
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="network-database-access"></a>Réseau de base de données Access
L’accès à une base de données sur un réseau requiert un nombre de composants, chacun d’eux est indépendante d’et se trouve en dessous de l’interface de programmation. Ces composants sont illustrés dans l’illustration suivante.  
  
 ![Composants pour accéder à une base de données sur un réseau](../../odbc/reference/media/pr04.gif "pr04")  
  
 Une description détaillée de chaque composant suit :  
  
-   **Interface de programmation** comme décrit précédemment dans cette section, l’interface de programmation contient les appels effectués par l’application. Ces interfaces (embedded SQL, SQL modules et des interfaces de niveau d’appel) sont généralement propres à chaque SGBD, même si elles sont généralement basées sur une norme ANSI ou ISO.  
  
-   **Protocole de flux de données** le protocole de flux de données décrit le flux de données transférées entre le SGBD et de son client. Par exemple, le protocole peut nécessiter le premier octet pour décrire ce que contient le reste du flux de données : une instruction SQL exécutée, une valeur d’erreur retourné, ou a retourné des données. Le format du reste des données dans le flux de données dépendent ensuite cet indicateur. Par exemple, un flux d’erreurs peut contenir l’indicateur, un code d’erreur entier de 2 octets, une longueur de message d’erreur entier de 2 octets et un message d’erreur.  
  
     Le protocole de flux de données est un protocole logique et est indépendant des protocoles utilisés par le réseau sous-jacent. Par conséquent, un protocole de flux de données unique peut généralement être utilisé sur un nombre de différents réseaux. Protocoles de flux de données sont généralement propriétaires et ont été optimisés pour fonctionner avec un SGBD particulier.  
  
-   **Mécanisme de Communication d’interprocessus** le mécanisme de communication interprocessus (IPC) est le processus par lequel un processus communique avec un autre. Exemples DECnet sockets, sockets TCP/IP et canaux nommés. Le choix du mécanisme IPC est limité par le système d’exploitation et le réseau utilisé.  
  
-   **Protocole réseau** le protocole de réseau est utilisé pour transporter le flux de données sur un réseau. Il peut être considéré comme la structure que prend en charge les mécanismes IPC permettant d’implémenter les données de flux du protocole, ainsi que les opérations de réseau de base telles que les transferts de fichiers de prise en charge et de partage d’impression. Protocoles réseau incluent NetBEUI, TCP/IP, DECnet et SPX/IPX et sont spécifiques à chaque réseau.
