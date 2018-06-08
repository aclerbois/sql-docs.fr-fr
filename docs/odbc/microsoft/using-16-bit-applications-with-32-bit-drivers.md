---
title: À l’aide d’Applications 16 bits avec les pilotes 32 bits | Documents Microsoft
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
- ODBC drivers [ODBC], 16-bit applications
- 16-bit applications with 32-bit drivers [ODBC]
ms.assetid: 68feb3b7-c01a-4f42-8df9-f9c182d89325
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f8be9b55137ebbfd0da4a4194b3f29ea57fb89d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-16-bit-applications-with-32-bit-drivers"></a>À l’aide d’Applications 16 bits avec les pilotes 32 bits
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez plutôt le Gestionnaire de pilote 32 bits ou 64 bits.  
  
 Vous pouvez exécuter des applications 16 bits avec les pilotes 32 bits sur votre système Windows tant que le pilote 32 bits n’appelle pas explicitement les fonctions API Win32 qui créent des threads. Le sous-système Windows on Windows (WOW) exécute les applications en mode 16 bits et résout les appels de 16 bits dans le système d’exploitation. ODBC thunking DLL résoudre les appels de 16 bits à partir de l’application pour les pilotes 32 bits. Les applications 16 bits utilisent l’API Windows et les pilotes 32 bits utilisent l’API Win32.  
  
## <a name="architecture"></a>Architecture  
 L’illustration suivante montre comment aux applications 16 bits communiquent avec les pilotes 32 bits. Entre le Gestionnaire de pilotes 16 bits et les pilotes 32 bits sont génériques thunking DLL qui convertissent les appels ODBC 16 bits en appels ODBC 32 bits.  
  
 ![Comment 16&#45;applications bits communiquent avec 32&#45;bit pilotes](../../odbc/microsoft/media/sdka2.gif "sdka2")  
  
> [!NOTE]  
>  Chaque fois qu’une application 16 bits interagit avec un pilote 32 bits, le Gestionnaire de pilotes 32 bits retourne toujours « 2.0 » en tant que version d’ODBC pris en charge par le pilote.  
  
## <a name="administration"></a>Administration  
 Vous pouvez gérer les sources de données pour les pilotes 32 bits à l’aide de l’administrateur de Source de données ODBC. Pour ouvrir l’administrateur ODBC sur des ordinateurs exécutant Microsoft® Windows® 2000, ouvrez le panneau de configuration Windows, double-cliquez sur **outils d’administration**, puis double-cliquez sur **des Sources de données (ODBC)**. Sur les ordinateurs exécutant des versions antérieures de Microsoft Windows, l’icône est nommé **ODBC 32 bits** ou simplement **ODBC**.  
  
 L’illustration suivante montre comment une application 16 bits appelle une DLL d’installation du pilote 32 bits. Entre le programme d’installation de 16 bits DLL et le pilote 32 bits DLL d’installation est une générique thunking DLL qui convertit les appels de DLL de programme d’installation de 16 bits aux appels de DLL de programme d’installation 32 bits.  
  
 ![Comment un 16&#45;bit application appelle une 32&#45;bits d’installation du pilote DLL](../../odbc/microsoft/media/sdka3.gif "sdka3")  
  
 Dans Windows on Windows (thunking de 16 bits en 32 bits), une DLL de conversion supplémentaire nommée convertit Ds32gt.dll valeurs d’argument de 16 bits transmis via un programme d’installation 32 bits DLL sauvegarder sur 16 bits.  
  
## <a name="components"></a>Components  
 Le composant ODBC du kit SDK MDAC 2.8 SP1 inclut les fichiers suivants pour l’exécution d’applications 16 bits avec les pilotes 32 bits. Ces composants sont dans le répertoire \Redist.  
  
|Nom de fichier| Description|  
|---------------|-----------------|  
|Odbc16gt.dll|16 bits générique thunking DLL ODBC|  
|ODBC32GT.dll|32 bits générique thunking DLL ODBC|  
|Odbccp32.dll|programme d’installation 32 bits DLL|  
|Odbcad32.exe|programme administrateur de 32 bits|  
|ODBCINST.hlp|Fichier d’aide du programme d’installation|  
|Ds16gt.dll|programme d’installation 16 bits pilote générique thunking DLL|  
|Ctl3d32.dll|bibliothèque de style de fenêtre à trois dimensions de 32 bits|  
  
 En outre, les fichiers suivants, ainsi que le Gestionnaire de pilote 2.10 de ODBC 16 bits, qui ne font pas partie d’ODBC 3.51, sont requis par et doivent être installés avec l’application 16 bits.  
  
|Nom de fichier| Description|  
|---------------|-----------------|  
|ODBC.dll|Gestionnaire de pilotes 16 bits|  
|ODBCINST.dll|programme d’installation de 16 bits DLL|  
|Odbcadm.exe|programme de l’administrateur ODBC 16 bits|
