---
title: catalog.executable_statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 3dda28d6-10d8-4294-9b5e-a6048c07faf9
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8f952429dfe0003b0b3ccc7c75565cd30702aa55
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogexecutablestatistics"></a>catalog.executable_statistics
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche une ligne pour chaque fichier exécutable exécuté, y compris chaque itération d'un fichier exécutable.  
  
 Un exécutable est une tâche ou un conteneur que vous ajoutez au flux de contrôle d'un package.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Statistics_id|BIGINT|ID unique des données.|  
|Execution_id|BIGINT|ID unique de l'instance de l'exécution.<br /><br /> La vue catalog.executions fournit des informations supplémentaires sur les exécutions. Pour plus d’informations, consultez [catalog.executions &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).|  
|Executable_id|BIGINT|ID unique du composant de package.<br /><br /> La vue catalog.executables fournit des informations supplémentaires sur les fichiers exécutables. Pour plus d’informations, consultez [catalog.executables](../../integration-services/system-views/catalog-executables.md).|  
|Execution_path|nvarchar(max)|Chemin d'exécution complet du composant de package, y compris chaque itération du composant.|  
|Start_time|datetimeoffset(7)|Heure à laquelle le fichier exécutable entre dans la phase de pré-exécution.|  
|End_time|datetimeoffset(7)|Heure à laquelle le fichier exécutable entre dans la phase de post-exécution.|  
|Execution_duration|INT|Durée passée par le fichier exécutable en phase d'exécution. Cette valeur est exprimée en millisecondes.|  
|Execution_result|SMALLINT|Les valeurs possibles sont les suivantes :<br /><br /> 0 (réussite)<br /><br /> 1 (échec)<br /><br /> 2 (achèvement)<br /><br /> 3 (annulé)|  
|Execution_value|sql_variant|Valeur retournée par l'exécution. Il s'agit d'une valeur définie par l'utilisateur.|  
  
## <a name="permissions"></a>Autorisations  
 La vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'instance de l'exécution.  
  
-   Appartenance au rôle de base de données **ssis_admin**.  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
  
