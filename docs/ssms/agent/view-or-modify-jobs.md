---
title: Afficher ou modifier les travaux | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- jobs [SQL Server Agent], viewing
- modifying jobs
- viewing jobs
- SQL Server Agent jobs, viewing
- SQL Server Agent jobs, modifying
- displaying jobs
ms.assetid: 57f649b8-190c-4304-abd7-7ca5297deab7
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4dc734ccf3ce4075a48d69213c2e3d87581f58e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-or-modify-jobs"></a>Afficher ou modifier les travaux
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Vous pouvez afficher tout travail que vous avez créé. Après avoir exécuté un travail, vous pouvez également afficher son historique. L'affichage de l'historique d'un travail vous permet de déterminer le moment de son exécution, l'état du travail dans son ensemble et l'état de chaque étape du travail. Il permet également de savoir si le travail a déjà échoué dans le passé, si sa dernière exécution s'est terminée correctement, ainsi que les résultats qu'il a générés à chaque exécution. Les membres du rôle serveur fixe **sysadmin** peuvent afficher ou modifier n’importe quel travail, quel que soit son propriétaire.  
  
> [!NOTE]  
> Le travail doit avoir été exécuté au moins une fois pour qu'un historique des travaux existe. Vous pouvez limiter la taille totale du journal d'historique des travaux et la taille par travail.  
  
Enfin, vous pouvez modifier un travail pour qu'il réponde aux nouvelles exigences. Vous pouvez modifier :  
  
-   les options de réponse ;  
  
-   Planifications  
  
-   les étapes du travail ;  
  
-   l'appartenance ;  
  
-   la catégorie du travail ;  
  
-   les serveurs cibles (travaux multiserveur uniquement).  
  
Pour être certain que les modifications apportées aux travaux multiserveur entrent en vigueur, vous devez les publier dans la liste de téléchargement afin que les serveurs cibles puissent télécharger le travail mis à jour. Pour vérifier que les serveurs cibles possèdent les définitions des travaux les plus récentes, publiez une instruction INSERT une fois le travail multiserveur mis à jour en procédant comme suit :  
  
```  
EXECUTE sp_post_msx_operation 'INSERT', 'JOB', '<job id>'  
```  
  
Pour plus d’informations, consultez [sp_purge_jobhistory (Transact-SQL)](http://msdn.microsoft.com/en-us/237f9bad-636d-4262-9bfb-66c034a43e88).  
  
Les membres du rôle serveur fixe **sysadmin** peuvent afficher la définition ou l’historique de n’importe quel travail, ainsi que modifier tout travail.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|**Description**|**Rubrique**|  
|Explique comment afficher des travaux [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Afficher un travail](../../ssms/agent/view-a-job.md)|  
|Explique comment afficher le journal d’historique des travaux [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Afficher l'historique des travaux](../../ssms/agent/view-the-job-history.md)|  
|Explique comment supprimer le contenu du journal d’historique des travaux [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Effacer le journal d'historique des travaux](../../ssms/agent/clear-the-job-history-log.md)|  
|Explique comment définir des limites de taille dans les journaux d’historique des travaux [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Redimensionner le journal d'historique des travaux](../../ssms/agent/resize-the-job-history-log.md)|  
|Explique comment modifier les propriétés des travaux [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent.|[Modifier un travail](../../ssms/agent/modify-a-job.md)|  
  
## <a name="see-also"></a> Voir aussi  
[sysjobhistory](http://msdn.microsoft.com/en-us/1b1fcdbb-2af2-45e6-bf3f-e8279432ce13)  
  
