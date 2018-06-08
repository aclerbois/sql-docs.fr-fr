---
title: Configurations matérielle et logicielle requises (ODBC) | Documents Microsoft
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
- hardware requirements [ODBC], desktop database drivers
- software requirements [ODBC], desktop database drivers
- system requirements [ODBC], desktop database drivers
- requirements [ODBC], desktop database drivers
ms.assetid: 6df2e9cd-de10-4629-97bd-32f2782616c7
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 389492c377105614c60c127041354786cabbd5ba
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hardware-and-software-requirements-odbc"></a>Configurations matérielle et logicielle requises (ODBC)
Cette rubrique répertorie les conditions requises pour utiliser les pilotes de base de données ODBC Desktop.  
  
## <a name="hardware-requirements"></a>Configuration matérielle  
 Pour utiliser les pilotes de base de données ODBC Desktop, vous devez :  
  
-   Un ordinateur compatible IBM.  
  
-   Un disque dur avec 6 Mo d’espace disque libre.  
  
-   Au moins 16 Mo de mémoire vive (RAM).  
  
## <a name="software-requirements"></a>Configuration logicielle  
 Pour accéder aux données avec un pilote ODBC, vous devez avoir :  
  
-   Le pilote ODBC.  
  
-   32 bits Gestionnaire de pilotes ODBC, version 3.51 ou version ultérieure (Odbc32.dll).  
  
-   Microsoft Windows 95 ou version ultérieure, ou Windows NT 4.0 ou Windows 2000.  
  
-   Une taille de la pile d’au moins 20 Ko pour une application à l’aide d’un pilote ODBC de Microsoft.  
  
 Lorsque vous utilisez Microsoft Windows NT 4.0 ou Windows 2000, le pilote 32 bits est thread-safe, mais uniquement via l’utilisation d’un sémaphore global qui contrôle l’accès au pilote. Utilisation simultanée du pilote est très limitée sous Windows NT. Tous les accès à la couche ISAM Jet sera monothread pour toutes les applications utilisant le moteur Microsoft Jet.  
  
 Lorsque vous exécutez plusieurs applications 16 bits sur Windows on Windows (WOW) sur Microsoft Windows NT 4.0, les applications doivent être exécutées dans des espaces mémoire distincts. (Le même espace mémoire ne peut pas être utilisé car ODBC ne prend pas en charge plusieurs environnements dans le même processus.) Pour exécuter une application dans un espace mémoire distinct, sélectionnez icône de l’application dans le Gestionnaire de programme, ouvrez le **fichier** menu et cliquez sur **propriétés**, puis cliquez sur **exécuter dans des zones mémoires**.  
  
 L’utilisation de ces pilotes par les applications 16 bits sur Windows 95 n’est pas pris en charge.  
  
## <a name="driver-specific-hardware-and-software-requirements"></a>Spécifiques au pilote matérielle et logicielle requise  
  
-   Les dBASEdrivers MicrosoftAccess peuvent nécessiter des modifications dans les fichiers Autoexec.bat ou Config.sys.
