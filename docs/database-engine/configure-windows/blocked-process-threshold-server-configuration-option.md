---
title: blocked process threshold (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- thresholds [SQL Server]
- blocked process threshold option
ms.assetid: 3d46d143-bc6a-4220-8b55-6baa37547c25
caps.latest.revision: 20
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b6c44f7148949290560d66a5a06dd70db13054f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="blocked-process-threshold-server-configuration-option"></a>blocked process threshold (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L'option **blocked process threshold** permet de spécifier le seuil, en secondes, à partir duquel des rapports de processus bloqués sont générés. Le seuil peut être compris entre 0 et 86 400. Par défaut, aucun rapport de processus bloqué n'est généré. Cet événement n'est pas généré pour les tâches système ou les tâches en attente de ressources qui ne génèrent pas de blocages détectables.  
  
 Vous pouvez définir une [alerte](http://msdn.microsoft.com/library/3f57d0f0-4781-46ec-82cd-b751dc5affef) à exécuter dès lorsque cet événement est généré. Ainsi, par exemple, vous pouvez choisir de contacter l'administrateur par récepteur de radiomessagerie afin de l'inviter à gérer la situation de blocage de manière appropriée.  
  
 L'option « blocked process threshold » utilise le thread d'arrière-plan Moniteur de blocage pour parcourir la liste des tâches en attente depuis une durée supérieure ou multiple du seuil configuré. L'événement est généré une fois par intervalle de génération de rapports pour chaque tâche bloquée.  
  
 Les rapports de processus bloqués sont générés le plus tôt possible. Toutefois, rien ne garantit qu'ils seront générés en temps réel ou quasiment en temps réel.  
  
 Le paramètre prend effet immédiatement, sans arrêt et redémarrage du serveur.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, l'option `blocked process threshold` est définie à `20` secondes, générant ainsi un rapport de processus bloqué pour chaque tâche bloquée.  
  
```  
sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE ;  
GO  
sp_configure 'blocked process threshold', 20 ;  
GO  
RECONFIGURE ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Blocked Process Report, classe d’événements](../../relational-databases/event-classes/blocked-process-report-event-class.md)  
  
  
