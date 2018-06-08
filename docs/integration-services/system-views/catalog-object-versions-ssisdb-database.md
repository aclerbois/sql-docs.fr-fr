---
title: catalog.object_versions (base de données SSISDB) | Microsoft Docs
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
ms.assetid: 2fd8c020-1c77-4702-8e6b-efa6a348daab
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 37697f5a74949ffe245b284ab486e9888b1350c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogobjectversions-ssisdb-database"></a>catalog.object_versions (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche les versions d'objets dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Dans cette version, seules les versions des projets sont prises en charge par cette vue.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_version_lsn|**bigint**|Identificateur unique (ID) de la version de l'objet. Ce nombre n'est pas obligatoirement séquentiel.|  
|object_id|**bigint**|ID unique de l'objet.|  
|object_type|**smallint**|Type de l'objet. Une valeur de `20` sera affichée pour les projets.|  
|object_name|**sysname(nvarchar(128))**|Nom de l'objet.|  
|description|**nvarchar(1024)**|Description du projet.|  
|created_by|**nvarchar(128)**|Nom de l'utilisateur qui a ajouté l'objet au catalogue.|  
|created_time|**datetimeoffset**|Date et heure auxquelles l'objet a été ajouté au catalogue.|  
|restored_by|**nvarchar(128)**|Nom de l'utilisateur qui a restauré l'objet.|  
|last_restored_time|**datetimeoffset**|Date et heure auxquelles l'objet a été restauré dernièrement.|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque version d'un objet dans le catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Pour consulter des lignes dans cette vue, vous devez avoir l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'objet  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
