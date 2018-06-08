---
title: Administration de l’Agent de réplication | Microsoft Docs
ms.custom: ''
ms.date: 08/24/2016
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
- Snapshot Agent, administering
- Log Reader Agent, administering
- Queue Reader Agent, administering
- shared agents [SQL Server replication]
- Merge Agent, administering
- Distribution Agent, administering
- agents [SQL Server replication], administering
- replication cleanup jobs [SQL Server]
- administering replication, agents
- replication [SQL Server], administering
- independent agents [SQL Server replication]
ms.assetid: f27186b8-b1b2-4da0-8b2b-91f632c2ab7e
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b2607af48ae1f542ce314a0aebca1ec3463921fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-agent-administration"></a>Administration de l'Agent de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les agents de réplication accomplissent de nombreuses tâches associées à la réplication, notamment la création de copies du schéma et des données, la détection des mises à jour sur le serveur de publication ou sur l'Abonné, et la propagation des modifications entre les serveurs. Par défaut, les agents de réplication s'exécutent sous les étapes du travail de l'Agent [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Les agents sont simplement des exécutables et peuvent donc être appelés directement à partir de la ligne de commande et de scripts de commande par lot. Chaque agent de réplication prend en charge un ensemble de paramètres d'exécution utilisés pour contrôler comment il s'exécute ; ces paramètres sont spécifiés dans un profil d'agent ou sur la ligne de commande.  
  
> [!IMPORTANT]  
>  Par défaut, le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent est désactivé lors de l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , sauf si vous choisissez explicitement de démarrer automatiquement le service au cours de l'installation.  
  
 Les fichiers de l'Agent de réplication se trouvent sous [!INCLUDE[ssInstallPathVar](../../../includes/ssinstallpathvar-md.md)]\COM. Le tableau suivant contient la liste des noms des exécutables de la réplication et les noms des fichiers correspondants. Cliquez sur le lien correspondant à un agent pour afficher les informations de référence de ses paramètres.  
  
|Exécutable de l'agent|Nom de fichier|  
|----------------------|---------------|  
|[Agent d’instantané de réplication](../../../relational-databases/replication/agents/replication-snapshot-agent.md)|snapshot.exe|  
|[Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)|distrib.exe|  
|[Agent de lecture du journal des réplications](../../../relational-databases/replication/agents/replication-log-reader-agent.md)|logread.exe|  
|[Agent de lecture de la file d’attente de réplication](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)|qrdrsvc.exe|  
|[Agent de fusion de réplication](../../../relational-databases/replication/agents/replication-merge-agent.md)|replmerg.exe|  
  
 En plus des agents de réplication, la réplication a plusieurs travaux qui effectuent de la maintenance planifiée et à la demande.  
  
 **Pour exécuter les travaux des agents et de maintenance**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et moniteur de réplication : [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)  
  
-   Programmation de la réplication : [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
## <a name="agent-profiles"></a>Profils de l'Agent  
 Quand la réplication est configurée, un ensemble de profils d'agent est installé sur le serveur de distribution. Un profil d'agent contient un ensemble de paramètres qui sont utilisés chaque fois qu'un agent s'exécute : pendant le processus de démarrage, chaque agent se connecte au service de distribution et interroge les paramètres situés dans son profil. La réplication fournit un profil par défaut pour chaque agent et des profils supplémentaires prédéfinis pour l'Agent de lecture du journal, l'Agent de distribution et l'Agent de fusion. En plus des profils fournis, vous pouvez créer des profils adaptés aux besoins de vos applications. Pour plus d'informations, voir [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 Pour obtenir des informations sur la spécification directe de paramètres en ligne de commande, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="monitoring-replication-agents"></a>Surveillance des Agents de réplication  
 Le moniteur de réplication vous permet d'afficher des informations et d'effectuer des tâches associées à chaque agent de réplication. La liste suivante comprend chacun des agents, les onglets du moniteur de réplication sur lesquels ils peuvent être trouvés et un lien vers une rubrique qui explique comment accéder à ces onglets :  
  
-   Les agents suivants sont associés à des publications dans le moniteur de réplication :  
  
    -   Agent d'instantané  
  
    -   l'Agent de lecture du journal ;  
  
    -   Agent de lecture de la file d'attente  
  
     Accédez aux informations et aux tâches associées à ces agents via l'onglet **Agents** . Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
-   Les agents suivants sont associés à des abonnements dans le moniteur de réplication :  
  
    -   Agent de distribution  
  
    -   Agent de fusion  
  
     Accédez aux informations et aux tâches associées à ces agents via les onglets suivants : **Liste de suivi des abonnements** (disponible pour chaque serveur de publication) ou **Tous les abonnements** (disponible pour chaque abonnement). Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="independent-and-shared-agents"></a>Agents indépendants et partagés  
 Un Agent indépendant est un Agent qui sert un seul abonnement. Un agent partagé fournit des services à plusieurs abonnements ; si plusieurs abonnements utilisant le même agent partagé doivent se synchroniser, ils attendent par défaut dans une file d'attente, et l'agent partagé leur fournit ce service l'un après l'autre. Le temps de latence est réduit lors de l'utilisation d'agents indépendants car ceux-ci sont disponibles dès que l'abonnement doit être synchronisé. La réplication de fusion utilise toujours des agents indépendants, et la réplication transactionnelle utilise par défaut des agents indépendants pour les publications créées dans l'Assistant Nouvelle publication (dans les versions précédentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la réplication transactionnelle utilisait par défaut des agents partagés).  
  
## <a name="replication-maintenance-jobs"></a>Travaux de maintenance de la réplication  
 La réplication utilise les travaux suivants pour effectuer de la maintenance planifiée et à la demande.  
  
|Travail de nettoyage|Description|Planification par défaut|  
|------------------|-----------------|----------------------|  
|Nettoyage de l'historique de l'agent : distribution|Supprime les enregistrements historiques des agents de réplication dans la base de données de distribution.|S'exécute toutes les dix minutes|  
|Nettoyage de la distribution : distribution|Suppression des transactions répliquées de la base de données de distribution. |S'exécute toutes les dix minutes|  
|Nettoyage de l'abonnement expiré|Détecte les abonnements expirés et les retire des bases de données de publication. Sur le serveur de distribution, désactive les abonnements qui n’ont été pas été synchronisés au cours de la période maximale de rétention de distribution.|S'exécute chaque jour à 1 heure du matin.| 
|Réinitialiser les abonnements présentant des erreurs lors de la validation de données|Détecte tous les abonnements qui ont des échecs de validation des données et les marque pour réinitialisation. Lors de l'exécution suivante de l'Agent de fusion ou de l'Agent de distribution, un nouvel instantané sera appliqué aux Abonnés.|Pas de planification par défaut (non activé par défaut).|  
|Contrôle des Agents de réplication|Détecte les Agents de réplication n'ayant pas d'enregistrement historique actif. Il écrit dans le journal des événements de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows si l'étape d'un travail échoue.|S'exécute toutes les dix minutes.|  
|Actualisateur d'analyse de réplication pour la distribution|Actualise les requêtes mises en cache utilisées par le moniteur de réplication.|S'exécute en permanence.|  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
