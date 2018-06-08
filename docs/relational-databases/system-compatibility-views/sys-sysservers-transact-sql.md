---
title: Sys.sysservers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sysservers
- sysservers_TSQL
- sysservers
- sys.sysservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysservers system table
- sys.sysservers compatibility view
ms.assetid: d02f186f-c00f-44a6-b38d-dc78a3d2145b
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: a052eecd27de7767f721bc71a070eb00dad99220
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssysservers-transact-sql"></a>sys.sysservers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque serveur auquel une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut accéder en tant que source de données OLE DB.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**srvid**|**smallint**|ID (pour utilisation locale uniquement) du serveur distant.|  
|**srvstatus**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nom du serveur.|  
|**srvproduct**|**sysname**|Nom de produit du serveur distant|  
|**ProviderName**|**nvarchar(128)**|Nom du fournisseur OLE DB pour l'accès au serveur.|  
|**datasource**|**nvarchar(4000)**|Valeur de source de données OLE DB.|  
|**Emplacement**|**nvarchar(4000)**|Valeur d'emplacement OLE DB.|  
|**ProviderString**|**nvarchar(4000)**|Valeur de chaîne de fournisseur OLE DB.|  
|**schemadate**|**datetime**|Date de la dernière mise à jour de la ligne.|  
|**TopologyX**|**int**|Non utilisé.|  
|**TopologyY**|**int**|Non utilisé.|  
|**catalog**|**sysname**|Catalogue utilisé lors de la connexion à un fournisseur OLE DB.|  
|**srvcollation**|**sysname**|Classement du serveur.|  
|**connecttimeout**|**int**|Paramètre de délai d'expiration de la connexion serveur.|  
|**querytimeout**|**int**|Paramètre de délai d'expiration des requêtes envoyées au serveur.|  
|**srvnetname**|**char(30)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**isremote**|**bit**|1 = serveur distant<br /><br /> 0 = serveur lié|  
|**rpc**|**bit**|1 = **sp_serveroption@rpc** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@rpc** la valeur **false** ou **hors**.|  
|**pub**|**bit**|1 = **sp_serveroption@pub** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@pub** la valeur **false** ou **hors**.|  
|**sub**|**bit**|1 = **sp_serveroption@sub** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@sub** la valeur **false** ou **hors**.|  
|**serveur de distribution**|**bit**|1 = **sp_serveroption@dist** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@dist** la valeur **false** ou **hors**.|  
|**dpub**|**bit**|1 = **sp_serveroption@dpub** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@dpub** la valeur **false** ou **hors**.|  
|**rpcout**|**bit**|1 =  **sp_serveroption@rpc hors** la valeur **true** ou **sur**.<br /><br /> 0 =  **sp_serveroption@rpc hors** la valeur **false** ou **hors**.|  
|**dataaccess**|**bit**|1 =  **sp_serveroption@data accès** la valeur **true** ou **sur**.<br /><br /> 0 =  **sp_serveroption@data accès** la valeur **false** ou **hors**.|  
|**collationcompatible**|**bit**|1 =  **sp_serveroption@collation compatible** la valeur **true** ou **sur**.<br /><br /> 0 =  **sp_serveroption@collation compatible** la valeur **false** ou **hors**.|  
|**system**|**bit**|1 = **sp_serveroption@system** la valeur **true** ou **sur**.<br /><br /> 0 = **sp_serveroption@system** la valeur **false** ou **hors**.|  
|**useremotecollation**|**bit**|1 =  **sp_serveroption@remote classement** la valeur **true** ou **sur**.<br /><br /> 0 =  **sp_serveroption@remote classement** la valeur **false** ou **hors**.|  
|**lazyschemavalidation**|**bit**|1 =  **sp_serveroption@lazy validation de schéma** la valeur **true** ou **sur**.<br /><br /> 0 =  **sp_serveroption@lazy validation de schéma** la valeur **false** ou **hors**.|  
|**Classement**|**sysname**|Classement du serveur en tant que jeu par  **sp_serveroption@collation nom**.|  
|**nonsqlsub**|bit|0 : le serveur est une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> 1 : le serveur n'est pas une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
