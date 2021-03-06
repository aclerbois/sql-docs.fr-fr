---
title: "Sélectionnez pour les instructions de mise à jour de traitement | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: odbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], select for update statements
- select for update [ODBC]
- SQL statements [ODBC], cursor library
- cursor library [ODBC], select for update statements
- ODBC cursor library [ODBC], select for update statements
- cursor library [ODBC], statement processing
ms.assetid: 8d2e79a4-5daf-458e-a536-d8b6e588753e
caps.latest.revision: "8"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 4c065a3194e492d7f05bae4b857eda293aa523cd
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="processing-select-for-update-statements"></a>Sélectionnez pour les instructions de mise à jour de traitement
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Pour une interopérabilité maximale, les applications doivent générer des jeux de résultats qui sera mis à jour avec une instruction de mise à jour positionnée en exécutant un **sélectionner pour la mise à jour** instruction. Bien que la bibliothèque de curseurs ne requiert pas cela, il est requis par la plupart des sources de données qui prennent en charge les instructions de mise à jour positionnée.  
  
 La bibliothèque de curseurs ignore les colonnes dans le **pour la mise à jour** clause d’une **sélectionner pour la mise à jour** instruction ; il supprime cette clause avant de passer de l’instruction au pilote. Dans la bibliothèque de curseurs, l’attribut d’instruction SQL_ATTR_CONCURRENCY, ainsi que les restrictions mentionné dans la section précédente, les contrôles si les colonnes dans un résultat de la valeur peuvent être mis à jour.
