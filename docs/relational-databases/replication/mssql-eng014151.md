---
title: MSSQL_ENG014151 | Microsoft Docs
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
- MSSQL_ENG014151 error
ms.assetid: 54b45e70-46b3-4c7a-a5bf-06f6dd028ceb
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: dedaa2d7137bedb285405635180dbfec017cf16c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng014151"></a>MSSQL_ENG014151
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|14151|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Réplication-%s : échec de l'agent %s. %s|  
  
## <a name="explanation"></a>Explication  
 Cette erreur est émise pour tout échec d'un Agent de réplication. Le texte de la fin du message dépend du contexte de l'échec.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Cette erreur peut se produire dans un certain nombre de situations ; utilisez l'une des approches ci-dessous selon le cas :  
  
-   Redémarrez l'Agent en échec pour voir s'il s'exécutera sans erreur. Pour plus d’informations, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) et [Concepts des exécutables de l’agent de réplication](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
-   Vérifiez dans l'historique de l'Agent et des travaux si d'autres erreurs se sont produites vers la même heure. Pour des informations sur l'affichage de l'état et des informations détaillées des erreurs des agents dans le moniteur de réplication, consultez les rubriques suivantes :  
  
    -   Pour l’Agent d’instantané, l’Agent de lecture du journal et l’Agent de lecture de la file d’attente, consultez [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
    -   Pour l’Agent de distribution et l’Agent de fusion, consultez [Afficher des informations et effectuer des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
-   Vérifiez que la connectivité de base fonctionne entre les ordinateurs auxquels accède l'Agent, puis connectez-vous à chaque ordinateur avec un utilitaire tel que [sqlcmd Utility](../../tools/sqlcmd-utility.md). Lors de la connexion, utilisez le même compte que celui qu'utilise l'Agent pour se connecter. Pour plus d'informations sur les autorisations requises pour chaque compte d'Agent, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
-   Si l'erreur se produit lors de la création ou de l'application d'un instantané, vérifiez si les fichiers du répertoire d'instantané ne comportent pas d'erreurs.  
  
-   Si l'erreur continue de se produire, augmentez le facteur de journalisation de l'agent et spécifiez un fichier de sortie pour le journal. En fonction du contexte de l'erreur, cette action peut fournir des pistes conduisant à l'erreur et/ou à d'autres messages d'erreur.  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Agent de distribution de réplication](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Agent de lecture du journal de réplication](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Agent de fusion de réplication](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agent de lecture de la file d’attente de réplication](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  
