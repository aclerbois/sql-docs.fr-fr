---
title: sp_releaseapplock (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_releaseapplock_TSQL
- sp_releaseapplock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_releaseapplock
ms.assetid: 51b03c2f-0d54-40f5-9172-e747942d4a46
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6db9c114f4f3faade3334e7bf682eba197a2dbb7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spreleaseapplock-transact-sql"></a>sp_releaseapplock (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Libère un verrou appliqué à une ressource d'application.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_releaseapplock [ @Resource = ] 'resource_name'   
     [ , [ @LockOwner = ] 'lock_owner' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @Resource=] '*nom_ressource*'  
 Nom de ressource de verrou spécifié par l'application cliente. L'application doit veiller à ce que la ressource soit unique. Le nom spécifié est haché en interne en une valeur qui peut être stockée dans le gestionnaire de verrous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *resource_name* est **nvarchar (255)** sans valeur par défaut. *resource_name* est binaire, est donc la casse, indépendamment des paramètres de classement de la base de données actuelle.  
  
 [ @LockOwner=] '*lock_owner*'  
 Propriétaire du verrou, qui est la valeur de *lock_owner* lorsque le verrou a été demandé. *lock_owner* est de type **nvarchar(32)**. La valeur peut être **Transaction** (valeur par défaut) ou **Session**. Lorsque le *lock_owner* valeur est **Transaction**, par défaut ou spécifiée explicitement, sp_getapplock doit être exécutée à partir d’une transaction.  
  
 [ @DbPrincipal=] '*principal_base_de_données*'  
 Utilisateur, rôle ou rôle d'application qui dispose d'autorisations sur un objet d'une base de données. L’appelant de la fonction doit être membre du rôle de base de données fixe *database_principal*, dbo ou db_owner pour pouvoir appeler la fonction. La valeur par défaut est public.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 \>= 0 (succès) ou < 0 (échec)  
  
|Valeur|Résultat|  
|-----------|------------|  
|0|Le verrou a été libéré avec succès.|  
|-999|Indique la validation de paramètre ou une autre erreur d'appel.|  
  
## <a name="remarks"></a>Notes  
 Si une application utilise plusieurs fois sp_getapplock pour appeler la même ressource de verrou, sp_releaseapplock doit être appelée autant de fois pour libérer le verrou.  
  
 Lorsque le serveur s'interrompt pour une raison quelconque, les verrous sont libérés.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant libère le verrou associé à la transaction active sur la ressource `Form1` de la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'Form1',   
     @LockMode = 'Shared';  
EXEC sp_releaseapplock @DbPrincipal = 'dbo', @Resource = 'Form1';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-SQL&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
  
  
