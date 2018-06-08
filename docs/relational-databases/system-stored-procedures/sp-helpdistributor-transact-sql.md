---
title: sp_helpdistributor (Transact-SQL) | Documents Microsoft
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
- sp_helpdistributor_TSQL
- sp_helpdistributor
helpviewer_keywords:
- sp_helpdistributor
ms.assetid: 37b0983e-3b69-4f0f-977e-20efce0a0b97
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e634d01d6bf241d6d626fb6c28038aa6175b2468
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpdistributor-transact-sql"></a>sp_helpdistributor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Répertorie des informations sur le serveur de distribution, la base de données de distribution, le répertoire de travail, et [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte d’utilisateur de l’Agent. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur n'importe quelle base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdistributor [ [ @distributor= ] 'distributor' OUTPUT ]  
    [ , [ @distribdb= ] 'distribdb' OUTPUT ]  
    [ , [ @directory= ] 'directory' OUTPUT ]  
    [ , [ @account= ] 'account' OUTPUT ]  
    [ , [ @min_distretention= ] min_distretention OUTPUT ]  
    [ , [ @max_distretention= ] max_distretention OUTPUT ]  
    [ , [ @history_retention= ] history_retention OUTPUT ]  
    [ , [ @history_cleanupagent= ] 'history_cleanupagent' OUTPUT ]  
    [ , [ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @local = ] 'local' ]  
    [ , [ @rpcsrvname= ] 'rpcsrvname' OUTPUT ]  
    [ , [ @publisher_type = ] 'publisher_type' OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@distributor=**] **'***distributeur***'** sortie  
 Est le nom du serveur de distribution. Serveur de distribution est **sysname**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@distribdb=**] **'***base_de_données_de_distribution***'** sortie  
 Est le nom de la base de données de distribution. *base_de_données_de_distribution* est **sysname**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@directory=**] **'***active***'** sortie  
 Est le répertoire de travail. *répertoire* est **nvarchar (255)**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@account=**] **'***compte***' sortie**  
 Compte d'utilisateur Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)]. *compte*est **nvarchar (255)**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@min_distretention=**] *rétention_de_distribution_minimale *** sortie**  
 Période de rétention minimale de la distribution, en heures. *rétention_de_distribution_minimale* est **int**, avec une valeur par défaut **-1**.  
  
 [  **@max_distretention=**] *max_distretention *** sortie**  
 Période de rétention maximale de la distribution, en heures. *max_distretention* est **int**, avec une valeur par défaut **-1**.  
  
 [  **@history_retention=**] *history_retention *** sortie**  
 Période de conservation de l'historique, en heures. *history_retention* est **int**, avec une valeur par défaut **-1**.  
  
 [  **@history_cleanupagent=**] **'***history_cleanupagent***' sortie**  
 Nom de l'Agent de nettoyage de l'historique. *history_cleanupagent* est **nvarchar (100)**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@distrib_cleanupagent =**] **'***agent_de_nettoyage_de_distribution***' sortie**  
 Est le nom de l’agent de nettoyage de distribution. *agent_de_nettoyage_de_distribution* est **nvarchar (100)**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@local=**] **'***local***'**  
 Indique si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit récupérer des valeurs sur le serveur local. *local* est **nvarchar (5)**, avec NULL comme valeur par défaut.  
  
 [  **@rpcsrvname=**] **'***nom_rpcsrv***' sortie**  
 Nom du serveur qui émet des appels de procédure à distance. *nom_rpcsrv* est **sysname**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
 [ **@publisher_type**=] **'***publisher_type***' sortie**  
 Type du serveur de publication. *publisher_type* est **sysname**, avec une valeur par défaut **%**, qui est la seule valeur qui retourne un jeu de résultats.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Serveur de distribution**|**sysname**|Nom du serveur de distribution.|  
|**Base de données de distribution**|**sysname**|Nom de la base de données de distribution.|  
|**Répertoire**|**nvarchar(255)**|Nom du répertoire de travail.|  
|**Compte**|**nvarchar(255)**|Nom du compte d'utilisateur Windows.|  
|**rétention du point de distrib min**|**int**|Période de rétention de distribution minimale.|  
|**rétention du point de distrib max**|**int**|Période maximale de rétention de distribution.|  
|**rétention de l’historique**|**int**|Période de rétention de l'historique.|  
|**agent de nettoyage de l’historique**|**nvarchar(100)**|Nom de l'Agent de nettoyage de l'historique|  
|**agent de nettoyage de distribution**|**nvarchar(100)**|Nom de l'Agent de nettoyage de distribution.|  
|**nom du serveur RPC**|**sysname**|Nom du serveur de distribution local ou distant.|  
|**nom de connexion RPC**|**sysname**|Connexion utilisée pour les appels de procédure à distance au serveur de distribution distant.|  
|**type de serveur de publication**|**sysname**|Type de serveur de publication ; il peut s'agir d'une des valeurs suivantes :<br /><br /> **MSSQLSERVER**<br /><br /> **ORACLE**<br /><br /> **ORACLE GATEWAY**|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpdistributor** est utilisée dans tous les types de réplication.  
  
 Si un ou plusieurs paramètres de sortie sont spécifiées lors de l’exécution **sp_helpdistributor**, tous les paramètres de sortie la valeur NULL sont affectées des valeurs à la sortie et aucun jeu de résultats n’est renvoyée. Si aucun paramètre de sortie n'est spécifié, un ensemble de résultats est retourné.  
  
## <a name="permissions"></a>Autorisations  
 Le résultat suivant définie les colonnes ou paramètres de sortie sont retournés aux membres de la **sysadmin** rôle serveur fixe du serveur de publication et la **db_owner** rôle de base de données fixe sur la base de données de publication :  
  
|Colonne de l'ensemble de résultats|Paramètre de sortie|  
|-----------------------|----------------------|  
|account|**@account**|  
|min distrib retention|**@min_distretention**|  
|max distrib retention|**@max_distretention**|  
|history retention|**@history_retention**|  
|history cleanup agent|**@history_cleanupagent**|  
|distribution cleanup agent|**@distrib_cleanupagent**|  
|rpc login name|aucun|  
  
 La colonne de l'ensemble de résultats suivante est retournée aux utilisateurs dans la liste d'accès aux publications sur le serveur de distribution :  
  
-   directory  
  
 Les colonnes de l'ensemble de résultats suivantes sont retournées à tous les utilisateurs.  
  
|Colonne de l'ensemble de résultats|Paramètre de sortie|  
|-----------------------|----------------------|  
|distributor|**@distributor**|  
|distribution database|**@distribdb**|  
|rpc server name|**@rpcsrvname**|  
|publisher type|**@publisher_type**|  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
