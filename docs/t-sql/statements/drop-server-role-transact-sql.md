---
title: DROP SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP SERVER ROLE
- DROP_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, DROP
- DROP SERVER ROLE statement
ms.assetid: a2a1e6e6-e40c-4d6a-81be-d197b80bf226
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3f0d7ede0f8d714fcf90bf62d57b62acf220a4aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-server-role-transact-sql"></a>DROP SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

  Supprime un rôle de serveur défini par l'utilisateur.  
  
 Les rôles de serveur définis par l'utilisateur constituent une nouveauté dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DROP SERVER ROLE role_name  
```  
  
## <a name="arguments"></a>Arguments  
 *role_name*  
 Spécifie le rôle de serveur défini par l'utilisateur à supprimer du serveur.  
  
## <a name="remarks"></a>Notes   
 Les rôles de serveur définis par l'utilisateur qui possèdent leurs propres éléments sécurisables ne peuvent pas être supprimés du serveur. Pour supprimer un rôle de serveur défini par l'utilisateur qui possède des éléments sécurisables, vous devez tout d'abord transférer la propriété de ces éléments ou supprimer ces derniers.  
  
 Les rôles de serveur définis par l'utilisateur qui comportent des membres ne peuvent pas être supprimés. Pour supprimer un rôle serveur défini par l’utilisateur qui comporte des membres, vous devez d’abord supprimer les membres du rôle à l’aide de l’instruction [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Les rôles serveur fixes ne peuvent pas être supprimés.  
  
 Vous pouvez afficher des informations sur l’appartenance à des rôles en interrogeant la vue de catalogue [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le rôle de serveur ou l'autorisation ALTER ANY SERVER ROLE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-to-drop-a-server-role"></a>A. Pour supprimer un rôle de serveur  
 L'exemple suivant supprime le rôle de serveur `purchasing`.  
  
```  
DROP SERVER ROLE purchasing;  
GO  
```  
  
### <a name="b-to-view-role-membership"></a>B. Pour consulter l'appartenance à un rôle  
 Pour afficher l’appartenance à un rôle, utilisez la page **Rôle serveur (Membres**) dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou exécutez la requête suivante :  
  
```  
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
### <a name="c-to-view-role-membership"></a>C. Pour consulter l'appartenance à un rôle  
 Pour déterminer si un rôle de serveur possède un autre rôle de serveur, exécutez la requête suivante :  
  
```  
SELECT SP1.name AS RoleOwner, SP2.name AS Server_Role  
FROM sys.server_principals AS SP1  
JOIN sys.server_principals AS SP2  
    ON SP1.principal_id = SP2.owning_principal_id   
ORDER BY SP1.name ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
