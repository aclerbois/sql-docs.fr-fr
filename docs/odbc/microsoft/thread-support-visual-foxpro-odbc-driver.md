---
title: Thread de prise en charge (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- thread support [ODBC]
- Visual FoxPro ODBC driver [ODBC], thread support
- FoxPro ODBC driver [ODBC], thread support
- multithreaded applications [ODBC]
ms.assetid: 0c6abbbc-012b-41aa-bded-5e7e362d015b
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 75adc2f72071765dc705d34b2bdd577316ae1acf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="thread-support-visual-foxpro-odbc-driver"></a>Prise en charge des threads (Visual FoxPro le pilote ODBC)
Le pilote ODBC Visual FoxPro est thread-safe. Accès aux handles d’environnement (*rsque*), les handles de connexion (*pas*) et les descripteurs d’instruction (*hstmt*) sont encapsulées dans des sémaphores appropriées pour empêcher les autres processus d’accéder à et potentiellement modifier les structures de données internes du pilote.  
  
 Dans une application multithread, vous pouvez annuler une fonction qui s’exécute de façon synchrone sur un *hstmt* en appelant [SQLCancel](../../odbc/microsoft/sqlcancel-visual-foxpro-odbc-driver.md) sur un thread distinct.  
  
 Le pilote utilise un thread séparé pour extraire des données lorsque vous utilisez l’extraction de progressif. Pour utiliser l’extraction progressive pour une source de données, sélectionnez le **extraire les données en arrière-plan** case à cocher sur la [boîte de dialogue d’installation de Visual FoxPro ODBC](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) ou utilisez le mot clé d’attribut BackgroundFetch dans votre chaîne de connexion. Évitez d’utiliser l’extraction en arrière-plan lorsque vous appelez le pilote à partir d’applications multithreads. Pour plus d’informations sur les mots clés attribut de chaîne de connexion, consultez [à l’aide de chaînes de connexion](../../odbc/microsoft/using-connection-strings.md).  
  
 Pour plus d’informations sur les threads et **SQLCancel**, consultez [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md) dans les *de référence du programmeur ODBC*.
