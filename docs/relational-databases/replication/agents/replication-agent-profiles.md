---
title: Profils de l’Agent de réplication | Microsoft Docs
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
- Distribution Agent, profiles
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- Merge Agent, profiles
- agents [SQL Server replication], profiles
- Queue Reader Agent, profiles
- profiles [SQL Server], replication agents
- Snapshot Agent, profiles
- Log Reader Agent, profiles
ms.assetid: 0e980725-e42f-4283-94cb-d8a6dba5df62
caps.latest.revision: 44
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 895b474a43d8112c8ff14d4a1aab7f3b3ae07dd7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-agent-profiles"></a>Profils de l'Agent de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Quand la réplication est configurée, un ensemble de profils d'agent est installé sur le serveur de distribution. Un profil d'agent contient un ensemble de paramètres qui sont utilisés chaque fois qu'un agent s'exécute : pendant le processus de démarrage, chaque agent se connecte au service de distribution et interroge les paramètres situés dans son profil. Pour les abonnements de fusion utilisant la synchronisation Web, les profils sont téléchargés et stockés sur l'Abonné. En cas de modification du profil, le profil sur l'Abonné est mis à jour à l'exécution suivante de l'Agent de fusion. Pour plus d'informations sur la synchronisation Web, consultez [Web Synchronization for Merge Replication](../../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 La réplication fournit un profil par défaut pour chaque agent et des profils supplémentaires prédéfinis pour l'Agent de lecture du journal, l'Agent de distribution et l'Agent de fusion. En plus des profils fournis, vous pouvez créer des profils adaptés aux besoins de vos applications. Un profil d'agent permet de modifier aisément les paramètres clés de tous les agents associés à ce profil. Par exemple, si vous disposez de 20 Agents d'instantané et devez changer la valeur du délai d'expiration des requêtes (paramètre **-QueryTimeout** ), vous pouvez mettre à jour le profil utilisé par les Agents d'instantané ; tous les agents de ce type utiliseront automatiquement la nouvelle valeur lors de leur exécution suivante.  
  
 Vous pouvez également disposer de profils différents pour les diverses instances d'un agent. Par exemple, un Agent de fusion qui utilise une connexion à distance pour se connecter au serveur de publication et au serveur de distribution peut se servir d'un ensemble de paramètres plus adaptés à cette liaison de communication moins rapide en recourant au profil **liaison lente** .  
  
> [!NOTE]  
>  Si vous spécifiez une valeur pour un paramètre d'agent sur la ligne de commande, cette valeur supplante la valeur définie pour le même paramètre dans le profil d'agent.  
  
 **Pour utiliser et modifier des profils d'agents**  
  
-   [Utiliser des profils d’agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
## <a name="snapshot-agent-profiles"></a>Profils de l'Agent d'instantané  
 Le tableau suivant montre les paramètres définis dans le profil par défaut de l'Agent d'instantané. Pour plus d'informations sur ces paramètres, consultez [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md).  
  
||par défaut|  
|-|-------------|  
|**-BcpBatchSize**|100000|  
|**-HistoryVerboseLevel**|2|  
|**-LoginTimeout**|15|  
|**-QueryTimeout**|1800|  
  
## <a name="log-reader-agent-profiles"></a>Profils de l'Agent de lecture du journal  
 Le tableau suivant montre les paramètres définis dans les profils de l'Agent de lecture du journal. Chaque colonne du tableau représente un profil nommé. Pour plus d'informations sur ces paramètres, consultez [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md).  
  
||par défaut|historique commenté|  
|-|-------------|---------------------|  
|**-HistoryVerboseLevel**| 1|2|  
|**-LoginTimeout**|15|15|  
|**-LogScanThreshold**|500000|500000|  
|**-PollingInterval**|5|5|  
|**-QueryTimeout**|1800|1800|  
|**-ReadBatchSize**|500|500|  
  
## <a name="distribution-agent-profiles"></a>Profils de l'Agent de distribution  
 Le tableau suivant montre les paramètres définis dans les profils de l'Agent de distribution. Chaque colonne du tableau représente un profil nommé. Pour plus d'informations sur ces paramètres, consultez [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
||par défaut|historique commenté|Gestionnaire de synchronisation Windows|Continuer avec les erreurs de cohérence des données|Profil de distribution du flux de données OLEDB|  
|-|-------------|---------------------|-------------------------------------|-----------------------------------------|----------------------------------------------|  
|**-BcpBatchSize**|100000|100000|1000|100000|2147473647|  
|**-CommitBatchSize**|100|100|100|100|100|  
|**-CommitBatchThreshold**|1000|1000|1000|1000|1000|  
|**-HistoryVerboseLevel**| 1|2| 1| 1| 1|  
|**-KeepAliveMessageInterval**|300|300|300|300|300|  
|**-LoginTimeout**|15|15|15|15|15|  
|**-MaxBcpThreads**| 1| 1| 1| 1| 1|  
|**-MaxDeliveredTransactions**|0|0|0|0|0|  
|**-OledbStreamThreshold**|NULL|NULL|NULL|NULL|32768|  
|**-PacketSize**|NULL|NULL|NULL|NULL|32768|  
|**-PollingInterval**|5|5|5|5|5|  
|**-QueryTimeout**|1800|1800|1800|1800|1800|  
|**-SkipErrors**|NULL|NULL|NULL|**-SkipErrors** 2601:2627:20598|NULL|  
|**-TransactionsPerHistory**|100|100|100|100|100|  
|**-UseOledbStreaming**|NULL|NULL|NULL|NULL|**-UseOledbStreaming**|  
  
## <a name="merge-agent-profiles"></a>Profils de l'Agent de fusion  
 Le tableau suivant montre les paramètres définis dans les profils de l'Agent de fusion. Chaque colonne du tableau représente un profil nommé. Pour plus d'informations sur ces paramètres, consultez [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md).  
  
||par défaut|historique commenté|Gestionnaire de synchronisation Windows|validation du nombre de lignes|validation du nombre de lignes et du total de contrôle|liaison lente|serveur à serveur haut volume|  
|-|-------------|---------------------|-------------------------------------|-------------------------|--------------------------------------|---------------|------------------------------------|  
|**-BcpBatchSize**|100000|100000|1000|100000|100000|100000|100000|  
|**-ChangesPerHistory**|100|50|50|100|100|100|1000|  
|**-DestThreads**|2| 1| 1| 1| 1| 1|4|  
|**-DownloadGenerationsPerBatch**|50|50|50|50|50| 1|500|  
|**-DownloadReadChangesPerBatch**|100|100|100|100|100|100|100|  
|**-DownloadWriteChangesPerBatch**|100|100|100|100|100|100|100|  
|**-FastRowCount**| 1| 1| 1| 1| 1| 1| 1|  
|**-HistoryVerboseLevel**|2|3| 1| 1|2| 1|2|  
|**-KeepAliveMessageInterval**|300|300|300|300|300|300|300|  
|**-LoginTimeout**|15|15|15|15|15|15|15|  
|**-MaxDownloadChanges**|0|0|0|0|0|0|0|  
|**-MaxUploadChanges**|0|0|0|0|0|0|0|  
|**-MetadataRetentionCleanup**| 1| 1| 1| 1| 1| 1| 1|  
|**-NumDeadlockRetries**|5|5|5|5|5|5|5|  
|**-ParallelUploadDownload**|NULL|NULL|NULL|NULL|NULL|NULL| 1|  
|**-PollingInterval**|60|60|60|60|60|60|60|  
|**-QueryTimeout**|300|300|300|300|300|300|600|  
|**-QueueSizeMultiplier**|NULL|NULL|NULL|NULL|NULL|NULL|5|  
|**-SrcThreads**|2|2|2|2|2| 1|3|  
|**-StartQueueTimeout**|0|0|0|0|0|0|0|  
|**-UploadGenerationsPerBatch**|50|50|50|50|50| 1|500|  
|**-UploadReadChangesPerBatch**|100|100|100|100|100|100|100|  
|**-UploadWriteChangesPerBatch**|100|100|100|100|100|100|100|  
|**-Validate**|0|0|0| 1|3|0|0|  
|**-ValidateInterval**|60|60|60|60|60|60|60|  
  
## <a name="queue-reader-agent-profiles"></a>Profils de l'Agent de lecture de la file d'attente  
 Le tableau suivant montre les paramètres définis dans le profil par défaut de l'Agent de lecture de la file d'attente. Pour plus d'informations sur ces paramètres, consultez [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md).  
  
||par défaut|  
|-|-------------|  
|**-HistoryVerboseLevel**| 1|  
|**-LoginTimeout**|15|  
|**-PollingInterval**|5|  
|**-QueryTimeout**|1800|  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)   
 [Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
  
