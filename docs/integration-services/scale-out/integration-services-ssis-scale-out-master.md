---
title: SQL Server Integration Services (SSIS) Scale Out Master | Microsoft Docs
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
ms.openlocfilehash: 2f0e604ff66388d351cbb4cf7092c0b6fe5edfea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-scale-out-master"></a>Integration Services (SSIS) Scale Out Master
Scale Out Master gère le système Scale Out par le biais du catalogue SSISDB et du service Scale Out Master. 

Le catalogue SSISDB stocke toutes les informations sur les Scale Out Workers, les packages et les exécutions. Il fournit l’interface pour activer un Scale Out Worker et exécuter des packages dans Scale Out. Pour plus d’informations, consultez [Procédure pas à pas : Configurer Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md) et [Exécuter des packages dans Integration Services](run-packages-in-integration-services-ssis-scale-out.md).

Le service Scale Out Master est un service Windows responsable de la communication avec les Scale Out Workers. Il retourne l’état des exécutions des packages sur les Scale Out Workers via le protocole HTTPS et opère sur les données dans SSISDB. 

## <a name="scale-out-views-and-stored-procedures-in-ssisdb"></a>Procédures stockées et vues Scale Out dans SSISDB

### <a name="views"></a>Vues :
-   [[catalog].[master_properties]](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)
-   [[catalog].[worker_agents]](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md).

### <a name="stored-procedures"></a>Procédures stockées :

-   Pour la gestion des Scale Out Workers :  
    -   [[catalog].[disable_worker_agent]](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)
    -   [[catalog].[enable_worker_agent]](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md).

- Pour l’exécution des packages dans Scale Out :   
    -   [[catalog].[create_execution]](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)
    -   [[catalog].[add_execution_worker]](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)
    -   [[catalog].[start_execution]](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).   

## <a name="configure-the-scale-out-master-service"></a>Configurer le service Scale Out Master
Vous configurez le service Scale Out Master à l’aide du fichier `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`. Vous devez redémarrer le service après la mise à jour du fichier de configuration.


Configuration  |Description  |Valeur par défaut  
---------|---------|---------
PortNumber|Numéro de port réseau utilisé pour communiquer avec un Scale Out Worker.|8391         
SSLCertThumbprint|Empreinte numérique du certificat SSL utilisé pour protéger les communications avec un Scale Out Worker.|Empreinte numérique du certificat SSL spécifié pendant l’installation de Scale Out Master.         
SqlServerName|Nom du serveur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] qui contient le catalogue SSISDB. Par exemple, nom_serveur\\\\nom_instance.|Nom du serveur SQL Server installé avec Scale Out Master.         
CleanupCompletedJobsIntervalInMs|Intervalle de nettoyage des tâches d’exécution terminées, en millisecondes.|43200000         
DealWithExpiredTasksIntervalInMs|Intervalle de traitement des tâches d’exécution ayant expiré, en millisecondes.|300000
MasterHeartbeatIntervalInMs|Intervalle de pulsation de Scale Out Master, en millisecondes. Cette propriété spécifie l’intervalle auquel Scale Out Master met à jour son état de connexion dans le catalogue SSISDB.|30 000
SqlConnectionTimeoutInSecs|Délai en secondes de la connexion SQL à SSISDB.|15    
||||    

## <a name="view-the-scale-out-master-service-log"></a>Afficher le journal du service Scale Out Master
Le fichier journal du service Scale Out Master se trouve dans le dossier `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Master`. 

Le paramètre *[account]* fait référence au compte exécutant le service Scale Out Master. Par défaut, ce compte est `SSISScaleOutMaster140`.

## <a name="next-steps"></a>Étapes suivantes
[SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
