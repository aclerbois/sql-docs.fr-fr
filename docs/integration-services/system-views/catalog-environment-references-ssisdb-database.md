---
title: catalog.environment_references (base de données SSISDB) | Microsoft Docs
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
ms.assetid: efec53ef-3e5a-4b76-b71d-a0cf9e11ac00
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c592c8957638aa2ad7a4d0f575c1fdd3213cab57
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogenvironmentreferences-ssisdb-database"></a>catalog.environment_references (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche les références environnementales pour tous les projets dans le catalogue **SSISDB**.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|reference_id|**bigint**|Identificateur (ID) unique de la référence.|  
|project_id|**bigint**|ID unique du projet.|  
|reference_type|**char(1)**|Indique si l'environnement peut se trouver dans le même dossier que le projet (référence relative) ou dans un dossier différent (référence absolue). Lorsque la valeur est `R`, l'environnement est localisé à l'aide d'une référence relative. Lorsque la valeur est `A`, l'environnement est localisé à l'aide d'une référence absolue.|  
|environment_folder_name|**sysname**|Nom du dossier si l'environnement est localisé à l'aide d'une référence absolue.|  
|environment_name|**sysname**|Nom de l'environnement qui est référencé par le projet.|  
|validation_status|**char(1)**|État de la validation.|  
|last_validation_time|**datatimeoffset(7)**|Heure de la dernière validation.|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque référence environnementale dans le catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur le projet correspondant  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**.  
  
> [!NOTE]  
>  Si vous avez l'autorisation READ sur un projet, vous avez également l'autorisation READ sur toutes les packages et les références environnementales associés à ce projet. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
## <a name="remarks"></a>Notes   
 Un projet peut avoir des références environnementales relatives ou absolues. Les références relatives font référence à l'environnement par nom et requièrent qu'il réside dans le même dossier que le projet. Les références absolues font référence à l'environnement par nom et par dossier et peuvent faire référence aux environnements qui résident dans un dossier différent que le projet. Un projet peut référencer plusieurs environnements.  
  
  
