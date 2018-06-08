---
title: sp_dbfixedrolepermission (Transact-SQL) | Documents Microsoft
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
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 32ca47ff848d735c9310d894eff46c94b0c8da92
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche les autorisations d'un rôle de base de données fixe. **sp_dbfixedrolepermission** retourne des informations correctes dans [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. La sortie ne reflète pas les modifications apportées à la hiérarchie des autorisations qui ont été implémentées dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez[autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom d'un rôle de base de données fixe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide. *rôle* est **sysname**, avec NULL comme valeur par défaut. Si *rôle* n’est pas spécifié, les autorisations pour tous les rôles de base de données fixes sont affichées.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nom du rôle de base de données fixe|  
|**Autorisation**|**nvarchar (70)**|Autorisations associées **DbFixedRole**|  
  
## <a name="remarks"></a>Notes  
 Pour afficher une liste des rôles de base de données fixe, exécutez **sp_helpdbfixedrole**. Le tableau suivant présente les rôles de base de données fixes.  
  
|Rôle de base de données fixe| Description|  
|-------------------------|-----------------|  
|**db_owner**|Propriétaires de base de données|  
|**db_accessadmin**|Administrateurs de l'accès aux bases de données|  
|**db_securityadmin**|Administrateurs de la sécurité des bases de données|  
|**db_ddladmin**|Administrateurs du langage de définition de données (DDL - Data Definition Language)|  
|**db_backupoperator**|Opérateurs de sauvegarde de base de données|  
|**db_datareader**|Utilisateurs autorisés à lire les données des bases de données|  
|**db_datawriter**|Utilisateurs autorisés à écrire des données dans les bases de données|  
|**db_denydatareader**|Utilisateurs non autorisés à lire les données des bases de données|  
|**db_denydatawriter**|Utilisateurs non autorisés à écrire des données dans les bases de données|  
  
 Membres de la **db_owner** rôle fixe de base de données possède les autorisations de tous les autres rôles de base de données fixe. Pour afficher les autorisations de rôles serveur fixes, exécutez **sp_srvrolepermission**.  
  
 L'ensemble des résultats comprend les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qu'il est possible d'exécuter ainsi que d'autres activités spéciales que les membres du rôle de base de données peuvent effectuer.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 La requête suivante renvoie les autorisations de tous les rôles de base de données fixes du fait qu'elle ne spécifie pas un rôle précis.  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
