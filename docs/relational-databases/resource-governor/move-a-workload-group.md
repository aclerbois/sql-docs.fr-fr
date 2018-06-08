---
title: Déplacer un groupe de charge de travail | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.rg.properties_moveworkloadgroup.f1
helpviewer_keywords:
- workload groups [SQL Server], move
- Resource Governor, workload group move
ms.assetid: f2068636-6e53-486a-a6fc-c12de2a38424
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 8d1eb983f32e995d974ffbfb460e679f715d20ac
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="move-a-workload-group"></a>Déplacer un groupe de charge de travail
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez déplacer un groupe de charge de travail de Resource Governor vers un pool de ressources différent à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de Transact-SQL.  
  
-   **Before you begin:**  [Limitations and Restrictions](#LimitationsRestrictions), [Permissions](#Permissions)  
  
-   **Pour déplacer un groupe de charge de travail, utilisez :**  [SQL Server Management Studio](#MoveWGSSMS), [Transact-SQL](#MoveWGTSQL)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 Vous ne pouvez pas déplacer un groupe de charge de travail s'il existe une opération en attente de configuration de Resource Governor.  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Vous ne pouvez pas déplacer un groupe de charge de travail s'il existe une opération en attente de configuration de Resource Governor. Vous pouvez déterminer s’il existe une configuration en attente en interrogeant la vue de gestion dynamique [sys.dm_resource_governor_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md) pour obtenir l’état en cours d’is_configuration_pending.  
  
###  <a name="Permissions"></a> Permissions  
 Le déplacement d'un groupe de charge de travail nécessite l'autorisation CONTROL SERVER.  
  
##  <a name="MoveWGSSMS"></a> Déplacer un groupe de charge de travail à l'aide de SQL Server Management Studio  
 **Pour déplacer un groupe de charge de travail à l'aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]**  
  
1.  Dans l'Explorateur d'objets, développez de manière récursive le nœud **Gestion** vers le bas jusqu'à **Resource Governor**.  
  
2.  Cliquez avec le bouton droit sur **Resource Governor** , puis sélectionnez **Propriétés**afin d’ouvrir la page **Propriétés de Resource Governor** .  
  
3.  Dans la fenêtre **Pools de ressources** , cliquez sur pool de ressources qui contient le groupe de charge de travail à déplacer. La fenêtre **Groupes de charge de travail** répertorie maintenant les groupes de charge de travail de ce pool de ressources.  
  
4.  Dans la fenêtre **Groupes de charge de travail** , cliquez avec le bouton droit sur la flèche vers la droite à gauche du groupe de charge de travail à déplacer, puis sélectionnez **Déplacer vers**. Cela affiche une fenêtre **Déplacer le groupe de charge de travail** .  
  
5.  Les pools de ressources disponibles sont affichés dans la fenêtre. Cliquez sur le nom du pool de ressources vers lequel vous souhaitez déplacer le groupe de charge de travail, puis cliquez sur **OK** pour effectuer cette action.  
  
6.  Cette action n'est pas effectuée tant que vous n'avez pas cliqué sur **OK**. Lorsque vous cliquez sur **OK**, l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE est exécutée.  
  
7.  Si l'opération de création ou de reconfiguration du pool de ressources ou du groupe de charge de travail échoue, un message d'erreur récapitulatif apparaît sous le titre de la page de propriétés. Pour consulter le message d'erreur détaillé, cliquez sur la flèche vers le bas du message d'erreur.  
  
##  <a name="MoveWGTSQL"></a> Déplacer un groupe de charge de travail à l'aide de Transact-SQL  
 **Pour déplacer un groupe de charge de travail à l'aide de Transact-SQL**  
  
1.  Exécutez l’instruction **ALTER WORKLOAD GROUP** en spécifiant le nom du groupe de charge de travail à déplacer, ainsi que le pool de ressources vers lequel il doit être déplacé.  
  
2.  Exécutez l'instruction **ALTER RESOURCE GOVERNOR RECONFIGURE** .  
  
### <a name="example-transact-sql"></a>Exemple (Transact-SQL)  
 L'exemple suivant déplace un groupe de charge de travail nommé `groupAdhoc` vers le pool de ressources par défaut.  
  
```  
ALTER WORKLOAD GROUP groupAdhoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Créer un groupe de charge de travail](../../relational-databases/resource-governor/create-a-workload-group.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
