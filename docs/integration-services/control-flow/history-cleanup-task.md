---
title: Nettoyage d’historique, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- sql13.dts.designer.historycleanuptask.f1
helpviewer_keywords:
- history tables [SQL Server]
- History Cleanup task [Integration Services]
ms.assetid: 5defc5b9-dfd3-4859-a7fe-ac8c2b5480f8
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 844bb2744d4f137f3f07c4f49a6c672f86b3ae68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="history-cleanup-task"></a>Tâche de nettoyage d'historique
  La tâche de nettoyage d'historique supprime des entrées dans les tables d'historique suivantes de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb.  
  
-   backupfile  
  
-   backupfilegroup  
  
-   backupmediafamily  
  
-   backupmediaset  
  
-   backupset  
  
-   restorefile  
  
-   restorefilegroup  
  
-   restorehistory  
  
 Grâce à la tâche de nettoyage d'historique, un package peut supprimer des données d'historique relatives aux activités de sauvegarde et de restauration, aux travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux plans de maintenance de base de données.  
  
 Cette tâche encapsule la procédure stockée système sp_delete_backuphistory et lui transmet la date spécifiée en guise d'argument. Pour plus d’informations, consultez [sp_delete_backuphistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md).  
  
## <a name="configuration-of-the-history-cleanup-task"></a>Configuration de la tâche de nettoyage d'historique  
 La tâche possède une propriété qui permet de spécifier la plus ancienne date des données conservées dans les tables d'historique. Vous pouvez indiquer la date en nombre de jours, de semaines, de mois ou d'années par rapport au jour actuel ; la tâche convertit automatiquement l'intervalle en une date.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche de nettoyage d’historique &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/history-cleanup-task-maintenance-plan.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
