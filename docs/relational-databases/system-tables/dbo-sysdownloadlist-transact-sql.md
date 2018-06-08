---
title: dbo.sysdownloadlist (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dbo.sysdownloadlist
- sysdownloadlist_TSQL
- dbo.sysdownloadlist_TSQL
- sysdownloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sysdownloadlist system table
ms.assetid: 71087a4c-e829-488e-aa7d-a9476e2b4779
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4390fe628a879b36b42ffd1a3c3209e910eb125c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbosysdownloadlist-transact-sql"></a>dbo.sysdownloadlist (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient la file d'attente des instructions de téléchargement pour l'ensemble des serveurs cibles.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Colonne d'identité indiquant la séquence naturelle d'insertion des lignes|  
|**source_server**|**sysname**|Nom du serveur source|  
|**operation_code**|**tinyint**|Code d'opération pour le travail :<br /><br /> **1** = INS (INSERTION)<br /><br /> **2** = UPD (MISE À JOUR)<br /><br /> **3** = SUPPR (SUPPRESSION)<br /><br /> **4** = DÉMARRAGE<br /><br /> **5** = ARRÊTER|  
|**object_type**|**tinyint**|Code du type d'objet.|  
|**object_id** <sup>1</sup>|**uniqueidentifier**|Numéro d'identification de l'objet.|  
|**target_server**|**sysname**|Nom du serveur cible|  
|**error_message**|**nvarchar(1024)**|Message d'erreur si le serveur cible rencontre une erreur lors du traitement de la ligne spécifique|  
|**date_posted**|**datetime**|Date et heure à laquelle le travail a été posté vers le serveur cible|  
|**date_downloaded**|**datetime**|Date et heure à laquelle le travail a été téléchargé pour la dernière fois|  
|**status**|**tinyint**|État du travail :<br /><br /> **0** ne = pas encore téléchargé<br /><br /> **1** = téléchargé avec succès|  
|**deleted_object_name**|**sysname**|Nom de l'objet supprimé.|  
  
 <sup>1</sup> le **object_id** colonne peut être une valeur de **-1**, qui correspond à la valeur ALL si la **operation_code** colonne est une valeur de suppression.  
  
  
