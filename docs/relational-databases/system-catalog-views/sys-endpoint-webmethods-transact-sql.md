---
title: Sys.endpoint_webmethods (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.endpoint_webmethods_TSQL
- sys.endpoint_webmethods
- endpoint_webmethods_TSQL
- sys.http_soap_methods_TSQL
- endpoint_webmethods
- sys.http_soap_methods
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoint_webmethods catalog view
ms.assetid: 7dad0cf6-eafa-47cf-98cc-75ba8d3c7959
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 56939639112a61054b6896a00a978f84fc4c51fb
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysendpointwebmethods-transact-sql"></a>sys.endpoint_webmethods (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Contient une ligne pour chaque méthode SOAP définie sur un point de terminaison HTTP SOAP. La combinaison des colonnes endpoint_id et espace de noms est unique.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|endpoint_id|**int**|ID du point de terminaison sur lequel la méthode Web est définie.|  
|espace de noms|**nvarchar(384)**|Espace de noms de la méthode Web.|  
|method_alias|**nvarchar(64)**|Alias de la méthode.<br /><br /> Remarque : [!INCLUDE[tsql](../../includes/tsql-md.md)] identificateurs autorisent des caractères qui ne sont pas autorisées dans les noms de méthode WSDL.<br /><br /> L'alias permet de mapper le nom exposé dans la description WSDL du point de terminaison avec l'objet exécutable [!INCLUDE[tsql](../../includes/tsql-md.md)] sous-jacent réel qui est appelé lorsque la méthode Web est invoquée.|  
|object_name|**nvarchar(776)**|Nom d'objet vers lequel la méthode Web est redirigée, tel que spécifié dans l'option NAME =. Parties du nom sont séparés par un point (.) et délimités par des crochets, `[``]`.<br /><br /> Le nom d'objet doit être composé de trois parties, tel que spécifié dans l'option WSDL.|  
|result_schema|**tinyint**|Option qui détermine le schéma XSD éventuellement retourné avec une réponse.<br /><br /> 0 = Aucun<br /><br /> 1 = standard<br /><br /> 2 = par défaut|  
|result_schema_desc|**nvarchar(60)**|Description de l'option qui détermine le schéma XSD éventuellement retourné avec une réponse.<br /><br /> Aucune<br /><br /> STANDARD<br /><br /> DEFAULT|  
|result_format|**tinyint**|Option qui détermine la mise en forme des résultats dans la réponse.<br /><br /> 1 = ALL_RESULTS<br /><br /> 2 = ROWSETS_ONLY<br /><br /> 3 = AUCUNE|  
|result_format_desc|**nvarchar(60)**|Description de l'option qui détermine la mise en forme des résultats dans la réponse.<br /><br /> ALL_RESULTS<br /><br /> ROWSETS_ONLY<br /><br /> Aucune|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
