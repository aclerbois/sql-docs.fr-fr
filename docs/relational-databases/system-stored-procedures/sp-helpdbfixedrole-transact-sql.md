---
title: sp_helpdbfixedrole (Transact-SQL) | Documents Microsoft
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
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2a1ca726e6770fd4a36e34651a83c55ab1e5475b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une liste des rôles de base de données fixes.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom d'un rôle de base de données fixe. *rôle* est **sysname**, avec NULL comme valeur par défaut. Si *rôle* est spécifié, uniquement les informations sur ce rôle sont retournées ; sinon, une liste et une description de tous les rôles de base de données fixe est retournée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|nom du rôle de base de données fixe.|  
|**Description**|**nvarchar (70)**|Description de **DbFixedRole.**|  
  
## <a name="remarks"></a>Notes  
 Les rôles de base de données fixes, tels que répertoriés dans le tableau ci-dessous, sont définis au niveau de la base de données et possèdent les autorisations leur permettant d'effectuer des opérations d'administration spécifiques au niveau de la base de données. Il est impossible d'ajouter ou de supprimer des rôles de base de données fixes. Les autorisations accordées à un rôle de base de données fixe ne peuvent pas être modifiées.  
  
|Rôle de base de données fixe| Description|  
|-------------------------|-----------------|  
|**db_owner**|Propriétaires de base de données|  
|**db_accessadmin**|Administrateurs de l'accès aux bases de données|  
|**db_securityadmin**|Administrateurs de la sécurité des bases de données|  
|**db_ddladmin**|Administrateurs des DDL de base de données|  
|**db_backupoperator**|Opérateurs de sauvegarde de base de données|  
|**db_datareader**|Utilisateurs autorisés à lire les données des bases de données|  
|**db_datawriter**|Utilisateurs autorisés à écrire des données dans les bases de données|  
|**db_denydatareader**|Utilisateurs non autorisés à lire les données des bases de données|  
|**db_denydatawriter**|Utilisateurs non autorisés à écrire des données dans les bases de données|  
  
 Le tableau ci-dessous indique les procédures stockées utilisées pour modifier les rôles de base de données.  
  
|Procédure stockée|Action|  
|----------------------|------------|  
|**sp_addrolemember**|Ajoute un utilisateur de base de données à un rôle de base de données fixe|  
|**sp_helprole**|Affiche la liste des membres d'un rôle fixe de base de données.|  
|**sp_droprolemember**|Supprime un membre d'un rôle fixe de base de données|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
 Les informations retournées sont sujettes à des restrictions d'accès aux métadonnées. Les entités sur lesquelles le principal ne possède pas d'autorisation n'apparaissent pas. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, la liste de tous les rôles de base de données fixe est affichée.  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
