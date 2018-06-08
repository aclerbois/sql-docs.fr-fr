---
title: Sys.dm_clr_loaded_assemblies (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 08abe7b86ec5d6c80d98bdfbead24cd3e7d20f3d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmclrloadedassemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque assembly d'utilisateur géré chargé dans l'espace d'adressage du serveur. Utilisez cette vue pour comprendre et résoudre les problèmes d’intégration du CLR géré des objets de base de données qui sont exécutent dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les assemblys sont des fichiers DLL de code managé qui sont utilisés pour définir et déployer des objets de base de données managés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dès lors qu'un utilisateur exécute un de ces objets de base de données managés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR chargent l'assembly (et ses références) dans lequel l'objet de base de données managé est défini. L'assembly reste chargé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour augmenter les performances, de sorte que les objets de base de données managés contenus dans l'assembly puissent être appelés ultérieurement, sans avoir à recharger l'assembly. L'assembly n'est pas déchargé tant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne présente pas une mémoire insuffisante. Pour plus d’informations sur les assemblys et intégration du CLR, consultez [environnement du CLR hébergé](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md). Pour plus d’informations sur les objets de base de données managés, consultez [création d’objets de base de données avec le Common Language Runtime &#40;CLR&#41; intégration](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID de l'assembly chargé. Le **assembly_id** peut être utilisé pour rechercher plus d’informations sur l’assembly dans le [sys.assemblies &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) affichage catalogue. Notez que la [!INCLUDE[tsql](../../includes/tsql-md.md)] [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) catalogue montre les assemblys dans la base de données active. Le **sqs.dm_clr_loaded_assemblies** affiche tous les assemblys chargés sur le serveur.|  
|**appdomain_address**|**int**|Adresse du domaine d’application (**AppDomain**) dans lequel l’assembly est chargé. Tous les assemblys appartenant à un seul utilisateur sont toujours chargés dans le même **AppDomain**. Le **appdomain_address** peut être utilisé pour rechercher plus d’informations sur la **AppDomain** dans les [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md) vue.|  
|**load_time**|**datetime**|Heure à laquelle l'assembly a été chargé. Notez que l’assembly reste chargé tant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une mémoire insuffisante et décharge le **AppDomain**. Vous pouvez surveiller **load_time** à comprendre à quelle fréquence [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] présente une mémoire insuffisante et décharge le **AppDomain**.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Le **dm_clr_loaded_assemblies.appdomain_address** vue a une relation plusieurs-à-un avec **dm_clr_appdomains.appdomain_address**. Le **dm_clr_loaded_assemblies.assembly_id** vue a une relation un-à-plusieurs avec **sys.assemblies.assembly_id**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment afficher les détails de tous les assemblys de la base de données active qui sont chargés.  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 L’exemple suivant montre comment afficher les détails de la **AppDomain** dans lequel un assembly donné est chargé.  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique liées à Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
