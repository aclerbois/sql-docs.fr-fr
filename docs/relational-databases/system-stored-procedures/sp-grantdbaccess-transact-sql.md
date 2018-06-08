---
title: sp_grantdbaccess (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_grantdbaccess
- sp_grantdbaccess_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grantdbaccess
ms.assetid: 3eb09513-03f1-42f8-9917-3a1f3a579bec
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6e62b1b4a05ade827c03ac0514cdd5545fa784db
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spgrantdbaccess-transact-sql"></a>sp_grantdbaccess (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un utilisateur à la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE USER](../../t-sql/statements/create-user-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_grantdbaccess [ @loginame = ] 'login'  
    [ , [ @name_in_db = ] 'name_in_db' [ OUTPUT ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@loginame =** ]  **' *** connexion* **'** est le nom du groupe Windows, nom de connexion Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion à associer à la nouvelle base de données utilisateur. Noms des groupes Windows et les connexions Windows doivent être qualifiés avec un nom de domaine Windows sous la forme *domaine*\\*connexion *, par exemple, **LONDON\Joeb**. La connexion ne peut pas être déjà associée à un utilisateur de la base de données. *connexion* est un **sysname**, sans valeur par défaut.  
  
 [  **@name_in_db=**] **'***nom_dans_la_base_de_données***'** [ **sortie**]  
 Nom du nouvel utilisateur de base de données. *nom_dans_la_base_de_données* est une variable de sortie avec un type de données **sysname**et une valeur par défaut NULL. Si non spécifié, *connexion* est utilisé. S’il est spécifié comme variable de sortie avec une valeur NULL, **@name_in_db** a la valeur *connexion*. *nom_dans_la_base_de_données* ne doit pas déjà exister dans la base de données actuelle.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_grantdbaccess** appelle CREATE USER, qui prend en charge des options supplémentaires. Pour plus d’informations sur la création d’utilisateurs de base de données, consultez [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md). Pour supprimer un utilisateur de base de données à partir d’une base de données, utilisez [DROP USER](../../t-sql/statements/drop-user-transact-sql.md).  
  
 **sp_grantdbaccess** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **db_owner** rôle de base de données fixe ou la **db_accessadmin** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise `CREATE USER` pour ajouter un utilisateur de base de données pour la connexion Windows `Edmonds\LolanSo` à la base de données actuelle. Le nouvel utilisateur se nomme `Lolan`. Il s'agit de la méthode recommandée pour la création d'un utilisateur de base de données.  
  
```  
CREATE USER Lolan FOR LOGIN [Edmonds\LolanSo];  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [DROP USER & #40 ; Transact-SQL & #41 ;](../../t-sql/statements/drop-user-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
