---
title: MSSQL_ENG020557 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG020557 error
ms.assetid: c43c6952-5b60-4347-b881-11a0004dce24
caps.latest.revision: 10
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1384d7cf516de638b1c73b864875ca288fb1b80d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng020557"></a>MSSQL_ENG020557
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|20557|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Arrêt de l'Agent. Pour plus d'informations, reportez-vous à l'historique des travaux de l'Agent SQL Server pour le travail « %s ».|  
  
## <a name="explanation"></a>Explication  
 Un agent de réplication a été arrêté sans qu'aucune explication n'ait été écrite dans la table d'historique appropriée ou l'agent a été arrêté au milieu d'un processus.  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
-   Redémarrez l'Agent pour voir s'il va s'exécuter sans erreur. Pour plus d’informations, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) et [Concepts des exécutables de l’agent de réplication](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
-   Vérifiez dans l'historique de l'Agent et des travaux si d'autres erreurs se sont produites vers la même heure. Pour des informations sur la manière dont vous pouvez afficher l'état des agents ainsi que des informations détaillées sur les erreurs dans le moniteur de réplication, consultez les rubriques suivantes :  
  
    -   Pour l’Agent d’instantané, l’Agent de lecture du journal et l’Agent de lecture de la file d’attente, consultez [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
    -   Pour l’Agent de distribution et l’Agent de fusion, consultez [Afficher des informations et effectuer des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
-   Vérifiez que la connectivité de base fonctionne entre les ordinateurs auxquels l’Agent accède, puis connectez-vous à chaque ordinateur à l’aide d’un utilitaire tel que **sqlcmd** . Lors de la connexion, utilisez le même compte que celui qu'utilise l'Agent pour se connecter. Pour plus d'informations sur les autorisations requises pour chaque compte d'agent, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
-   Si l'erreur se produit lors de la création ou de l'application d'un instantané, vérifiez si les fichiers du répertoire d'instantanés comportent des erreurs.  
  
-   Si l'erreur continue de se produire, augmentez le facteur de journalisation de l'agent et spécifiez un fichier de sortie pour le journal. En fonction du contexte de l'erreur, cette action peut révéler les étapes qui conduisent à l'erreur et fournir d'autres messages d'erreur. Pour plus d'informations sur la configuration de la journalisation pour la réplication, consultez l'article [312292](http://support.microsoft.com/kb/312292)de la Base de connaissances Microsoft.  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
