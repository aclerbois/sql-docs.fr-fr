---
title: sp_update_proxy (Transact-SQL) | Documents Microsoft
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
- sp_update_proxy
- sp_update_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROXY statement
- sp_update_proxy
ms.assetid: 864fd0e6-9d61-4f07-92ef-145318d2f881
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 072c36097eb465fa43a785c59d4b0ef73d6d351f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spupdateproxy-transact-sql"></a>sp_update_proxy (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés d'un proxy existant.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @new_name = ] 'new_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@proxy_id**=] *id*  
 Numéro d'identification du proxy à modifier. Le *proxy_id* est **int**, avec NULL comme valeur par défaut.  
  
 [ **@proxy_name**=] **'***proxy_name***'**  
 Nom du proxy à modifier. Le *proxy_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@credential_name** =] **'***credential_name***'**  
 Nom relatif aux nouvelles informations d'identification du proxy. Le *credential_name* est **sysname**, avec NULL comme valeur par défaut. Soit *credential_name* ou *credential_id* peut être spécifié.  
  
 [ **@credential_id** =] *credential_id*  
 Numéro d'identification des nouvelles informations d'identification du proxy. Le *credential_id* est **int**, avec NULL comme valeur par défaut. Soit *credential_name* ou *credential_id* peut être spécifié.  
  
 [ **@new_name**=] **'***nouveau_nom***'**  
 Le nouveau nom du proxy. Le *nouveau_nom* est **sysname**, avec NULL comme valeur par défaut. Quand il est fourni, la procédure modifie le nom du proxy à *nouveau_nom*. Si cet argument est NULL, le nom du proxy reste inchangé.  
  
 [ **@enabled** =] *is_enabled*  
 Indique si le proxy est activé. Le *is_enabled* indicateur est **tinyint**, avec NULL comme valeur par défaut. Lorsque *is_enabled* est **0**, le proxy n’est pas activé et ne peut pas être utilisé par une étape de travail. Si cet argument est NULL, l'état du proxy reste inchangé.  
  
 [ **@description**=] **'***description***'**  
 Nouvelle description du proxy. Le *description* est **nvarchar (512)**, avec NULL comme valeur par défaut. Si cet argument est NULL, la description du proxy reste inchangé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Soit **@proxy_name** ou **@proxy_id** doit être spécifié. Si ces deux arguments sont spécifiés, ils doivent tous les deux référencer le même proxy, sinon la procédure stockée échoue.  
  
 Soit **@credential_name** ou **@credential_id** doit être spécifié pour modifier les informations d’identification pour le proxy. Si ces deux arguments sont spécifiés, ils doivent tous les deux référencer les mêmes informations d'identification, sinon la procédure stockée échoue.  
  
 Cette procédure modifie le proxy sans modifier son accès. Pour modifier l’accès à un serveur proxy, utilisez **sp_grant_login_to_proxy** et **sp_revoke_login_from_proxy**.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle de sécurité fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit la valeur enabled pour le proxy `Catalog application proxy` sur `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 0;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implémenter la sécurité de l’Agent SQL Server](http://msdn.microsoft.com/library/d770d35c-c8de-4e00-9a85-7d03f45a0f0d)   
 [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
