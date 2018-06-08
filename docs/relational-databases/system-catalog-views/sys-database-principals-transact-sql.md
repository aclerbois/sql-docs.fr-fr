---
title: Sys.database_principals (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
caps.latest.revision: 46
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a879dfcc6dd0feb57126574947b51f84261af915
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabaseprincipals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque principal de sécurité d'une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom du principal, unique dans la base de données.|  
|**principal_id**|**int**|ID du principal, unique dans la base de données.|  
|**type**|**char(1)**|Type de principal :<br /><br /> A = rôle d'application<br /><br /> C = utilisateur mappé à un certificat<br /><br /> E = utilisateur externe à partir d’Azure Active Directory<br /><br /> G = groupe Windows<br /><br /> K = utilisateur mappé à une clé asymétrique<br /><br /> R = rôle de base de données<br /><br /> S = utilisateur SQL<br /><br /> U = utilisateur Windows<br /><br /> X = groupe externe à partir d’applications ou de groupe Active Directory de Azure|  
|**type_desc**|**nvarchar(60)**|Description du type de principal.<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|Nom à utiliser lorsque le nom SQL ne spécifie pas un schéma. Null pour les principaux qui ne sont pas de type S, U ou A.|  
|**create_date**|**datetime**|Heure de création du principal.|  
|**modify_date**|**datetime**|Heure de la dernière modification du principal.|  
|**owning_principal_id**|**int**|Identificateur du principal qui possède ce principal. Toutes les entités à l’exception des rôles de base de données doivent être détenues par **dbo**.|  
|**sid**|**varbinary(85)**|SID (identificateur de sécurité) du principal.  NULL pour SYS et INFORMATION SCHEMAS.|  
|**is_fixed_role**|**bit**|Avec la valeur 1, cette ligne représente une entrée pour un des rôles de base de données fixes : db_owner, db_accessadmin, db_datareader, db_datawriter, db_ddladmin, db_securityadmin, db_backupoperator, db_denydatareader, db_denydatawriter.|  
|**authentication_type**|**int**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Signifie le type d'authentification. Voici les valeurs possibles et leurs descriptions.<br /><br /> 0 : aucune authentification<br />1 : authentification de l’instance<br />2 : l’authentification de base de données<br />3 : l’authentification Windows|  
|**authentication_type_desc**|**nvarchar(60)**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Description du type d'authentification. Voici les valeurs possibles et leurs descriptions.<br /><br /> NONE : Aucune authentification<br />INSTANCE : Authentification de l’Instance<br />Base de données : Authentification de base de données<br />WINDOWS : L’authentification Windows|  
|**default_language_name**|**sysname**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Signifie la langue par défaut de ce principal.|  
|**default_language_lcid**|**int**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Signifie le LCID par défaut de ce principal.|  
|**allow_encrypted_value_modifications**|**bit**|**S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur pour les données de copie en bloc chiffrées à l’aide Always Encrypted, entre des tables ou des bases de données, sans déchiffrage des données. La valeur par défaut est OFF. |      
  
## <a name="remarks"></a>Notes  
 Le *PasswordLastSetTime* propriétés sont disponibles sur toutes les configurations prises en charge de SQL Server, mais les autres propriétés sont disponibles uniquement lorsque SQL Server est en cours d’exécution sur Windows Server 2003 ou version ultérieure et que CHECK_POLICY et CHECK_EXPIRATION sont activés. Consultez [stratégie de mot de passe](../../relational-databases/security/password-policy.md) pour plus d’informations.  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut consulter son nom d'utilisateur, les utilisateurs système et les rôles de base de données fixes. Pour voir les autres utilisateurs, vous devez disposer de l'autorisation ALTER ANY USER, ou d'une autorisation sur l'utilisateur. Pour afficher les rôles définis par l'utilisateur, vous devez disposer de l'autorisation ALTER ANY ROLE, ou appartenir au rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A : Énumération de toutes les autorisations des principaux de base de données  
 La requête suivante énumère les autorisations accordées ou refusées explicitement aux principaux de base de données.  
  
> [!IMPORTANT]  
>  Les autorisations de rôles de base de données fixes n'apparaissent pas dans sys.database_permissions. Par conséquent, les principaux de base de données peuvent avoir des autorisations supplémentaires non répertoriées ici.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>B. Énumération des autorisations sur les objets de schéma dans une base de données  
 La requête suivante joint sys.database_principals et sys.database_permissions à sys.objects et sys.schemas pour répertorier les autorisations accordées ou refusées sur des objets de schéma spécifiques.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C: répertoriant toutes les autorisations des principaux de base de données  
 La requête suivante énumère les autorisations accordées ou refusées explicitement aux principaux de base de données.  
  
> [!IMPORTANT]  
>  Les autorisations des rôles de base de données fixes n’apparaissent pas dans `sys.database_permissions`. Par conséquent, les principaux de base de données peuvent avoir des autorisations supplémentaires non répertoriées ici.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D : affichage des autorisations sur les objets de schéma dans une base de données  
 La requête suivante jointures `sys.database_principals` et `sys.database_permissions` à `sys.objects` et `sys.schemas` pour répertorier les autorisations accordées ou refusées à des objets de schéma spécifiques.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Utilisateurs de base de données de relation contenant-contenu - rendre votre base de données Portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication)  
  
  


