---
title: Types de verrous | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- AdLockBatchOptimistic [ADO]
- AdLockReadOnly [ADO]
- AdLockUnspecified [ADO]
- locks [ADO], types
- AdLockOptimistic [ADO]
- AdLockPessimistic [ADO]
ms.assetid: 12a978c0-b8a0-4ef0-87f0-a43c13659272
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2f20cb8bb9344b107a32f29aa2a9a9f83206ccaf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-locks"></a>Types de verrous
## <a name="adlockbatchoptimistic"></a>adLockBatchOptimistic  
 Indique les mises à jour par lot optimiste. Requis pour le mode de mise à jour par lots.  
  
 De nombreuses applications extraire un nombre de lignes à la fois et vous devez ensuite effectuer les mises à jour de coordonnées qui incluent l’ensemble de lignes à être insérés, mis à jour ou supprimées. Avec les curseurs de traitement par lots, qu’un seul aller-retour au serveur est nécessaire, améliorant ainsi les performances de mise à jour et de réduire le trafic réseau. À l’aide d’une bibliothèque de curseurs par lots, vous pouvez créer un curseur statique et puis vous déconnecter de la source de données. À ce stade vous pouvez apporter des modifications aux lignes par la suite vous reconnecter et valider les modifications apportées à la source de données dans un lot.  
  
## <a name="adlockoptimistic"></a>adLockOptimistic  
 Indique que le fournisseur utilise le verrouillage optimiste : verrouillage d’enregistrements que lorsque vous appelez le **mise à jour** (méthode). Cela signifie qu’il est probable qu’un autre utilisateur peut modifier les données entre le moment où vous modifiez l’enregistrement et lorsque vous appelez **mise à jour**, ce qui crée des conflits. Utilisez ce type de verrou dans les cas où le risque d’une collision est faible ou où les collisions peuvent être facilement résolues.  
  
## <a name="adlockpessimistic"></a>adLockPessimistic  
 Indique un verrouillage pessimiste, enregistrement par enregistrement. Le fournisseur effectue ce qui est nécessaire pour garantir une modification réussie des enregistrements, généralement par un verrouillage d’enregistrements à la source de données immédiatement avant la modification. Bien entendu, cela signifie que les enregistrements ne sont pas disponibles à d’autres utilisateurs une fois que vous commencez à modifier, jusqu'à ce que vous libériez le verrou en appelant **mise à jour.** Utilisez ce type de verrou dans un système où vous devez impérativement a des modifications simultanées des données, comme dans un système de réservation.  
  
## <a name="adlockreadonly"></a>adLockReadOnly  
 Indique les enregistrements en lecture seule. Vous ne pouvez pas modifier les données. Un verrou en lecture seule est le type « rapide » du verrou, car il ne nécessite pas le serveur pour maintenir un verrouillage des enregistrements.  
  
## <a name="adlockunspecified"></a>adLockUnspecified  
 Ne spécifie pas un type de verrou.
