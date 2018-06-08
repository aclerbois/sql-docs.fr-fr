---
title: sp_help_proxy (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_proxy
- sp_help_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_proxy
ms.assetid: a2fce164-2b64-40c2-8f35-6eeb7844abf1
caps.latest.revision: 38
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8a64cf35c51d4857b666798debb633828b6c66b8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpproxy-transact-sql"></a>sp_help_proxy (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Répertorie les informations d'un ou plusieurs serveurs proxy.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @subsystem_name = ] 'subsystem_name' ,  
    [ @name = ] 'name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@proxy_id** =] *id*  
 Numéro d'identification du serveur proxy pour lequel énumérer les informations. Le *proxy_id* est **int**, avec NULL comme valeur par défaut. Soit le *id* ou *proxy_name* peut être spécifié.  
  
 [ **@proxy_name** =] **'***proxy_name***'**  
 Nom du serveur proxy pour lequel énumérer les informations. Le *proxy_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *id* ou *proxy_name* peut être spécifié.  
  
 [ **@subsystem_name** =] '*subsystem_name*'  
 Nom du sous-système pour lequel énumérer les serveurs proxy. Le *subsystem_name* est **sysname**, avec NULL comme valeur par défaut. Lorsque *subsystem_name* est spécifié, *nom* doit également être spécifié.  
  
 Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
|Valeur| Description|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Système d'exploitation (CmdExec)|  
|Snapshot|Agent d'instantané de réplication|  
|LogReader|Agent de lecture du journal des réplications|  
|Distribution|Agent de Distribution de réplication|  
|Fusion|Agent de fusion de réplication|  
|QueueReader|Agent de lecture de la file d'attente de réplication|  
|ANALYSISQUERY|Commandes Analysis Services|  
|ANALYSISCOMMAND|Requête Analysis Services|  
|Dts|Exécution de package SSIS|  
|PowerShell|script PowerShell|  
  
 [ **@name** =] '*nom*'  
 Nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour laquelle énumérer les serveurs proxy. Le nom est **nvarchar (256)**, avec NULL comme valeur par défaut. Lorsque *nom* est spécifié, *subsystem_name* doit également être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Numéro d'identification du proxy.|  
|**nom**|**sysname**|Nom du proxy.|  
|**credential_identity**|**sysname**|Nom du domaine Microsoft Windows et nom d'utilisateur pour les informations d'identification associées au serveur proxy.|  
|**enabled**|**tinyint**|Indique si ce serveur proxy est activé. { **0** = non activé, **1** = activé}|  
|**description**|**nvarchar(1024)**|Description de ce serveur proxy.|  
|**user_sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) Windows de l'utilisateur Windows pour ce serveur proxy.|  
|**credential_id**|**int**|Identifiant des informations d'identification associées à ce serveur proxy.|  
|**credential_identity_exists**|**int**|Indique si l'identifiant des informations d'identification existe. { 0 = inexistant, 1 = existant }|  
  
## <a name="remarks"></a>Notes  
 Lorsque aucun paramètre n’est fourni, **sp_help_proxy** répertorie des informations pour tous les proxys de l’instance.  
  
 Pour déterminer les serveurs proxy qu’une connexion peuvent utiliser pour un sous-système donné, spécifiez *nom* et *subsystem_name*. Lorsque ces arguments sont fournis, **sp_help_proxy** répertorie les serveurs proxy auxquels la connexion spécifiée peut accéder et qui peuvent être utilisés pour le sous-système spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer du rôle de base de données fixe **SQLAgentOperatorRole** dans la base de données **msdb** .  
  
 Pour plus d’informations sur les **SQLAgentOperatorRole**, consultez [SQL Server Agent Fixed Database Roles](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
> [!NOTE]  
>  Le **credential_identity** et **user_sid** colonnes sont retournées uniquement dans le jeu de résultats lorsque les membres de **sysadmin** exécuter cette procédure stockée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-all-proxies"></a>A. Affichage des informations pour tous les serveurs proxy  
 L'exemple ci-dessous répertorie les informations pour tous les serveurs proxy de l'instance.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-proxy"></a>B. Affichage des informations pour un serveur proxy spécifique  
 L'exemple ci-dessous répertorie les informations pour le serveur proxy nommé `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)  
  
  
