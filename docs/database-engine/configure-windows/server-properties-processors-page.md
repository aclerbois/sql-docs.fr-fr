---
title: Propriétés du serveur (page Processeurs) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: adbaaa3528bf3a1c1e52d1c850ed641768c4272f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-properties---processors-page"></a>Propriétés du serveur - Page Processeurs
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez cette page pour afficher ou modifier les options de votre processeur. Les paramètres d'affinité du processeur ne sont activés que si plusieurs processeurs sont installés.  
  
## <a name="options"></a>Options  
 **Affinité du processeur**  
 Affecte des processeurs à des threads spécifiques afin d'éliminer les recharges de processeurs et de réduire la migration des threads entre les processeurs. Pour plus d’informations, consultez [affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).  
  
 **Affinité d'E/S**  
 Lie les E/S disque [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un sous-ensemble spécifié d’UC. Pour plus d’informations, consultez [affinity Input-Output mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).  
  
 **Définir automatiquement le masque d'affinité du processeur pour tous les processeurs**  
 Autorise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à définir l'affinité du processeur.  
  
 **Définir automatiquement le masque d'affinité d'E/S pour tous les processeurs**  
 Autorise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à définir l'affinité d'E/S.  
  
 **Nombre maximal de threads de travail**  
 0 autorise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à définir dynamiquement le nombre de threads de travail. Ce paramètre convient à la plupart des systèmes. Toutefois, selon votre configuration système, l'attribution d'une valeur spécifique à cette option peut permettre d'accroître les performances. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  
  
 **Renforcer la priorité SQL Server**  
 Indique si l’exécution de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir une priorité de planification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows supérieure à celle d’autres processus du même ordinateur. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  
  
 **Utiliser les fibres Windows (regroupement léger)**  
 Utilise les fibres Windows plutôt que des threads pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Notez que cette option n’est disponible que dans Windows 2003 Server Edition. Pour plus d’informations, consultez [lightweight pooling Server Configuration Option](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).  
  
 **Valeurs configurées**  
 Affiche les valeurs configurées pour les options de ce volet. Si vous modifiez ces valeurs, cliquez sur **Valeurs en cours d’exécution** pour vérifier si les modifications ont été prises en compte. Si ce n'est pas le cas, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être d'abord redémarrée.  
  
 **Valeurs en cours d’exécution**  
 Affiche les valeurs en cours d'exécution pour les options de ce volet. Ces valeurs sont en lecture seule.  
  
## <a name="see-also"></a> Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
  
