---
title: État de la ligne | Documents Microsoft
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
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- row status [ODBC]
- cache [ODBC]
ms.assetid: 0f0b1fb6-f697-4ced-811c-2908e210bc71
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba4f36ad63ee5e7d9fada29d444e8cc36eca1bcf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="row-status"></a>État de la ligne
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 La bibliothèque de curseurs crée une mémoire tampon dans le cache pour l’état de la ligne. La bibliothèque de curseurs récupère les valeurs pour le tableau d’état de ligne (spécifié avec l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR) à partir de cette mémoire tampon. Pour chaque ligne, la bibliothèque de curseurs définit cette mémoire tampon à :  
  
-   SQL_ROW_DELETED lorsqu’il exécute un positionnée delete instruction sur la ligne.  
  
-   SQL_ROW_ERROR quand il rencontre une erreur durant la récupération de la ligne à partir de la source de données avec **SQLFetch**.  
  
-   SQL_ROW_SUCCESS lorsqu’il extrait avec succès la ligne à partir de la source de données avec **SQLFetch**.  
  
-   SQL_ROW_UPDATED lorsqu’il exécute une instruction de mise à jour positionnée sur la ligne.
