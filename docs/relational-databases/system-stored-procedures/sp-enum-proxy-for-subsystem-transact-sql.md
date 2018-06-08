---
title: sp_enum_proxy_for_subsystem (Transact-SQL) | Documents Microsoft
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
- sp_enum_proxy_for_subsystem_TSQL
- sp_enum_proxy_for_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_proxy_for_subsystems
ms.assetid: 580cc3be-1068-4a96-8d15-78ca3a5bb719
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b524d44236cb9c5a070b460a3f3a0d0736b16aca
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spenumproxyforsubsystem-transact-sql"></a>sp_enum_proxy_for_subsystem (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Répertorie les autorisations pour les proxys de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent afin d'accéder aux sous-systèmes.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_enum_proxy_for_subsystem  
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@proxy_id** =] *proxy_id*  
 Numéro d'identification du proxy pour lequel répertorier des informations. Le *proxy_id* est **int**, avec NULL comme valeur par défaut. Soit le *id* ou *proxy_name* peut être spécifié.  
  
 [ **@proxy_name** =] **'***proxy_name***'**  
 Nom du serveur proxy pour lequel énumérer les informations. Le *proxy_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *id* ou *proxy_name* peut être spécifié.  
  
 [ **@subsystem_id** =] *subsystem_id*  
 Numéro d'identification du sous-système pour lequel répertorier des informations. Le *subsystem_id* est **int**, avec NULL comme valeur par défaut. Soit le *subsystem_id* ou *subsystem_name* peut être spécifié.  
  
 [ **@subsystem_name** =] **'***subsystem_name***'**  
 Nom du sous-système pour lequel répertorier des informations. Le *subsystem_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *subsystem_id* ou *subsystem_name* peut être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Numéro d'identification du sous-système|  
|**subsystem_name**|**sysname**|Nom du sous-système.|  
|**proxy_id**|**int**|Numéro d'identification du proxy.|  
|**proxy_name**|**sysname**|Nom du proxy.|  
  
## <a name="remarks"></a>Notes  
 Lorsque aucun paramètre n’est fourni, **sp_enum_proxy_for_subsystem** répertorie des informations sur tous les proxys de l’instance de chaque sous-système.  
  
 Lorsqu’un id ou un nom de proxy est fourni, **sp_enum_proxy_for_subsystem** répertorie des sous-systèmes auxquels le proxy ait accès à. Lorsqu’un id ou un nom de sous-système est fourni, **sp_enum_proxy_for_subsystem** répertorie des proxys qui ont accès à ce sous-système.  
  
 Lorsque des informations de proxy et de sous-systèmes sont fournies, le jeu de résultats renvoie une ligne si le proxy spécifié dispose d'un accès au sous-système spécifié.  
  
 Cette procédure stockée se trouve dans **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-associations"></a>A. Création de la liste de toutes les associations  
 L'exemple suivant répertorie toutes les autorisations établies entre des proxys et des sous-systèmes dans l'instance en cours.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem ;  
GO  
```  
  
### <a name="b-determining-if-a-proxy-has-access-to-a-specific-subsystem"></a>B. Détermination de si un proxy dispose d'un accès à un sous-système spécifique  
 L'exemple suivant renvoie une ligne si le proxy `Catalog application proxy` peut accéder au sous-système `ActiveScripting`. Dans les autres cas, l'exemple renvoie un jeu de résultats vide.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem  
    @subsystem_name = 'ActiveScripting',  
    @proxy_name = 'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_grant_proxy_to_subsystem &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
