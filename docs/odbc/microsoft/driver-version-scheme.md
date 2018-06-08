---
title: Schéma de Version de pilote | Documents Microsoft
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
- ODBC driver for Oracle [ODBC], versions
ms.assetid: e4a8d9d7-8aba-48ab-8be6-1a6129adfb8f
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f4a6e04da609034fbf0a3d0ba57bc5db2b7b7870
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-version-scheme"></a>Schéma de Version de pilote
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Le tableau suivant répertorie les versions du pilote Microsoft ODBC pour Oracle.  
  
|Version du pilote|Numéro de build|Historique des disponibilités|  
|--------------------|------------------|--------------------------|  
|1.0|2.00.6235|Visual C++ 4.2 et Visual Basic 5.0, Enterprise Edition|  
|2|2.73.7269|Visual Studio 97 et MDAC 1,5 a|  
|2.0 mis à jour|2.73.7283.01|IIS 4.0|  
|2.0 mis à jour|2.73.7283.03|MDAC 1.5b et 1.5 c|  
|2.0 mis à jour|2.73.7356|KIT DE DÉVELOPPEMENT LOGICIEL ODBC 3.5|  
|2.5|2.573.2927|Visual Studio 6.0 et MDAC 2.0|  
|2.5 mis à jour|2.573.3513|SQL Server 7.0<br /><br /> Le Service Pack 5 SQL Server 6.5|  
  
 Build 2.00.6235 (version 1) a été la première version du pilote Microsoft ODBC pour Oracle. Après la publication de la première version, une nouvelle convention d’affectation de noms a été arrêtée.  
  
 Par exemple, 2.73.7283.03 peut être divisé en composants distincts suivants :  
  
-   2 = le numéro de version.  
  
-   73 = la version du serveur Oracle pour lequel le pilote a été conçu.  
  
-   7283.03 = le numéro de version du pilote.  
  
> [!NOTE]  
>  Avec la version 2.573.2973, la convention d’affectation de noms a conduit à une certaine confusion 2.573 est une version antérieure à celle de 2,73 que chaque section du numéro de build doit être considérée comme individuellement. Le nombre 573 est supérieur à 73, par conséquent, il est une version plus récente. « 2.5 » indique également, numéro de version du pilote.
