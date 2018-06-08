---
title: lightweight pooling (option de configuration de serveur) | Microsoft Docs
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
- default lightweight pooling
- decreasing overhead
- lightweight pooling option
- system overhead [SQL Server]
- performance [SQL Server], lightweight pooling
- context switching [SQL Server], lightweight pooling option
- excessive context switching [SQL Server]
- reducing overhead
- overhead [SQL Server]
ms.assetid: 2dc11b61-d065-4126-8e00-acf40390f9fb
caps.latest.revision: 31
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f786e74c9ee0ec0e29f6ec75df123f0e953e616b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lightweight-pooling-server-configuration-option"></a>lightweight pooling (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Utilisez l’option **lightweight pooling** pour fournir un moyen de réduire la charge système associée aux basculements excessifs de contexte rencontrés parfois en environnement de multitraitement symétrique. En cas de basculements de contexte excessifs, le regroupement léger peut offrir une meilleure capacité de traitement en effectuant le basculement de contexte en ligne, ce qui permet de réduire les permutations circulaires utilisateur/noyau.  
  
 Le mode fibre est prévu pour certaines situations dans lesquelles le basculement de contexte des travailleurs UMS constitue le goulot d'étranglement critique en ce qui concerne les performances. Ces situations étant rares, le mode fibre améliore rarement les performance ou l'évolutivité sur un système ordinaire. L’amélioration du basculement de contexte dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] a également réduit la nécessité du mode fibre. Il n'est pas recommandé d'utiliser la planification du mode fibre pour les opérations courantes. Ceci pour deux raisons : cela peut réduire les performances en inhibant les avantages ordinaires du basculement de contexte et certains composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui utilisent des objets TLS (Thread Stockage Local) ou possédés par des threads, tels que des mutexes (un type d'objet de noyau Win32), ne peuvent pas fonctionner correctement en mode fibre.  
  
 Quand l’option de **regroupement léger** a la valeur 1, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bascule en planification de mode fibre. La valeur par défaut de cette option est 0.  
  
 L’option **lightweight pooling** est une option avancée. Si vous utilisez la procédure stockée système **sp_configure** pour changer sa valeur, vous ne pouvez modifier l’option **lightweight pooling** que si la valeur 1 a été attribuée à l’option **Afficher les options avancées**. Le paramétrage prend effet une fois le serveur redémarré.  
  
> [!NOTE]  
>  Le regroupement léger n’est pas pris en charge pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 2000 ni [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows XP. [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] fournit une prise en charge complète du regroupement léger.  
  
> [!NOTE]  
>  L'exécution du CLR (Common Language Runtime) n'est pas prise en charge sous l'option lightweight pooling. Désactivez l'une des deux options suivantes : « clr enabled » ou « lightweight pooling ». Les fonctionnalités qui reposent sur le CLR et qui ne fonctionnent pas correctement en mode fibre incluent le type de données de hiérarchie, la réplication et la Gestion basée sur des stratégies.  
  
## <a name="see-also"></a> Voir aussi  
 [clr enabled (option de configuration de serveur)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [clr enabled (option de configuration de serveur)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)  
  
  
