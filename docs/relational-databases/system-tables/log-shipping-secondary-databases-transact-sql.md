---
title: log_shipping_secondary_databases (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- log_shipping_secondary_databases_TSQL
- log_shipping_secondary_databases
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_secondary_databases system table
ms.assetid: ba2374af-86b8-480c-a10c-51e7c4e3ae23
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7f3daf3fa3a2e0873b5f7c62e8b0357cce858ca2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="logshippingsecondarydatabases-transact-sql"></a>log_shipping_secondary_databases (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Stocke un enregistrement par base de données secondaire dans une configuration de la copie des journaux de transaction. Cette table est stockée dans le **msdb** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**secondary_database**|**sysname**|Nom de la base de données secondaire dans la configuration de la copie des journaux de transactions.|  
|**secondary_id**|**uniqueidentifier**|ID du serveur secondaire dans la configuration d'envoi de journaux.|  
|**restore_delay**|**int**|Délai, en minutes, que le serveur doit respecter avant de restaurer un fichier de sauvegarde donné. La valeur par défaut est 0 minute.|  
|**restore_all**|**bit**|Si la valeur est 1, le serveur secondaire restaurera toutes les sauvegardes du journal de transactions disponibles lors de l'exécution du travail de restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier a été restauré.|  
|**restore_mode**|**bit**|Mode de restauration pour la base de données secondaire.<br /><br /> 0 = Restauration du journal avec l'option NORECOVERY.<br /><br /> 1 = Restauration du journal en mode STANDBY.|  
|**disconnect_users**|**bit**|Si la valeur est 1, les utilisateurs seront déconnectés de la base de données secondaire lors de l'exécution d'une opération de restauration. La valeur par défaut est de 0.|  
|**block_size**|**int**|Taille, en octets, qui définit la taille des blocs pour l'unité de sauvegarde.|  
|**buffer_count**|**int**|Nombre total de mémoires tampons utilisées par l'opération de sauvegarde ou de restauration.|  
|**max_transfer_size**|**int**|La taille, en octets, de l’entrée maximale ou de la demande de sortie émise par [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’unité de sauvegarde.|  
|**last_restored_file**|**nvarchar(500)**|Nom du dernier fichier de sauvegarde restauré dans la base de données secondaire.|  
|**last_restored_date**|**datetime**|Heure et date auxquelles a eu lieu la dernière opération de restauration de la base de données secondaire.|  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
