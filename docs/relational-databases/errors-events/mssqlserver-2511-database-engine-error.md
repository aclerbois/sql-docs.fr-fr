---
title: MSSQLSERVER_2511 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 2511 (Database Engine error)
ms.assetid: 9a00c0ed-eb4b-4fae-8016-192396006c37
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 3954cadaa9f9a9bdd847da05b15accb59ebbece7
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver2511"></a>MSSQLSERVER_2511
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|2511|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC_KEYS_OUT_OF_ORDER|  
|Texte du message|Erreur de table : ID d'objet %d, ID d'index %d, ID de partition %I64d, ID d'unité d'allocation %I64d (type %.*ls). Les clés sont désordonnées sur la page %S_PGID, slots %d et %d.|  
  
## <a name="explanation"></a>Explication  
Des clés désordonnées ont été détectées dans l'index spécifié. La page qui contient ces clés peut être endommagée.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Reconstruisez l'index spécifié en utilisant l'instruction ALTER INDEX REBUILD.  
  
