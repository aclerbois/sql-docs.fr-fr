---
title: SQL Server Integration Services (SSIS) Scale Out Worker | Microsoft Docs
ms.description: This article describes the Scale Out Master component of SSIS Scale Out
ms.custom: ''
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: scale-out
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
caps.latest.revision: 1
author: haoqian
ms.author: haoqian
manager: craigg
ms.openlocfilehash: 5b19aee11489551b3f06fb58cb0242b495929213
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-scale-out-worker"></a>Integration Services (SSIS) Scale Out Worker

Scale Out Worker exécute le service Scale Out Worker pour extraire des tâches d’exécution à partir de Scale Out Master. Le Worker exécute ensuite les packages localement avec `ISServerExec.exe`.

## <a name="configure-the-scale-out-worker-service"></a>Configurer le service Scale Out Worker
Vous pouvez configurer le service Scale Out Worker à l’aide du fichier ` \<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`. Vous devez redémarrer le service après la mise à jour du fichier de configuration.

Configuration  |Description  |Valeur par défaut  
---------|---------|---------
DisplayName|Nom complet du Scale Out Worker. **Pas en cours d’utilisation dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Nom de l'ordinateur         
Description|Description du Scale Out Worker. **Pas en cours d’utilisation dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Vide         
MasterEndpoint|Point de terminaison pour la connexion à Scale Out Master.|Point de terminaison défini pendant l’installation du Scale Out Worker.         
MasterHttpsCertThumbprint|Empreinte numérique du certificat client SSL utilisé pour authentifier Scale Out Master.|Empreinte numérique du certificat client spécifié pendant l’installation de Scale Out Worker.          
WorkerHttpsCertThumbprint|Empreinte numérique du certificat Scale Out Master utilisé pour authentifier le Scale Out Worker.|Empreinte numérique d’un certificat créé et installé automatiquement pendant l’installation du Scale Out Worker.          
StoreLocation|Emplacement du magasin de certificats de Worker.|LocalMachine       
StoreName|Nom du magasin où se trouve ce certificat de Worker.|My         
AgentHeartbeatInterval|Intervalle de pulsation du Scale Out Worker.|00:01:00         
TaskHeartbeatInterval|Intervalle d’état de la tâche de rapport du Scale Out Worker.|00:00:10         
HeartbeatErrorTollerance|Après ce laps de temps depuis la dernière pulsation de tâche, la tâche est arrêtée en cas de réception d’une réponse d’erreur de pulsation.|00:10:00      
TaskRequestMaxCPU|Limite supérieure d’utilisation du processeur pour la demande de tâches par le Scale Out Worker.|70.0         
TaskRequestMinMemory|Limite inférieure d’utilisation de la mémoire (en Mo) pour la demande de tâches par le Scale Out Worker.|100.0         
MaxTaskCount|Quantité maximale de tâches que peut contenir le Scale Out Worker.|10         
LeaseInternval|Intervalle de bail de détention d’une tâche par le Scale Out Worker.|00:01:00         
TasksRootFolder|Dossier des journaux des tâches. Le chemin de dossier `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Tasks` est utilisé si la valeur est vide. [compte] est le compte exécutant le service Scale Out Worker. Par défaut, le compte est SSISScaleOutWorker140.|Vide         
TaskLogLevel|Niveau de journal des tâches du Scale Out Worker. (Verbose 0x01, Information 0x02, Warning 0x04, Error 0x08, Progress 0x10, CriticalError 0x20, Audit 0x40)|126 (Information, Warning, Error, Progress, CriticalError, Audit)     
TaskLogSegment|Intervalle de temps d’un fichier journal de tâche.|00:00:00         
TaskLogEnabled|Spécifie si le journal des tâches est activé.|true         
ExecutionLogCacheFolder|Dossier utilisé pour mettre en cache le journal d’exécution de package. Le chemin de dossier ` \<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Agent\ELogCache` est utilisé si la valeur est vide. [compte] est le compte exécutant le service Scale Out Worker. Par défaut, le compte est SSISScaleOutWorker140.|Vide         
ExecutionLogMaxBufferLogCount|Quantité maximale de journaux d’exécution mis en cache, dans une mémoire tampon de journal d’exécution en mémoire.|10000        
ExecutionLogMaxInMemoryBufferCount|Quantité maximale de mémoires tampon de journal d’exécution en mémoire pour les journaux d’exécution.|10         
ExecutionLogRetryCount|Nombre de nouvelles tentatives en cas d’échec de journalisation de l’exécution.|3
ExecutionLogRetryTimeout|Délai d’expiration des nouvelles tentatives en cas d’échec de journalisation de l’exécution. ExecutionLogRetryCount est ignoré si ExecutionLogRetryTimeout est atteint. |7.00:00:00 (7 jours)        
AgentId|ID d’agent de Worker du Scale Out Worker|Généré automatiquement.    
||||    

## <a name="view-the-scale-out-worker-log"></a>Afficher le journal Scale Out Worker
Le fichier journal du service Scale Out Worker se trouve dans le dossier `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Agent`.

L’emplacement du journal de chaque tâche est configuré dans le fichier `WorkerSettings.config` dans `TasksRootFolder`. Si aucune valeur n’est spécifiée, le journal se trouve dans le dossier `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. 

Le paramètre *[account]* fait référence au compte exécutant le service Scale Out Worker. Par défaut, le compte est `SSISScaleOutWorker140`.

## <a name="next-steps"></a>Étapes suivantes
[SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
