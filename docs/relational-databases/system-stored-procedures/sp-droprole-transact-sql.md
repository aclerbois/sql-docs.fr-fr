---
title: sp_droprole (Transact-SQL) | Documents Microsoft
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
- sp_droprole
- sp_droprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droprole
ms.assetid: 889ee074-00f8-40a9-bddb-d7d3ef0cbc19
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 39ef6031494b8e5bd4718c26d7635be4f916517e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdroprole-transact-sql"></a>sp_droprole (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime un rôle de base de données de la base de données actuelle.  
  
> [!IMPORTANT]  
>  Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], **sp_droprole** a été remplacé par l’instruction DROP ROLE. **sp_droprole** est inclus uniquement pour la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ne peut pas être pris en charge dans une version ultérieure.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_droprole [ @rolename= ] 'role'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom du rôle de base de données à supprimer de la base de données active. *rôle* est un **sysname**, sans valeur par défaut. *rôle* doit déjà exister dans la base de données actuelle.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Seuls les rôles de base de données peuvent être supprimés à l’aide de **sp_droprole**.  
  
 Il est impossible de supprimer un rôle de base de données s'il possède des membres. Il faut supprimer tous les membres d'un rôle de base de données avant de supprimer ce rôle. Pour supprimer les utilisateurs d’un rôle, utilisez **sp_droprolemember**. Si les utilisateurs sont toujours membres du rôle, **sp_droprole** affiche ces membres.  
  
 Rôles fixes et **public** rôle ne peut pas être supprimé.  
  
 Il n'est pas possible de supprimer un rôle s'il est propriétaire d'éléments sécurisables. Avant de supprimer un rôle d'application propriétaire d'éléments sécurisables, vous devez transférer la propriété de ces éléments sécurisables ou les supprimer. Utilisez ALTER AUTHORIZATION pour changer le propriétaire des objets qui ne doivent pas être supprimés.  
  
 **sp_droprole** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Il faut avoir l'autorisation CONTROL sur le rôle.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime le rôle d'application `Sales`.  
  
```  
EXEC sp_droprole 'Sales';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_dropapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropapprole-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
