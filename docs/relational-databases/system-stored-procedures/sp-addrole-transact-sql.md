---
title: sp_addrole (Transact-SQL) | Documents Microsoft
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
- sp_addrole
- sp_addrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrole
ms.assetid: e8a21642-8440-419a-8585-93d3d9d44f00
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 01a51141bc5e28c09879667dee69721f6e5d4473
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddrole-transact-sql"></a>sp_addrole (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un rôle de base de données dans la base de données active.  
  
> [!IMPORTANT]  
>  **sp_addrole** est inclus pour la compatibilité avec les versions antérieures de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ne peut pas être pris en charge dans une version ultérieure. Utilisez [CREATE ROLE](../../t-sql/statements/create-role-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addrole [ @rolename = ] 'role' [ , [ @ownername = ] 'owner' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom du nouveau rôle de base de données. *rôle* est un **sysname**, sans valeur par défaut. *rôle* doit être un identificateur valid (ID) et ne doit pas déjà exister dans la base de données actuelle.  
  
 [  **@ownername =**] **'***propriétaire***'**  
 Propriétaire du nouveau rôle de base de données. *propriétaire* est un **sysname**, avec une valeur par défaut de l’utilisateur en cours d’exécution actuel. *propriétaire* doit être un utilisateur de base de données ou un rôle de base de données dans la base de données en cours.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Les noms des rôles de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent compter de 1 à 128 caractères, y compris des lettres, des symboles et des chiffres. Les noms de rôles de base de données ne peut pas : contenir une barre oblique inverse (\\), la valeur null, ou une chaîne vide (**''**).  
  
 Après avoir ajouté un rôle de base de données, utilisez [sp_addrolemember &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) pour ajouter des entités au rôle. Lorsque vous utilisez les instructions GRANT, DENY ou REVOKE pour appliquer des autorisations au rôle de base de données, les membres de ce rôle héritent de ces autorisations comme si elles avaient été appliquées directement à leur compte.  
  
> [!NOTE]  
>  Il n'est pas possible de créer de nouveaux rôles de serveurs. Les rôles ne peuvent être créés qu'au niveau de la base de données.  
  
 **sp_addrole** ne peut pas être utilisé à l’intérieur d’une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CREATE ROLE sur la base de données. Pour la création d'un schéma, exige l'autorisation CREATE SCHEMA sur la base de données. Si *propriétaire* est spécifié en tant qu’un utilisateur ou un groupe, requiert IMPERSONATE sur cet utilisateur ou groupe. Si *propriétaire* est spécifié en tant que rôle, nécessite l’autorisation ALTER sur ce rôle ou un membre de ce rôle. Si le propriétaire est spécifié en tant que rôle d'application, exige l'autorisation ALTER sur ce rôle d'application.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant ajoute le nouveau rôle `Managers` à la base de données active.  
  
```  
EXEC sp_addrole 'Managers';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)  
  
  
