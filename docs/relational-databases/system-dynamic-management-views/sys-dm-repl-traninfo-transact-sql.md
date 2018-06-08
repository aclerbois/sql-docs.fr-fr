---
title: Sys.dm_repl_traninfo (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_traninfo
- dm_repl_traninfo
- sys.dm_repl_traninfo_TSQL
- dm_repl_traninfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_traninfo dynamic management view
ms.assetid: 5abe2605-0506-46ec-82b5-6ec08428ba13
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 20a15bd329da102b45b3f611a9cbe86651ba2b3d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmrepltraninfo-transact-sql"></a>sys.dm_repl_traninfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur chaque transaction de capture de données répliquées ou modifiées.  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**fp2p_pub_exists**|**tinyint**|Indique si la transaction se trouve dans une base de données publiée à l'aide de la réplication transactionnelle d'égal à égal. Si tel est le cas, la valeur est 1 ; sinon, 0.|  
|**db_ver**|**int**|Version de base de données.|  
|**comp_range_address**|**varbinary(8)**|Définit une plage d'annulations partielles à ignorer.|  
|**textinfo_address**|**varbinary(8)**|Adresse en mémoire de la structure d'informations textuelles mises en cache.|  
|**fsinfo_address**|**varbinary(8)**|Adresse en mémoire de la structure d'informations FILESTREAM mises en cache.|  
|**begin_lsn**|**nvarchar(64)**|Numéro séquentiel dans le journal (NSE) de l'enregistrement du début pour la transaction.|  
|**commit_lsn**|**nvarchar(64)**|Numéro de séquence de l'enregistrement du journal de validation pour la transaction.|  
|**dbid**|**smallint**|ID de la base de données.|  
|**Lignes**|**int**|ID de la commande répliquée à l'intérieur de la transaction.|  
|**xdesid**|**nvarchar(64)**|ID de transaction.|  
|**artcache_table_address**|**varbinary(8)**|Adresse en mémoire de la dernière structure de table d'article mis en cache utilisée pour cette transaction.|  
|**server**|**nvarchar(514)**|Nom du serveur.|  
|**server_len_in_bytes**|**smallint**|Longueur des caractères, en octets, du nom du serveur.|  
|**database**|**nvarchar(514)**|Nom de la base de données.|  
|**db_len_in_bytes**|**smallint**|Longueur des caractères, en octets, du nom de la base de données.|  
|**donneur d’ordre**|**nvarchar(514)**|Nom du serveur sur lequel la transaction a débuté.|  
|**originator_len_in_bytes**|**smallint**|Longueur des caractères, en octets, du serveur sur lequel la transaction a débuté.|  
|**orig_db**|**nvarchar(514)**|Nom de la base de données où la transaction a débuté.|  
|**orig_db_len_in_bytes**|**smallint**|Longueur des caractères, en octets, de la base de données où la transaction a débuté.|  
|**cmds_in_tran**|**int**|Nombre de commandes répliquées dans la transaction active, permettant de déterminer quand une transaction logique doit être validée.|  
|**is_boundedupdate_singleton**|**tinyint**|Indique si une mise à jour de colonne unique affecte une seule ligne.|  
|**begin_update_lsn**|**nvarchar(64)**|Numéro de séquence d'enregistrement utilisé dans une mise à jour de colonne unique.|  
|**delete_lsn**|**nvarchar(64)**|Numéro de séquence d'enregistrement à supprimer dans le cadre d'une mise à jour.|  
|**last_end_lsn**|**nvarchar(64)**|Dernier numéro d'enregistrement de séquence d'une transaction logique.|  
|**fcomplete**|**tinyint**|Spécifie si la commande est une mise à jour partielle.|  
|**fcompensated**|**tinyint**|Spécifie si la transaction est impliquée dans une restauration partielle.|  
|**fprocessingtext**|**tinyint**|Spécifie si la transaction inclut une colonne avec un type de données binary large.|  
|**max_cmds_in_tran**|**int**|Nombre maximum de commandes dans une transaction logique, comme le spécifie l'Agent de lecture du journal.|  
|**begin_time**|**datetime**|Heure de démarrage de la transaction.|  
|**commit_time**|**datetime**|Heure de que la transaction a été validée.|  
|**session_id**|**int**|ID de la session d'analyse du journal des captures de données modifiées. Cette colonne est mappée à la **session_id** colonne [sys.dm_cdc_logscan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**session_phase**|**int**|Nombre qui indique la phase de la session au moment où l'erreur s'est produite. Cette colonne est mappée à la **phase_number** colonne [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md).|  
|**is_known_cdc_tran**|**bit**|Indique que la transaction est suivie par la capture de données modifiées.<br /><br /> 0 = Transaction de réplication de transactions.<br /><br /> 1 = Transaction de capture de données modifiées.|  
|**error_count**|**int**|Nombre d'erreurs rencontrées.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE sur la base de données de publication ou sur la base de données activée pour la capture de données modifiées.  
  
## <a name="remarks"></a>Notes  
 Les informations ne sont retournées que pour les objets de base de données répliqués ou les tables activées pour la capture de données modifiées actuellement chargés dans le cache des articles.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la réplication &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)   
 [Vues de gestion dynamique liées à la capture des données modifiées &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/2a771d7d-693a-4f56-9227-02cd00e0e200)  
  
  

