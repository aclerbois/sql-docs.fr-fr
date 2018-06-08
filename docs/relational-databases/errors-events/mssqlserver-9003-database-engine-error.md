---
title: MSSQLSERVER_9003 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 9003 (Database Engine error)
ms.assetid: 7fdfb391-5c6f-428b-b434-6c3d0b30fd7b
caps.latest.revision: 15
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d8d9266f4867918893af9f1298fbb3a82f8e8635
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver9003"></a>MSSQLSERVER_9003
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|9003|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LOG_INVALID_LSN|  
|Texte du message|Le numéro d'analyse du journal % S_LSN transmis à l'analyse du journal dans la base de données '%.*ls' n'est pas valide. Cette erreur peut indiquer les données sont endommagées ou que le fichier journal (.ldf) ne correspond pas au fichier de données (.mdf). Si cette erreur s'est produite pendant la réplication, recréez la publication. Sinon, restaurez les données à partir d'une sauvegarde si le problème se traduit par une défaillance lors du démarrage.|  
  
## <a name="explanation"></a>Explication  
Un composant a passé un numéro séquentiel dans le journal non valide au gestionnaire de fichiers journaux de la base de données. Il peut s'agir d'une réplication, d'une altération ou d'une incohérence entre le fichier de données primaires et le journal.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Si cela s’est produit pendant la réplication, recréez la publication. Sinon, effectuez une restauration à partir de la sauvegarde.  
  
