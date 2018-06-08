---
title: MSSQLSERVER_3937 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 3937 (Database Engine error)
ms.assetid: 312d5bbe-c8de-42db-af4b-4ccb448ce6ef
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 263747514a29b61f82b308078decbe2967c6fb6b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver3937"></a>MSSQLSERVER_3937
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|MSSQLSERVER|  
|ID d'événement|3937|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|XACT_FILESTREAM_ROLLBACK_ERROR|  
|Texte du message|Une erreur s'est produite en tentant d'avertir le pilote de filtre FILESTREAM qu'une transaction était restaurée. Code d'erreur : 0x%0x.|  
  
## <a name="explanation"></a>Explication  
Une erreur a été retournée par le pilote RsFx lors de la publication d'une notification de restauration pour une transaction. Cela est probablement dû à une insuffisance de ressources. Cela peut provoquer une petite fuite de mémoire dans le pilote de filtre RsFx, mais elle sera libérée lorsque le processus sqlservr.exe qui a créé la transaction se terminera.  
  
## <a name="user-action"></a>Action de l'utilisateur  
