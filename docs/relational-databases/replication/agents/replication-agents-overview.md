---
title: Présentation des Agents de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- Distribution Agent
- agents [SQL Server replication]
- Queue Reader Agent, about Queue Reader Agent
- Queue Reader Agent
- Merge Agent, about Merge Agent
- Log Reader Agent, about Log Reader Agent
- replication [SQL Server], agents and profiles
- Log Reader Agent
- Distribution Agent, about Distribution Agent
- agents [SQL Server replication], about agents
- Merge Agent
- Snapshot Agent, about Snapshot Agent
- Snapshot Agent
ms.assetid: a35ecd7d-f130-483c-87e3-ddc8927bb91b
caps.latest.revision: 42
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 70b47a65edd3acba9051d08219bb804117ca7557
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-agents-overview"></a>Présentation des Agents de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication utilise une série de programmes indépendants, appelés Agents, pour effectuer les tâches associées au suivi des modifications et à la distribution des données. Par défaut, les Agents de réplication s'exécutent comme des travaux planifiés sous [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent doit être démarré pour que ces travaux puissent s'exécuter. Les Agents de réplication peuvent être également exécutés à partir de la ligne de commande et par des applications qui utilisent des Replication Management Objects. Les Agents de réplication peuvent être administrés à partir du moniteur de réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
## <a name="sql-server-agent"></a>SQL Server Agent  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent héberge et planifie les Agents utilisés dans le cadre de la réplication, en plus d'offrir un moyen simple d'exécuter les Agents de réplication. Par ailleurs,[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent contrôle et analyse les opérations qui ne relèvent pas de la réplication. Pour plus d’informations, consultez [Configure SQL Server Agent](http://msdn.microsoft.com/library/2e361a62-9e92-4fcd-80d7-d6960f127900).  
  
> [!IMPORTANT]  
>  Par défaut, le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent est désactivé lors de l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , sauf si vous choisissez explicitement de démarrer automatiquement le service au cours de l'installation. Pour plus d'informations sur le démarrage du service de l'agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , consultez [Start, Stop, or Pause the SQL Server Agent Service](http://msdn.microsoft.com/library/c95a9759-dd30-4ab6-9ab0-087bb3bfb97c).  
  
## <a name="snapshot-agent"></a>Agent d'instantané  
 L'Agent d'instantané est généralement utilisé avec tous les types de réplication. Il prépare le schéma et les fichiers de données initiaux des tables publiées et d'autres objets, stocke les fichiers d'instantanés et enregistre les informations relatives à la synchronisation dans la base de données de distribution. L'Agent d'instantané s'exécute sur le serveur de distribution. Pour plus d’informations, consultez [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md).  
  
## <a name="log-reader-agent"></a>l'Agent de lecture du journal ;  
 L'Agent de lecture de journal est utilisé dans la réplication transactionnelle. Il déplace les transactions marquées pour la réplication depuis le journal des transactions du serveur de publication vers la base de données de distribution. Chaque base de données publiée à l'aide de la réplication transactionnelle possède son propre Agent de lecture du journal qui s'exécute sur le serveur de distribution et se connecte au serveur de publication (le serveur de distribution et le serveur de publication peuvent être installés sur le même ordinateur). Pour plus d’informations, consultez [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md).  
  
## <a name="distribution-agent"></a>Agent de distribution  
 L'Agent de distribution est utilisé dans la réplication d'instantané et dans la réplication transactionnelle. Il applique l'instantané initial sur l'Abonné et déplace les transactions conservées dans la base de données de distribution vers les Abonnés. L'Agent de distribution est exécuté sur le serveur de distribution pour les abonnements envoyés et sur l'Abonné pour les abonnements extraits. Pour plus d'informations, consultez [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="merge-agent"></a>Agent de fusion  
 L'Agent de fusion est utilisé dans la réplication de fusion. Il applique l'instantané initial à l'Abonné et déplace puis rapproche les modifications de données incrémentielles effectuées. Chaque abonnement de fusion a son propre Agent de fusion qui se connecte à la fois au serveur de publication et à l'Abonné et les met à jour. L'Agent de fusion est généralement exécuté sur le serveur de distribution pour les abonnements envoyés et sur l'Abonné pour les abonnements extraits. Par défaut, il télécharge les modifications de l'Abonné vers le serveur de publication puis du serveur de publication vers l'Abonné. Pour plus d’informations, consultez [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="queue-reader-agent"></a>Agent de lecture de la file d'attente  
 L'Agent de lecture de la file d'attente est utilisé dans le cadre de la réplication transactionnelle avec l'option de mise à jour en attente. Il s'exécute sur le serveur de distribution et redéplace les modifications effectuées sur l'Abonné vers le serveur de publication. Contrairement aux Agents de distribution et de fusion, il n'existe qu'une seule instance de l'Agent de lecture de file d'attente pour servir l'ensemble des serveurs de publication et des publications pour une base de données de distribution donnée. Pour plus d'informations sur l'Agent de lecture de la file d'attente, consultez [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md). Pour plus d'informations sur les abonnements pouvant être mis à jour, consultez [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="replication-maintenance-jobs"></a>Travaux de maintenance de la réplication  
 La réplication possède un certain nombre de travaux de maintenance qui effectuent une maintenance planifiée et à la demande. Pour plus d’informations, consultez [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Exécuter des travaux de maintenance de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)   
 [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)   
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
