---
title: Limitations de l’utilisation de curseurs pilotés par jeu de clés | Documents Microsoft
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
- ODBC driver for Oracle [ODBC], cursors
- keyset-driven cursors [ODBC]
ms.assetid: 59d86fed-387c-4719-9550-36343e74da44
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a8ae5c5fbc73ff0128eb44944d5a0e5573c5b0d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="limitations-of-using-keyset-driven-cursors"></a>Limitations de l’utilisation de curseurs
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Vous devez être en mesure de récupérer une seule colonne ROWID pour la table interrogée. Un curseur keyset ne peut pas être utilisé sur les jointures, les requêtes ou les instructions qui contiennent DISTINCT, GROUP BY, UNION, INTERSECT ou moins clauses.  
  
 En outre, si votre application utilise des alias de table, les curseurs keyset ne fonctionnera pas. types de curseurs avant uniquement ou statiques sont requis. Le jeu de clés à l’aide de type de curseur avec un alias de table entraîne l’erreur suivante : « [Microsoft] [pilote ODBC pour Oracle] ne peut pas utiliser le curseur sur la jointure, avec union, intersect ou jeu de résultats moins ou sur l’accès en lecture seule. »  
  
> [!NOTE]  
>  En raison du mode le pilote traite l’instruction SQL qui est envoyée sur le serveur Oracle, Oracle retourne en interne le message d’erreur suivant : « ORA-00964 : table nom n’est pas dans à partir de la liste. »
