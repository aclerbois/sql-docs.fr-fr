---
title: Mettre à jour les statistiques, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.updatestatisticstask.f1
helpviewer_keywords:
- updating statistics
- Update Statistics task [Integration Services]
ms.assetid: 0247483b-f092-4511-8fa8-3610108bd1bc
caps.latest.revision: 45
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 99ffc22f76b60744b492bf943960f9d05dc0b779
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="update-statistics-task"></a>Tâche Mettre à jour les statistiques
  La tâche Mettre à jour les statistiques met à jour les informations sur la distribution des valeurs de clé pour un ou plusieurs groupes de statistiques (collections) dans la table ou la vue indexée spécifiées. Pour plus d'informations, consultez [Statistics](../../relational-databases/statistics/statistics.md).  
  
 La tâche Mettre à jour les statistiques permet à un package de mettre à jour les statistiques d'une ou plusieurs bases de données. Si la tâche met à jour uniquement les statistiques d'une base de données, vous pouvez choisir les vues ou les tables concernées par cette mise à jour. Vous pouvez configurer la mise à jour de manière à actualiser toutes les statistiques ou uniquement celles des colonnes ou des index.  
  
 Cette tâche encapsule une instruction UPDATE STATISTICS, qui comprend les clauses et les arguments suivants :  
  
-   Argument *nom_table* ou *nom_vue* .  
  
-   Si la mise à jour s'applique à toutes les statistiques, la clause WITH ALL est implicite.  
  
-   Si la mise à jour s'applique uniquement aux colonnes, la clause WITH COLUMN est incluse.  
  
-   Si la mise à jour s'applique uniquement aux index, la clause WITH INDEX est incluse.  
  
 Si la tâche Mettre à jour les statistiques met à jour les statistiques dans plusieurs bases de données, elle exécute plusieurs instructions UPDATE STATISTICS, à raison d'une pour chaque table ou vue. Toutes les instances de l’instruction UPDATE STATISTICS utilisent la même clause, mais différentes valeurs pour *nom_table* ou pour *nom_vue*. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md) et [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md).  
  
> [!IMPORTANT]  
>  Le temps que prend la tâche à créer l'instruction Transact-SQL qu'elle exécute est proportionnel au nombre de statistiques qu'elle met à jour. Si la tâche est configurée de manière à mettre à jour les statistiques de toutes les tables et vues d'une base de données avec un grand nombre d'index ou à mettre à jour les statistiques de plusieurs bases de données, elle peut prendre beaucoup de temps pour générer l'instruction Transact-SQL.  
  
## <a name="configuration-of-the-update-statistics-task"></a>Configuration de la tâche Mettre à jour les statistiques  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche Mettre à jour les statistiques &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/update-statistics-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
