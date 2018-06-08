---
title: catalog.execution_component_phases | Microsoft Docs
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
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ec2754c1a9ced1ada44d4d461e8f3e76c4bc6c66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogexecutioncomponentphases"></a>catalog.execution_component_phases
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche le temps consacré à chaque phase d'exécution par un composant de flux de données.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|Identificateur (ID) unique de la phase.|  
|execution_id|**bigint**|ID unique de l'instance d'exécution.|  
|package_name|**nvarchar(260)**|Nom du premier package démarré pendant l'exécution.|  
|task_name|**nvarchar(4000)**|Nom de la tâche de flux de données.|  
|subcomponent_name|**nvarchar(4000)**|Nom du composant de flux de données.|  
|phase|**nvarchar(128)**|Nom de la phase d'exécution.|  
|start_time|**datetimeoffset(7)**|Heure de début de la phase.|  
|end_time|**datetimeoffset(7)**|Heure de fin de la phase.|  
|execution_path|**nvarchar(max)**|Chemin d'exécution de la tâche de flux de données.|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque phase d'exécution d'un composant de flux de données, par exemple Validate, Pre-Execute, Post-Execute, PrimeOutput et ProcessInput. Chaque ligne affiche l'heure de début et de fin d'une phase d'exécution spécifique.  
  
## <a name="example"></a> Exemple  
 L’exemple suivant utilise la vue catalog.execution_component_phases pour déterminer le temps total d’exécution d’un package spécifique au cours de toutes les phases (**active_time**), ainsi que le temps total écoulé pour le package (**total_time**).  
  
> [!WARNING]  
>  La vue catalog.execution_component_phases fournit ces informations lorsque le niveau de journalisation de l'exécution du package est défini sur Performances ou Commentaires. Pour plus d’informations, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
