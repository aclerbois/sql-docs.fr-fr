---
title: LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: c178a308-8d99-47fc-8a49-5a480dc592f6
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 28c3c7f214ef3eff27f04c24cee8b809c063587f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="localdberrorinstancefolderpathtoolong"></a>LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|260|  
|Source de l'événement|Runtime de base de données locale SQL Server 12.0|  
|Composant|API d'exécution de la base de données locale|  
|Texte du message|Le chemin complet du dossier d'instance de base de données locale est plus long que MAX_PATH. L’instance doit être stockée dans le dossier : %%LOCALAPPDATA%%\Microsoft\Microsoft DB\Instances locale de SQL Server\\< nom de l’instance\>.|  
  
## <a name="explanation"></a>Explication  
 Le chemin d'accès où l'instance doit être stockée est plus long que MAX_PATH.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Créez un nouveau chemin d'accès plus court que MAX_PATH.  
  
  
