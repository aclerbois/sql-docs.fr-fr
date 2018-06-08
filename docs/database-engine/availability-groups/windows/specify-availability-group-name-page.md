---
title: Page Spécifier les options du groupe de disponibilité (Assistant Nouveau groupe de disponibilité/Assistant Ajouter une base de données) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.specifyagname.f1
- sql13.swb.adddatabasewizard.specifyagname.f1
ms.assetid: dcb6374d-becb-4c6c-b88c-5a8273f8aa38
caps.latest.revision: 13
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1938b41ea4d23bc92e0c18a5a5ec7ccbcd2f8b8e
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771345"
---
# <a name="specify-availability-group-options-page"></a>Page Spécifier les options du groupe de disponibilité
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit les options de la page **Spécifier le nom du groupe de disponibilité** . Cette rubrique est utilisée par l' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] et l' [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="PageOptions"></a> Spécifier les options du groupe de disponibilité  
 **Nom du groupe de disponibilité**  
 Spécifiez le nom du groupe de disponibilité. Pour un nouveau groupe de disponibilité, spécifiez un identificateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valide, unique dans tous les groupes de disponibilité du cluster de basculement Windows Server. La longueur maximale d'un nom de groupe de disponibilité est de 128 caractères.  

 **Type de cluster** Ensuite, spécifiez le type de cluster. Les types de cluster possibles dépendent de la version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et du système d’exploitation. Choisissez dans la liste suivante :

   * **Clustering de basculement Windows Server**
   
      À utiliser quand le groupe de disponibilité est hébergé sur des instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui appartiennent à un cluster de basculement Windows Server pour la haute disponibilité et la récupération d’urgence. S’applique à toutes les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prises en charge. 

   * **EXTERNAL**
      
      À utiliser quand le groupe de disponibilité est hébergé sur une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui est gérée par une technologie de cluster externe pour la haute disponibilité et la récupération d’urgence, par exemple Pacemaker sur Linux. S’applique à [!INCLUDE[sssqlv14](../../../includes/sssqlv14-md.md)] et ultérieur.

   * **NONE**
      
      À utiliser quand le groupe de disponibilité est hébergé sur une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui n’est pas gérée par une technologie de cluster pour la mise à l’échelle en lecture et l’équilibrage de charge. S’applique à [!INCLUDE[sssqlv14](../../../includes/sssqlv14-md.md)] et ultérieur. 
 
   **Détection de l’état d’intégrité au niveau base de données** Cochez cette case pour activer l’option de détection de l’état d’intégrité au niveau base de données (DB_FAILOVER) pour le groupe de disponibilité. La détection de l’état d’intégrité de base de données indique quand une base de données n’est plus en ligne ou quand un problème se produit, puis déclenche le basculement automatique du groupe de disponibilité. Consultez [Option Détection de l’état d’intégrité au niveau base de données Always On SQL Server](sql-server-always-on-database-health-detection-failover-option.md).


Page Sélectionner les bases de données (Assistant Nouveau groupe de disponibilité et Assistant Ajouter une base de données)  
  
##  <a name="LaunchWiz"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
