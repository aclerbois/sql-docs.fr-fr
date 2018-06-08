---
title: sp_addremotelogin (Transact-SQL) | Documents Microsoft
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
- sp_addremotelogin_TSQL
- sp_addremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addremotelogin
ms.assetid: 71b7cd36-a17d-4b12-b102-10aeb0f9268b
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ec988334611350fdf736b69100b27d79d5374342
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddremotelogin-transact-sql"></a>sp_addremotelogin (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouvel ID de connexion à distance sur le serveur local. Cela permet aux serveurs distants de se connecter et d'exécuter des appels de procédure distante.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilisez des serveurs liés et des procédures stockées de serveur lié à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @remoteserver **=** ] **'***remoteserver***'**  
 Nom du serveur distant auquel s'applique la connexion d'accès à distance. *RemoteServer* est **sysname**, sans valeur par défaut. Si une seule *remoteserver* est spécifié, tous les utilisateurs sur *remoteserver* sont mappés aux connexions existantes ayant le même nom sur le serveur local. Le serveur doit être connu du serveur local. Il est ajouté à l’aide de sp_addserver. Lorsque les utilisateurs sur *remoteserver* se connecter au serveur local qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, ils se connectent sous la connexion locale qui correspond à leur propre connexion sur *remoteserver*. *RemoteServer* est le serveur qui lance l’appel de procédure distante.  
  
 [ @loginame **=** ] **'***connexion***'**  
 ID de connexion de l'utilisateur sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* est de type **sysname**, avec NULL comme valeur par défaut. *connexion*doit déjà exister sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si *connexion* est spécifié, tous les utilisateurs sur *remoteserver* sont mappées à la connexion locale spécifique. Lorsque les utilisateurs sur *remoteserver* se connecter à l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, ils se connectent en tant que *connexion*.  
  
 [ @remotename **=** ] **'***nom_distant***'**  
 ID de connexion de l'utilisateur sur le serveur distant. *nom_distant* est **sysname**, avec NULL comme valeur par défaut. *nom_distant* doit exister sur *remoteserver*. Si *nom_distant* est spécifié, l’utilisateur spécifique *nom_distant* est mappé à *connexion* sur le serveur local. Lorsque *nom_distant* sur *remoteserver* se connecte à l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter une procédure stockée distante, il se connecte en tant que *connexion*. ID de connexion de *nom_distant* peut être différent de l’ID de connexion sur le serveur distant, *connexion*.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour exécuter des requêtes distribuées, utilisez la procédure sp_addlinkedsrvlogin.  
  
 sp_addremotelogin ne peut pas être utilisé à l’intérieur d’une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du sysadmin et securityadmin rôles serveur fixé peuvent exécuter sp_addremotelogin.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-mapping-one-to-one"></a>A. Mappage de un-à-un  
 L'exemple suivant mappe des noms distants à des noms locaux lorsque le serveur distant `ACCOUNTS` et le serveur local ont les mêmes connexions utilisateur.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-mapping-many-to-one"></a>B. Mappage de plusieurs-à-un  
 L'exemple suivant crée une entrée qui mappe tous les utilisateurs du serveur distant `ACCOUNTS` à la connexion locale `Albert`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-using-explicit-one-to-one-mapping"></a>C. Utilisation d'un mappage explicite de un-à-un  
 L'exemple suivant mappe une connexion distante de l'utilisateur distant `Chris` situé sur le serveur distant `ACCOUNTS` à l'utilisateur local `salesmgr`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
