---
title: log_shipping_monitor_history_detail (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- log_shipping_monitor_history_detail_TSQL
- log_shipping_monitor_history_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_history_detail system table
ms.assetid: 7080c888-323b-4206-a1ab-e6c51f9e2579
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b57136a4f8378c25892fa32c29956d89f7da9868
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="logshippingmonitorhistorydetail-transact-sql"></a>log_shipping_monitor_history_detail (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Stocke les détails de l'historique des opérations de copie des journaux de transaction. Cette table est stockée dans le **msdb** base de données.  
  
 Les tables liées à l'historique et à la surveillance sont également utilisées au niveau du serveur principal et des serveurs secondaires.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|ID principal pour la sauvegarde ou ID secondaire pour la copie ou la restauration.|  
|**agent_type**|**tinyint**|Type d'opération de copie des journaux de transaction.<br /><br /> 0 = Sauvegarde.<br /><br /> 1 = Copie.<br /><br /> 2 = Restauration.|  
|**session_id**|**int**|ID de session pour le travail de sauvegarde/copie/restauration.|  
|**database_name**|**sysname**|Nom de la base de données associée à cet enregistrement. Base de données primaire pour la sauvegarde, base de données secondaire pour la restauration ou vide pour la copie.|  
|**session_status**|**tinyint**|État de la session.<br /><br /> 0 = Démarrage.<br /><br /> 1 = Exécution.<br /><br /> 2 = Réussite.<br /><br /> 3 = Erreur.<br /><br /> 4 = Avertissement.|  
|**log_time**|**datetime**|Date et heure de création de l'enregistrement.|  
|**log_time_utc**|**datetime**|Date et heure de création de l'enregistrement, exprimée en heure UTC.|  
|**message**|**nvarchar(max)**|Texte du message.|  
  
## <a name="remarks"></a>Notes  
 Cette table contient des détails d'historique pour les agents d'envoi de journaux. Pour identifier une session d’agent, utilisez les colonnes **agent_id**, **agent_type**, et **session_id**. Pour afficher le détail de l’historique pour la session de l’agent, triez par **log_time**.  
  
 En plus d’êtres stockées sur le serveur moniteur distant, les informations relatives au serveur principal sont stockées sur le serveur principal dans son **log_shipping_monitor_history_detail** table et les informations relatives à un serveur secondaire est également stockée sur le serveur secondaire dans son **log_shipping_monitor_history_detail** table.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [log_shipping_monitor_error_detail &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)  
  
  
