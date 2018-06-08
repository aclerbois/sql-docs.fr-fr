---
title: Propriétés de mémoire partagée | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- shared memory [SQL Server]
ms.assetid: dc1704da-eacd-4d26-b529-c996f958ca4b
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 730c784c1c7f92bf0c8fc81f66484b57354f7c31
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="shared-memory-properties"></a>Propriétés de Mémoire partagée
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  La page **Protocole** de la boîte de dialogue **Propriétés de Mémoire partagée** permet d’activer ou de désactiver le protocole de mémoire partagée. Il s'agit du protocole le plus simple à utiliser et pour lequel aucun paramètre ne doit être configuré. Étant donné que les clients qui utilisent le protocole de mémoire partagée peuvent se connecter uniquement à une instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée sur le même ordinateur, ce protocole n’est généralement pas utile pour les activités de base de données. Utilisez le protocole de mémoire partagée pour débloquer une situation dans laquelle vous suspectez que les autres protocoles ne sont pas configurés correctement.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être redémarré pour activer ou désactiver le protocole.  
  
## <a name="options"></a>Options  
 **Activé**  
 Les valeurs possibles sont **Yes** et **No**. Le protocole de mémoire partagée est activé par défaut.  
  
## <a name="see-also"></a> Voir aussi  
 [Choix d'un protocole réseau](http://msdn.microsoft.com/library/6565fb7d-b076-4447-be90-e10d0dec359a)   
 [Création d'une chaîne de connexion valide à l'aide du protocole de mémoire partagée](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
  
