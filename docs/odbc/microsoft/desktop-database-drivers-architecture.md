---
title: Architecture des pilotes de bureau de base de données | Documents Microsoft
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
- Jet-based ODBC drivers [ODBC], architecture
- ODBC desktop database drivers [ODBC], architecture
- desktop database drivers [ODBC], architecture
ms.assetid: 8b4d13f7-ab37-40b4-a9c6-145e7385352f
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b54a98aea619949ab51d20dd599fdc0fe3e71321
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="desktop-database-drivers-architecture"></a>Architecture des pilotes de bureau de base de données
Ces pilotes sont conçues pour utiliser Microsoft Windows 95 ou version ultérieure, ou Windows NT 4.0 et Windows 2000. Seules les applications 32 bits sont pris en charge sur Windows 95 ou version ultérieure ; les applications 16 bits et 32 bits sont pris en charge sur Windows NT 4.0 et Windows 2000.  
  
> [!NOTE]  
>  Pour plus d’informations sur la version d’ODBC à utiliser avec ces pilotes, reportez-vous à la *de référence du programmeur ODBC*et les notes de publication antérieurs et en cours. À l’exception des zones indiquées, ces pilotes sont conformes à la *de référence du programmeur ODBC*.  
  
 Les pilotes de base de données ODBC Desktop incluent les pilotes 32 bits pour Microsoft Access, dBASE, Microsoft Excel, Paradox et texte. N° 16 bits pilotes sont inclus. (Un pilote pour Microsoft FoxPro est disponible séparément.)  
  
 Est de l’architecture application/pilote sur Windows 95 ou version ultérieure :  
  
 ![Application&#47;architecture du pilote : Windows 95 et versions ultérieures](../../odbc/microsoft/media/odbcjetarch1.gif "ODBCJetArch1")  
  
 L’utilisation de ces pilotes par les applications 16 bits sur Windows 95 n’est pas pris en charge.  
  
 Est de l’architecture application/pilote sur Windows NT 4.0 et Windows 2000 :  
  
 ![Application&#47;architecture du pilote : NT 4.0 et Windows 2000](../../odbc/microsoft/media/odbcjetarch2.gif "ODBCJetArch2")  
  
 Les pilotes de base de données de bureau sont à deux niveaux. Dans une configuration à deux niveaux, le pilote n’effectue pas le processus de l’analyse, validation, optimisation et l’exécution de la requête. Au lieu de cela, Microsoft Jet effectue ces tâches. Il traite les appels d’API ODBC et agit comme un moteur SQL. Microsoft Jet fait désormais partie intégrante, non séparable des pilotes : il est fourni avec les pilotes et réside avec les pilotes, même si aucune autre application sur l’ordinateur n’utilise.  
  
 Les pilotes de base de données de bureau se composent de six différents pilotes, ou, plus précisément, un pilote de fichier (Odbcjt32.dll) qui ODBC [du Gestionnaire de pilotes](../../odbc/reference/the-driver-manager.md) utilise de six différentes façons. L’indicateur DRIVERID dans l’entrée de Registre pour une source de données détermine le pilote dans Odbcjt32.dll le Gestionnaire de pilotes utilise. Une application transmet cet indicateur dans la chaîne de connexion incluse dans un appel à **SQLDriverConnect**. Par défaut, l’indicateur est l’ID du pilote Microsoft Access.  
  
 Le fichier d’installation du pilote change l’indicateur DRIVERID au moment de l’installation. Tous les pilotes sauf le pilote Microsoft Access ont une DLL d’installation associé. Lorsque vous cliquez sur **le programme d’installation** dans les [administrateur de Source de données ODBC Microsoft](../../odbc/admin/odbc-data-source-administrator.md) pour une source de données, le programme d’installation ODBC DLL (Odbcinst.dll) charge de la DLL d’installation. Le programme d’installation DLL exporte la fonction du programme d’installation ODBC **SQLConfigDataSource**. Si un handle de fenêtre est passé à **SQLConfigDataSource**, cette fonction affiche une fenêtre de configuration et change l’indicateur DRIVERID selon le pilote sélectionné à partir de l’interface utilisateur.  
  
 Lorsqu’un fichier est créé par programme, un handle de fenêtre NULL est passé à **SQLConfigDataSource**, et la fonction crée une source de données de façon dynamique, la modification de l’indicateur DRIVERID conformément à la *lpszDriver* argument dans l’appel de fonction.  
  
 Odbcjt32.dll implémente des fonctions API Microsoft Jet ODBC. Il n’est cependant pas de mappage direct entre les fonctions ODBC et Microsoft Jet. De nombreux facteurs, tels que les modèles de curseurs et le mappage de SQL, empêchent une corrélation directe des fonctions.  
  
 Le pilote ODBC se trouve entre le moteur Microsoft Jet et le Gestionnaire de pilotes ODBC. Certaines fonctions ODBC appelées par une application sont gérées par le Gestionnaire de pilotes et pas passées au pilote. Pour ces fonctions, Microsoft Jet voit jamais la fonction appeler car il ne dispose pas d’une connexion directe au Gestionnaire de pilote.
