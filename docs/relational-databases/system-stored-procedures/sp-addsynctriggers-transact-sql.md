---
title: sp_addsynctriggers (Transact-SQL) | Documents Microsoft
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
- sp_addsynctriggers_TSQL
- sp_addsynctriggers
helpviewer_keywords:
- sp_addsynctriggers
ms.assetid: e37d0c3b-19bf-4719-9535-96ba361372b3
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 711126c199480bd1d8ef836b9b587f51db8e4552
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddsynctriggers-transact-sql"></a>sp_addsynctriggers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée des déclencheurs sur l'Abonné, utilisés avec tous les types d'abonnements pouvant être mis à jour (mise à jour immédiate, mise à jour en attente et mise à jour immédiate avec mise à jour en attente sous forme de basculement). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
> [!IMPORTANT]  
>  Le [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) procédure doit être utilisée à la place de **sp_addsynctrigger**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) génère un script qui contient le **sp_addsynctrigger** appels.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsynctriggers [ @sub_table = ] 'sub_table'  
        , [ @sub_table_owner = ] 'sub_table_owner'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @ins_proc = ] 'ins_proc'   
        , [ @upd_proc = ] 'upd_proc'   
        , [ @del_proc = ] 'del_proc'   
        , [ @cftproc = ] 'cftproc'  
        , [ @proc_owner = ] 'proc_owner'  
    [ , [ @identity_col = ] 'identity_col' ]  
    [ , [ @ts_col = ] 'timestamp_col' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]   
        , [ @primary_key_bitmap = ] 'primary_key_bitmap'  
    [ , [ @identity_support = ] identity_support ]  
    [ , [ @independent_agent = ] independent_agent ]  
        , [ @distributor = ] 'distributor'   
    [ , [ @pubversion = ] pubversion  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@sub_table=**] **'***table_d***'**  
 Nom de la table Subscriber. *table_d* est **sysname**, sans valeur par défaut.  
  
 [  **@sub_table_owner=**] **'***propriétaire_de_table_d***'**  
 Nom du propriétaire de la table Subscriber. *l’argument propriétaire_de_table_d* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher=**] **'***publisher***'**  
 Est le nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher_db=**] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication. *publisher_db* est **sysname**, sans valeur par défaut. Si NULL est spécifié, la base de données active est utilisée.  
  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@ins_proc=**] **'***inser_proc***'**  
 Nom de la procédure stockée qui prend en charge les insertions de transactions synchrones sur le serveur de publication. *inser_proc* est **sysname**, sans valeur par défaut.  
  
 [  **@upd_proc=**] **'***Mise_à_j_proc***'**  
 Nom de la procédure stockée qui prend en charge les mises à jour de transactions synchrones sur le serveur de publication. *inser_proc* est **sysname**, sans valeur par défaut.  
  
 [  **@del_proc=**] **'***suppr_proc***'**  
 Nom de la procédure stockée qui prend en charge les suppressions de transactions synchrones sur le serveur de publication. *inser_proc* est **sysname**, sans valeur par défaut.  
  
 [  **@cftproc =** ] **'***cftproc***'**  
 Nom de la procédure générée automatiquement utilisée par les publications qui autorisent la mise à jour en attente. *cftproc* est **sysname**, sans valeur par défaut. Pour les publications autorisant la mise à jour immédiate, cette valeur est NULL. Ce paramètre s'applique aux publications qui autorisent la mise à jour en attente (mise à jour en attente et mise à jour immédiate avec mise à jour en attente sous forme de basculement).  
  
 [  **@proc_owner =** ] **'***proc_owner***'**  
 Spécifie le compte utilisateur du serveur de publication sous lequel ont été créées toutes les procédures stockées générées automatiquement pour la publication avec mise à jour (en attente et/ou immédiate). *proc_owner* est **sysname** sans valeur par défaut.  
  
 [  **@identity_col=**] **'***identity_col***'**  
 Nom de la colonne d'identité au niveau du serveur de publication. *identity_col* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@ts_col=**] **'***l’argument timestamp_col***'**  
 Nom de la **timestamp** colonne sur le serveur de publication. *l’argument timestamp_col* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@filter_clause=**] **'***filter_clause***'**  
 Clause de restriction (WHERE) qui définit un filtre horizontal. Lorsque vous entrez la clause de restriction, omettez le mot clé où. *filter_clause*est **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
 [  **@primary_key_bitmap =**] **'***bitmap_clé_primaire***'**  
 Image bitmap des colonnes clés primaires dans la table. *bitmap_clé_primaire* est **varbinary (4000)**, sans valeur par défaut.  
  
 [  **@identity_support =** ] *identity_support*  
 Active et désactive la gestion automatique des plages d'identité lorsque la mise à jour en attente est utilisée. *identity_support* est un **bits**, avec une valeur par défaut **0**. **0** signifie qu’il n’existe aucune identité de la plage prise en charge, **1** permet la gestion de plages d’identité automatique.  
  
 [  **@independent_agent =** ] *independent_agent*  
 Indique s'il existe un Agent de distribution unique (Agent indépendant) pour cette publication ou bien un Agent de distribution par paire base de données de publication/base de données d'abonnement (Agent partagé). Cette valeur reflète la valeur de la propriété independent_agent de la publication définie sur le serveur de publication. *independent_agent* est de type bit, avec une valeur par défaut **0**. Si **0**, l’agent est un Agent partagé. Si **1**, l’agent est un agent indépendant.  
  
 [  **@distributor =** ] **'***distributeur***'**  
 Est le nom du serveur de distribution. *serveur de distribution* est **sysname**, sans valeur par défaut.  
  
 [ **@pubversion**=] *pubversion*  
 Indique la version du serveur de publication. *pubversion* est **int**, avec 1 comme valeur par défaut. **1** signifie que la version du serveur de publication est [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 2 ou version antérieure ; **2** signifie que le serveur de publication est [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 3 (SP3) ou version ultérieure. *pubversion* doit être définie explicitement sur **2** lorsque la version du serveur de publication est [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] SP3 ou version ultérieure.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addsynctriggers** est utilisé par l’Agent de Distribution dans le cadre de l’abonnement. Cette procédure stockée n'est généralement pas exécutée par les utilisateurs mais peut s'avérer utile s'ils doivent configurer manuellement un abonnement sans synchronisation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addsynctriggers**.  
  
## <a name="see-also"></a>Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
