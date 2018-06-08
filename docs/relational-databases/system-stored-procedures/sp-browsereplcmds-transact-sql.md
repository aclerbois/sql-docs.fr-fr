---
title: sp_browsereplcmds (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_browsereplcmds_TSQL
- sp_browsereplcmds
helpviewer_keywords:
- sp_browsereplcmds
ms.assetid: 30abcb41-1d18-4f43-a692-4c80914c0450
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 19c4c3b162d882f3d7a31701ad2cc838fcaaed74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spbrowsereplcmds-transact-sql"></a>sp_browsereplcmds (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne un jeu de résultats, dans une version lisible, des commandes répliquées et stockées dans la base de données de distribution. Également utilisé en tant qu'outil de diagnostic. Cette procédure stockée est exécutée au niveau du serveur de distribution sur la base de données de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_browsereplcmds [ [ @xact_seqno_start = ] 'xact_seqno_start' ]  
    [ , [ @xact_seqno_end = ] 'xact_seqno_end' ]   
    [ , [ @originator_id = ] 'originator_id' ]  
    [ , [ @publisher_database_id = ] 'publisher_database_id' ]  
    [ , [ @article_id = ] 'article_id' ]  
    [ , [ @command_id= ] command_id ]  
    [ , [ @agent_id = ] agent_id ]  
    [ , [ @compatibility_level = ] compatibility_level ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@xact_seqno_start =**] **'***xact_seqno_start***'**  
 Spécifie le plus petit numéro de séquence exact à retourner. *xact_seqno_start* est **nchar (22)**, avec 0 x 00000000000000000000 comme valeur par défaut.  
  
 [  **@xact_seqno_end =**] **'***xact_seqno_end***'**  
 Spécifie le plus grand numéro de séquence exact à retourner. *xact_seqno_end* est **nchar (22)**, avec 0xFFFFFFFFFFFFFFFFFFFF comme valeur par défaut.  
  
 [  **@originator_id =**] **'***originator_id***'**  
 Spécifie si les commandes avec l’objet *originator_id* sont retournées. *originator_id* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@publisher_database_id =**] **'***publisher_database_id***'**  
 Spécifie si les commandes avec l’objet *publisher_database_id* sont retournées. *publisher_database_id* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@article_id =**] **'***article_id***'**  
 Spécifie si les commandes avec l’objet *article_id* sont retournées. *article_id* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@command_id =**] *id_de_commande*  
 Est l’emplacement de la commande dans [MSrepl_commands &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/msrepl-commands-transact-sql.md) à décoder. *id_de_commande* est **int**, avec NULL comme valeur par défaut. Si spécifié, tous les autres paramètres doivent être spécifiés en outre, et *xact_seqno_start*doit être identique à *xact_seqno_end*.  
  
 [  **@agent_id =**] *agent_id*  
 Spécifie que seules les commandes correspondant à un Agent de réplication donné sont retournées. *éléments agent_id* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@compatibility_level =**] *compatibility_level*  
 Est la version de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel le *compatibility_level* est **int**, avec 9000000 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de la commande.|  
|**originator_srvname**|**sysname**|Serveur d'origine de la transaction.|  
|**originator_db**|**sysname**|Base de données d'origine de la transaction.|  
|**article_id**|**int**|ID de l’article.|  
|**type**|**int**|Type de commande.|  
|**commande_partielle**|**bit**|Indique s'il s'agit d'une commande partielle.|  
|**hashkey**|**int**|À usage interne uniquement|  
|**originator_publication_id**|**int**|ID de la publication d'origine de la transaction.|  
|**originator_db_version**|**int**|Version de la base de données d'origine de la transaction.|  
|**originator_lsn**|**varbinary(16)**|Identifie le numéro séquentiel dans le journal (LSN) de la commande dans la publication d'origine. Utilisé dans la réplication transactionnelle d’égal à égal.|  
|**commande**|**nvarchar(1024)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] commande.|  
|**id_de_commande**|**int**|ID de la commande dans [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
  
 Les commandes longues peuvent être réparties entre plusieurs lignes dans l'ensemble de résultats.  
  
## <a name="remarks"></a>Notes  
 **sp_browsereplcmds** est utilisé dans la réplication transactionnelle.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou les membres de la **db_owner** ou **replmonitor** des rôles de base de données fixe sur la base de données de distribution peuvent exécuter **sp_browsereplcmds**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replshowcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
