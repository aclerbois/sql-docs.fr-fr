---
title: sp_grant_login_to_proxy (Transact-SQL) | Documents Microsoft
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
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e07e03296c9264245504b65136a467e0fe7040e2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spgrantlogintoproxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Accorde à un principal de sécurité les droits d'accès à un proxy.  

  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@login_name** =] **'***login_name***'**  
 Nom d'accès auquel le droit d'accès est octroyé. Le *login_name* est **nvarchar (256)**, avec NULL comme valeur par défaut. Un des **@login_name**, **@fixed_server_role**, ou **@msdb_role** doit être spécifié, ou la procédure stockée échoue.  
  
 [ **@fixed_server_role**=] **'***fixed_server_role***'**  
 Rôle de serveur fixe auquel le droit d'accès est octroyé. Le *fixed_server_role* est **nvarchar (256)**, avec NULL comme valeur par défaut. Un des **@login_name**, **@fixed_server_role**, ou **@msdb_role** doit être spécifié, ou la procédure stockée échoue.  
  
 [ **@msdb_role**=] '*msdb_role*'  
 Le rôle de base de données dans le **msdb** pour accorder l’accès à base de données. Le *msdb_role* est **nvarchar (256)**, avec NULL comme valeur par défaut. Un des **@login_name**, **@fixed_server_role**, ou **@msdb_role** doit être spécifié, ou la procédure stockée échoue.  
  
 [ **@proxy_id**=] *id*  
 Identificateur du proxy pour lequel le droit d'accès est octroyé. Le *id* est **int**, avec NULL comme valeur par défaut. Un des **@proxy_id** ou **@proxy_name** doit être spécifié, ou la procédure stockée échoue.  
  
 [ **@proxy_name**=] **'***proxy_name***'**  
 Nom du proxy pour lequel le droit d'accès est octroyé. Le *proxy_name* est **nvarchar (256)**, avec NULL comme valeur par défaut. Un des **@proxy_id** ou **@proxy_name** doit être spécifié, ou la procédure stockée échoue.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_grant_login_to_proxy** doit être exécuté à partir de la **msdb** base de données.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peut exécuter **sp_grant_login_to_proxy**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant permet la connexion `adventure-works\terrid` d’utiliser le proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
