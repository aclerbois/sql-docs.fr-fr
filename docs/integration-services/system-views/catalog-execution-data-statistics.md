---
title: catalog.execution_data_statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 6f51407e-0e4e-4b44-af33-db14c9d40ded
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8afc79fecdf3731bc1579dc354a16162f0bfe3f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogexecutiondatastatistics"></a>catalog.execution_data_statistics
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette vue affiche une ligne chaque fois qu'un composant de flux de données envoie des données à un composant en aval, pour une exécution de package donnée. Les informations de cette vue peuvent être utilisées pour calculer le débit des données pour un composant.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|data_stats_id|**bigint**|Identificateur (ID) unique des données.|  
|execution_id|**bigint**|ID unique de l'instance d'exécution.|  
|package_name|**nvarchar(260)**|Nom du premier package démarré pendant l'exécution.|  
|task_name|**nvarchar(4000)**|Nom de la tâche de flux de données.|  
|dataflow_path_id_string|**nvarchar(4000)**|Chaîne d'identification du chemin d'accès de flux de données.|  
|dataflow_path_name|**nvarchar(4000)**|Nom du chemin d'accès de flux de données.|  
|source_component_name|**nvarchar(4000)**|Nom du composant de flux de données ayant envoyé les données.|  
|destination_component_name|**nvarchar(4000)**|Nom du composant de flux de données ayant reçu les données.|  
|rows_sent|**bigint**|Nombre de lignes envoyées à partir du composant source.|  
|created_time|**datatimeoffset(7)**|Heure à laquelle les valeurs ont été obtenues.|  
|execution_path|**nvarchar(max)**|Chemin d'accès d'exécution du composant.|  
  
## <a name="remarks"></a>Notes   
  
-   Lorsqu'il existe plusieurs sorties du composant, une ligne est ajoutée pour chacune d'elles.  
  
-   Par défaut, lors du démarrage d'une exécution, les informations sur le nombre de lignes envoyées ne sont pas enregistrées.  
  
-   Pour afficher ces données pour une exécution de package donnée, définissez le niveau de journalisation sur **Verbose**. Pour plus d’informations, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
