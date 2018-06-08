---
title: sp_changeobjectowner (Transact-SQL) | Documents Microsoft
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
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: db97bba80119f8d460b221bcbfdb4932e1eee692
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spchangeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie le propriétaire d'un objet dans la base de données active.  
  
> [!IMPORTANT]  
>  Cette procédure stockée ne fonctionne qu’avec les objets disponibles dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [ALTER SCHEMA](../../t-sql/statements/alter-schema-transact-sql.md) ou [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) à la place. **sp_changeobjectowner** modifie le schéma et le propriétaire. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette procédure stockée modifie les propriétaires d'objet uniquement si les schémas, dont les noms sont identiques aux noms d'utilisateur de base de données, appartiennent à la fois au propriétaire actuel et au nouveau propriétaire.  
  
> [!IMPORTANT]  
>  Une nouvelle autorisation a été ajoutée à cette procédure stockée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@objname =** ] **'***objet***'**  
 Nom d'une table, d'une vue, d'une fonction définie par l'utilisateur ou d'une procédure stockée dans la base de données actuelle. *objet* est un **nvarchar(776)**, sans valeur par défaut. *objet* peut être qualifié avec le propriétaire de l’objet existant, sous la forme *propriétaire_existant.objet ***.*** objet* si le schéma et son propriétaire ont le même nom.  
  
 [  **@newowner=**] **' *** propriétaire* **'**  
 Nom du compte de sécurité qui sera le nouveau propriétaire de l'objet. *propriétaire* est **sysname**, sans valeur par défaut. *propriétaire* doit être un utilisateur de base de données valide, le rôle de serveur, [!INCLUDE[msCoName](../../includes/msconame-md.md)] connexion Windows ou du groupe Windows ayant accès à la base de données actuelle. Un utilisateur de base de données est créé si le nouveau propriétaire est un utilisateur Windows ou un groupe Windows pour lequel il n'existe pas de principal de base de données correspondant.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changeobjectowner** supprime toutes les autorisations existantes de l’objet. Vous devez réappliquer toutes les autorisations que vous souhaitez conserver après l’exécution **sp_changeobjectowner**. Par conséquent, nous vous recommandons de générer le script des autorisations existantes avant d’exécuter **sp_changeobjectowner**. Après avoir modifié la propriété de l'objet, vous pouvez utiliser le script afin de réappliquer les autorisations. Vous devez modifier le propriétaire de l'objet dans le script des autorisations avant l'exécution.  
  
 Pour modifier le propriétaire d'un élément sécurisable, utilisez ALTER AUTHORIZATION. Pour modifier un schéma, utilisez ALTER SCHEMA.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **db_owner** fixe du rôle de base de données, ou l’appartenance à la fois dans le **db_ddladmin** rôle fixe de base de données et la **db_securityadmin** fixe de base de données ainsi que l’autorisation CONTROL sur l’objet.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie le propriétaire de la `authors` table `Corporate\GeorgeW`.  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
