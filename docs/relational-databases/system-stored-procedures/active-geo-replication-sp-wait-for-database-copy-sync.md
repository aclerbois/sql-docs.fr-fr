---
title: sp_wait_for_database_copy_sync (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: ''
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-stored-procedures
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_wait_for_database_copy_sync_TSQL
- sp_wait_for_database_copy_sync
dev_langs:
- TSQL
helpviewer_keywords:
- sp_wait_for_database_copy_sync
ms.assetid: 7068da7f-cb74-47f2-b064-eb076a0d3885
caps.latest.revision: 13
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6658ab7b1b68637978ca76ea709c4c0a23414d07
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="active-geo-replication---spwaitfordatabasecopysync"></a>Géo-réplication Active - sp_wait_for_database_copy_sync
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Cette procédure aborde la relation [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] entre une base de données primaire et une base de données secondaire. Appel de la **sp_wait_for_database_copy_sync** entraîne l’application d’attendre que toutes les transactions validées sont répliquées et acceptées par la base de données secondaire active. Exécutez **sp_wait_for_database_copy_sync** uniquement la base de données primaire.  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_wait_for_database_copy_sync [ @target_server = ] 'server_name'   
     , [ @target_database = ] 'database_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @target_server =] 'nom_serveur'  
 Nom du serveur SQL Database qui héberge la base de données secondaire active. server_name est de type sysname, sans valeur par défaut.  
  
 [ @target_database =] 'nom_base_de_données'  
 Nom de la base de données secondaire active. database_name est de type sysname, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Retourne 0 en cas de réussite ou un numéro d'erreur en cas d'échec.  
  
 Les conditions d'erreur les plus probables sont les suivantes :  
  
-   Le nom du serveur ou le nom de la base de données est manquant.  
  
-   Le lien est introuvable sur le nom du serveur ou la base de données spécifié.  
  
-   La connectivité de l'interlien est perdue. **sp_wait_for_database_copy_sync** est retourné après le délai de connexion.  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur dans la base de données primaire peut appeler cette procédure stockée système. La connexion doit être un utilisateur dans les bases de données primaire et secondaire active.  
  
## <a name="remarks"></a>Notes  
 Toutes les transactions validées avant un **sp_wait_for_database_copy_sync** appel sont envoyées à la base de données secondaire active.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant appelle **sp_wait_for_database_copy_sync** pour vous assurer que toutes les transactions sont validées dans la base de données primaire, db0, envoyées à sa base de données secondaire active sur l’ubfyu5ssyt du serveur cible.  
  
```  
USE db0;  
GO  
EXEC sys.sp_wait_for_database_copy_sync @target_server = N'ubfyu5ssyt1', @target_database = N'db0';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.dm_continuous_copy_status &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-continuous-copy-status-azure-sql-database.md)   
 [Fonctions et vues de gestion dynamique de géo-réplication &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)  
  
  
