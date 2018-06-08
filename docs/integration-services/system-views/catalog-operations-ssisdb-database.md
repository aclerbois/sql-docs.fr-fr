---
title: catalog.operations (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- operations view [Integration Services]
- catalog.operations view [Integration Services]
ms.assetid: 9455c5b1-60ff-45fc-8599-cc3abbd6daf5
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 91312502c10bf515e08328b5bab52c73e7098143
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogoperations-ssisdb-database"></a>catalog.operations (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche les détails de toutes les opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|operation_id|**bigint**|Identificateur (ID) unique de l'opération.|  
|operation_type|**smallint**|Type d’opération.|  
|created_time|**datetimeoffset**|Heure à laquelle l'opération a été créée.|  
|object_type|**smallint**|Type d'objet affecté par l'opération. L'objet peut être un dossier (`10`), un projet (`20`), un package (`30`), un environnement (`40`) ou une instance d'exécution (`50`).|  
|object_id|**bigint**|ID de l'objet affecté par l'opération.|  
|object_name|**nvarchar(260)**|Nom de l'objet.|  
|status|**Int**|État de l'opération. Les valeurs possibles sont Créé (`1`), En cours d'exécution (`2`), Annulé (`3`), Échec (`4`), En attente (`5`), Arrêté prématurément (`6`), Opération réussie (`7`), Arrêt en cours (`8`) et Fin (`9`).|  
|start_time|**datetimeoffset**|Heure de début de l'opération, si disponible.|  
|end_time|**datetimeoffsset**|Heure à laquelle l'opération s'est terminée.|  
|caller_sid|**varbinary(85)**|ID de sécurité (SID) de l'utilisateur si l'Authentification Windows a été utilisée pour se connecter.|  
|caller_name|**nvarchar(128)**|Nom du compte qui a effectué l'opération.|  
|process_id|**Int**|ID de processus externe, le cas échéant.|  
|stopped_by_sid|**varbinary(85)**|SID de l'utilisateur qui a arrêté l'opération.|  
|stopped_by_name|**nvarchar(128)**|Nom de l'utilisateur qui a arrêté l'opération.|  
|server_name|**nvarchar(128)**|Informations relatives à l'instance et au serveur Windows pour une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nom de l'ordinateur sur lequel s'exécute l'instance du serveur.|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque opération dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Elle permet à l'administrateur d'énumérer toutes les opérations logiques effectuées sur le serveur, telles que le déploiement d'un projet ou l'exécution d'un package.  
  
 Cette vue affiche les types d’opération suivants, répertoriés dans la colonne **operation_type** :  
  
|Valeur **operation_type**|Description **operation_type**|Description **object_id**|Description **object_name**|  
|-------------------------------|-------------------------------------|--------------------------------|----------------------------------|  
|`1`|Initialisation [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|**NULL**|**NULL**|  
|`2`|Période de conservation<br /><br /> (Travail de l'Agent SQL)|**NULL**|**NULL**|  
|`3`|MaxProjectVersion<br /><br /> (Travail de l'Agent SQL)|**NULL**|**NULL**|  
|`101`|**deploy_project**<br /><br /> (Procédure stockée)|ID de projet|Nom du projet|  
|`106`|**restore_project**<br /><br /> (Procédure stockée)|ID de projet|Nom du projet|  
|`200`|**create_execution** et **start_execution**<br /><br /> (Procédures stockées)|ID de projet|**NULL**|  
|`202`|**stop_operation**<br /><br /> (Procédure stockée)|ID de projet|**NULL**|  
|`300`|**validate_project**<br /><br /> (Procédure stockée)|ID de projet|Nom du projet|  
|`301`|**validate_package**<br /><br /> (Procédure stockée)|ID de projet|Nom du package|  
|`1000`|**configure_catalog**<br /><br /> (Procédure stockée)|**NULL**|**NULL**||  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
