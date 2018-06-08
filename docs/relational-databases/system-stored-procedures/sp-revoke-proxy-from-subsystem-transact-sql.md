---
title: sp_revoke_proxy_from_subsystem (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_revoke_login_from_subsystem
- sp_revoke_login_from_subsystem_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_proxy_from_subsystem
ms.assetid: b87bc8ba-3ea8-4aed-b54b-32c3d82d9d2a
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c80739e4a888316837b22838b92f6d5919c998d0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprevokeproxyfromsubsystem-transact-sql"></a>sp_revoke_proxy_from_subsystem (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Interdit à un proxy d'accéder à un sous-système.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_revoke_proxy_from_subsystem   
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@proxy_id** =] *id*  
 Numéro d'identification du proxy auquel le droit d'accès est refusé. Le *proxy_id* est **int**, avec NULL comme valeur par défaut. Soit *proxy_id* ou *proxy_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@proxy_name** =] **'***proxy_name***'**  
 Nom du proxy auquel le droit d'accès est refusé. Le *proxy_name* est **sysname**, avec NULL comme valeur par défaut. Soit *proxy_id* ou *proxy_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@subsystem_id** =] *id*  
 Numéro d'identification du sous-système auquel il est interdit d'accéder. Le *subsystem_id* est **int**, avec NULL comme valeur par défaut. Soit *subsystem_id* ou *subsystem_name* doit être spécifié, mais ne peut pas être spécifiés. Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**2**|Script ActiveX<br /><br /> **\*\* Important \* \***  sous-système de l’un script ActiveX sera supprimé de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une future version de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
|**3**|Système d'exploitation (CmdExec)|  
|**4**|Agent d'instantané de réplication|  
|**5**|Agent de lecture du journal des réplications|  
|**6**|Agent de distribution de réplication|  
|**7**|Agent de fusion de réplication|  
|**8**|Agent de lecture de la file d'attente de réplication|  
|**9**|Commandes Analysis Services|  
|**10**|Requête Analysis Services|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] Exécution du package|  
|**12**|script PowerShell|  
  
 [ **@subsystem_name**=] **'***subsystem_name***'**  
 Nom du sous-système auquel il est interdit d'accéder. Le *subsystem_name* est **sysname**, avec NULL comme valeur par défaut. Soit *subsystem_id* ou *subsystem_name* doit être spécifié, mais ne peut pas être spécifiés. Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
|Valeur| Description|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Système d'exploitation (CmdExec)|  
|Snapshot|Agent d'instantané de réplication|  
|LogReader|Agent de lecture du journal des réplications|  
|Distribution|Agent de distribution de réplication|  
|Fusion|Agent de fusion de réplication|  
|QueueReader|Agent de lecture de la file d'attente de réplication|  
|ANALYSISQUERY|Commandes Analysis Services|  
|ANALYSISCOMMAND|Requête Analysis Services|  
|Dts|[!INCLUDE[ssIS](../../includes/ssis-md.md)] Exécution du package|  
|PowerShell|script PowerShell|  
  
## <a name="remarks"></a>Notes  
 Refuser l'accès à un sous-système ne change en rien les autorisations accordées au principal spécifié dans le proxy.  
  
> [!NOTE]  
>  Pour déterminer les étapes de travail fait référence à un proxy, cliquez sur le **proxys** nœud sous **l’Agent SQL Server** dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis cliquez sur **propriétés**. Dans le **propriétés du compte Proxy** boîte de dialogue, sélectionnez le **références** page pour afficher toutes les étapes de travail qui font référence à ce serveur proxy.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_revoke_proxy_from_subsystem**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant refuse l'accès au sous-système [!INCLUDE[ssIS](../../includes/ssis-md.md)] au proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_proxy_from_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_name = N'Dts';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implémenter la sécurité de l’Agent SQL Server](http://msdn.microsoft.com/library/d770d35c-c8de-4e00-9a85-7d03f45a0f0d)   
 [sp_grant_proxy_to_subsystem &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
