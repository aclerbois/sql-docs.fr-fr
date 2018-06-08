---
title: À l’aide d’Applications 16 bits et 32 bits avec les pilotes 32 bits | Documents Microsoft
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
- ODBC drivers [ODBC], 32-bit applications
- 32-bit applications with 32-bit drivers [ODBC]
- 16-bit applications with 32-bit drivers [ODBC]
ms.assetid: fc65c988-b31f-4cc9-851f-30d2119604fd
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 97f4224967cd0c54b134d118533ed66f6043762c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-16-bit-and-32-bit-applications-with-32-bit-drivers"></a>À l’aide d’Applications 16 bits et 32 bits avec les pilotes 32 bits
> [!IMPORTANT]  
>  prise en charge les applications 16 bits sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Permet de développer des applications 32 bits ou 64 bits à la place.  
  
 Avec le composant d’accès aux données ODBC, vous pouvez utiliser les applications 16 bits et 32 bits avec les pilotes 32 bits. Systèmes d’exploitation Microsoft® Windows® 95/98 et Microsoft Windows et Windows 2000 prennent en charge les combinaisons suivantes des applications et des pilotes :  
  
-   applications 16 bits avec les pilotes 32 bits  
  
-   applications 32 bits avec les pilotes 32 bits  
  
 À l’aide d’une application 32 bits avec un pilote de 16 bits n’est pas pris en charge.  
  
> [!NOTE]  
>  À partir de la version d’ODBC version 3.0, Windows NT 4.0 a été pris en charge.  
  
 ODBC inclut les composants ODBC nécessaires pour prendre en charge des configurations ci-dessus par « thunking » bibliothèques de liens dynamiques (DLL) pour convertir les adresses de 16 bits pour les adresses 32 bits et vice versa. Le programme d’installation détermine quel système d’exploitation, vous utilisez et installe les composants ODBC requis par ce système. Vous pouvez également choisir d’installer les composants ODBC utilisés par tous les systèmes.  
  
 Dans la plupart des cas, portage d’une application ou le pilote à partir de 16 bits en 32 bits implique cinq types de modifications :  
  
-   Modifications apportées au code de gestion des messages  
  
-   Modifie les entiers et étant handles 32 bits  
  
-   Modifications dans les appels aux interfaces de programmation d’application Windows (API)  
  
-   Modifications pour rendre le pilote thread-safe.  
  
-   Modifications apportées aux composants ODBC  
  
 À partir d’une application ou le pilote programmation du point de vue, la principale différence entre les composants ODBC 16 bits et 32 bits est qu’ils ont des noms de fichiers différents. À partir d’un point de vue système, l’architecture de chaque connexion de l’application ou le pilote est différente et les outils utilisés pour gérer les sources de données sont différents.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Utilisation d’applications 16 bits avec des pilotes 32 bits](../../odbc/microsoft/using-16-bit-applications-with-32-bit-drivers.md)  
  
-   [Utilisation d’applications 32 bits avec des pilotes 32 bits](../../odbc/microsoft/using-32-bit-applications-with-32-bit-drivers.md)
