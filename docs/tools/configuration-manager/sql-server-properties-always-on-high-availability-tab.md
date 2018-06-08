---
title: Propriétés de SQL Server (onglet Haute disponibilité AlwaysOn) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d8630923-a600-4f1c-aca1-027453a3ec82
caps.latest.revision: 13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: cb889dfae1625cdf5f07175fe72ce679f0f477ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-properties-always-on-high-availability-tab"></a>Propriétés de SQL Server (onglet Haute disponibilité AlwaysOn)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  Utilisez l’onglet **Haute disponibilité AlwaysOn** de la boîte de dialogue **Propriétés de SQL Server** du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour activer ou désactiver la fonctionnalité Groupes de disponibilité AlwaysOn de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. L’activation de Groupes de disponibilité AlwaysOn est nécessaire pour qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les groupes de disponibilité comme solution de haute disponibilité et de récupération d’urgence.  
  
##  <a name="Prerequisites"></a> Conditions préalables  
 Pour être activée pour Groupes de disponibilité AlwaysOn, une instance de serveur doit remplir les conditions suivantes :  
  
-   L'instance de serveur doit résider sur un nœud de Clustering de basculement Windows Server (WSFC).  
  
-   Pour appartenir au même groupe de disponibilité, les instances doivent être dans le même cluster WSFC. Un groupe de disponibilité ne peut pas s'étendre sur plusieurs clusters WSFC.  
  
-   L'instance du serveur doit exécuter une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prend en charge [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
-   Activez Groupes de disponibilité AlwaysOn pour une seule instance de serveur à la fois. Après avoir activé Groupes de disponibilité AlwaysOn, attendez le redémarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant d’activer l’instance de serveur suivante.  
  
> [!NOTE]  
>  Pour plus d’informations sur la prise en charge des fonctionnalités et sur les conditions préalables, restrictions et recommandations supplémentaires pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], consultez la documentation en ligne de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
## <a name="dialog-options"></a>Options de boîte de dialogue  
 **Nom du cluster de basculement Windows**  
 Affiche le nom du cluster WSFC dans lequel l'ordinateur local est un nœud.  
  
 **Activer les groupes de disponibilité Always On**  
 Cochez cette case pour activer ou désactiver Groupes de disponibilité AlwaysOn sur cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme suit :  
  
-   Si cette case à cocher est vide, Groupes de disponibilité AlwaysOn est désactivé. Pour activer Groupes de disponibilité AlwaysOn, cochez cette case, cliquez sur **OK**et redémarrez manuellement le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Si cette case est déjà cochée, Groupes de disponibilité AlwaysOn est actuellement activé. Pour désactiver Groupes de disponibilité AlwaysOn, décochez la case et cliquez sur **OK**. Cela entraîne le redémarrage de l'instance de serveur.  
  
    > [!TIP]  
    >  Après avoir désactivé Groupes de disponibilité AlwaysOn, supprimez les réplicas de disponibilité locaux de l’instance de serveur. Si vous supprimez la dernière réplica d'un groupe de disponibilité donné, supprimez également le groupe.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
  
> [!NOTE]  
>  Pour plus d’informations sur le suivi après la désactivation de Groupes de disponibilité AlwaysOn et la procédure à suivre pour créer et configurer des groupes de disponibilité, consultez la documentation en ligne de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
  
