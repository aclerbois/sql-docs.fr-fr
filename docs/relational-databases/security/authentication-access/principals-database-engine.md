---
title: Principaux (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectroll.f1
- sql13.swb.databaseuser.permissions.user.f1--May use common.permissions
helpviewer_keywords:
- certificates [SQL Server], principals
- roles [SQL Server], principals
- permissions [SQL Server], principals
- '##MS_SQLAuthenticatorCertificate##'
- principals [SQL Server]
- '##MS_SQLResourceSigningCertificate##'
- groups [SQL Server], principals
- '##MS_AgentSigningCertificate##'
- authentication [SQL Server], principals
- schemas [SQL Server], principals
- principals [SQL Server], about principals
- security [SQL Server], principals
- users [SQL Server], principals
- '##MS_SQLReplicationSigningCertificate##'
ms.assetid: 3f7adbf7-6e40-4396-a8ca-71cbb843b5c2
caps.latest.revision: 57
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d70f44041140b8205ead612789bf463ae94d9122
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="principals-database-engine"></a>Principaux (moteur de base de données)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Les*principaux* sont des entités qui peuvent demander des ressources [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Comme les autres composants du modèle d'autorisation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , les principaux peuvent être ordonnés de façon hiérarchique. Le champ d'influence d'un principal dépend de l'étendue de sa définition : Windows, serveur, base de données, et du fait qu'il est indivisible ou qu'il s'agit d'une collection. Une connexion Windows est un exemple de principal indivisible et un groupe Windows est un exemple de principal constituant une collection. Chaque principal a un identificateur de sécurité (SID). Cette rubrique s’applique à toutes les versions de SQL Server, mais il existe certaines restrictions sur les principaux au niveau du serveur dans SQL Database ou SQL Data Warehouse. 
  
## <a name="sql-server-level-principals"></a>Principaux au niveau de SQL Server  
  
-  Connexion d’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]   
-  Connexion d’authentification Windows pour un utilisateur Windows  
-  Connexion d’authentification Windows pour un groupe Windows   
-  Connexion d’authentification Azure Active Directory pour un utilisateur AD
-  Connexion d’authentification Azure Active Directory pour un groupe AD
-  Rôle serveur  
  
 ## <a name="database-level-principals"></a>Principaux au niveau des bases de données  
  
-   Utilisateur de base de données (il existe 11 types d’utilisateurs. Pour plus d’informations, consultez [CREATE USER ](../../../t-sql/statements/create-user-transact-sql.md).) 
-   Rôle de base de données  
-   Rôle d'application  
  
## <a name="sa-login"></a>Connexion sa  
 La connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] `sa` est un principal au niveau du serveur. Par défaut, elle est créée lorsqu'une instance est installée. À compter de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], la base de données par défaut de sa est MASTER. Il s'agit là d'une différence par rapport aux versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La connexion `sa` est un membre du rôle de base de données fixe `sysadmin`. La connexion `sa` a toutes les autorisations sur le serveur et ne peut pas être limitée. La connexion `sa` ne peut pas être supprimée, mais elle peut être désactivée afin que personne ne puisse l’utiliser.

## <a name="dbo-user-and-dbo-schema"></a>Utilisateur dbo et schéma dbo

L’utilisateur `dbo` est un principal d’utilisateur spécial dans chaque base de données. Tous les administrateurs de SQL Server, les membres du rôle de serveur fixe `sysadmin`, la connexion `sa` et les propriétaires de la base de données accèdent aux bases de données en tant qu’utilisateur `dbo`. L’utilisateur `dbo` a toutes les autorisations dans la base de données et ne peut pas être limité ou supprimé. `dbo` signifie « database owner » (propriétaire de base de données), mais le compte d’utilisateur `dbo` n’est pas identique au rôle de base de données fixe `db_owner`, et le rôle de base de données fixe `db_owner` n’est pas identique au compte d’utilisateur qui est inscrit en tant que propriétaire de la base de données.     
L’utilisateur `dbo` est propriétaire du schéma `dbo`. Le schéma `dbo` est le schéma par défaut pour tous les utilisateurs, sauf si un autre schéma est spécifié.  Le schéma `dbo` ne peut pas être supprimé.
  
## <a name="public-server-role-and-database-role"></a>Rôle serveur public et rôle de base de données  
Chaque connexion appartient au rôle serveur fixe `public` et chaque utilisateur de base de données appartient au rôle de base de données `public`. Quand des autorisations spécifiques sur un élément sécurisable n’ont pas été accordées ni refusées à une connexion ou à un utilisateur, celui-ci ou celle-ci hérite des autorisations accordées à public sur cet élément sécurisable. Le rôle serveur fixe `public` et le rôle de base de données fixe `public` ne peuvent pas être supprimés. Toutefois, vous pouvez révoquer des autorisations des rôles `public`. De nombreuses autorisations sont affectées par défaut aux rôles `public`. La plupart de ces autorisations sont nécessaires pour les opérations de routine dans la base de données (le genre de choses que tout le monde doit pouvoir faire). Soyez prudent lors de la révocation des autorisations de l’utilisateur ou de la connexion public, car cela affecte tous les utilisateurs/connexions. En général, vous ne devez pas refuser d’autorisations à public, car l’instruction deny remplace toutes les instructions grant que vous pouvez exécuter pour des utilisateurs spécifiques. 
  
## <a name="informationschema-and-sys-users-and-schemas"></a>INFORMATION_SCHEMA et schémas et utilisateurs sys 
 Chaque base de données inclut deux entités qui apparaissent comme des utilisateurs dans des affichages catalogue : `INFORMATION_SCHEMA` et `sys`. Ces entités sont nécessaires pour une utilisation interne par le moteur de base de données. Elles ne peuvent être ni modifiées ni supprimées.  
  
## <a name="certificate-based-sql-server-logins"></a>Connexions SQL Server basées sur des certificats  
 Les principaux de serveur compris entre deux signes dièse (##) sont destinés uniquement à une utilisation système interne. Les principaux suivants sont créés à partir de certificats lors de l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ; ils ne doivent pas être supprimés.  
  
-   \##MS_SQLResourceSigningCertificate##    
-   \##MS_SQLReplicationSigningCertificate##    
-   \##MS_SQLAuthenticatorCertificate##    
-   \##MS_AgentSigningCertificate##   
-   \##MS_PolicyEventProcessingLogin##   
-   \##MS_PolicySigningCertificate##   
-   \##MS_PolicyTsqlExecutionLogin##   
 
 Ces comptes de principaux n’ont pas de mots de passe modifiables par les administrateurs, car ils utilisent des certificats envoyés à Microsoft.
  
## <a name="the-guest-user"></a>Utilisateur invité  
 Chaque base de données inclut un `guest`. Les autorisations accordées à l'utilisateur `guest` sont héritées par les utilisateurs qui ont accès à la base de données, mais n'ont pas de compte d'utilisateur dans la base de données. L’utilisateur `guest` ne peut pas être supprimé, mais vous pouvez le désactiver en révoquant son autorisation CONNECT. Vous pouvez révoquer l’autorisation CONNECT en exécutant l’instruction `REVOKE CONNECT FROM GUEST;` dans n’importe quelle base de données autre que `master` ou `tempdb`.  
  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la conception d’un système d’autorisations, consultez [Getting Started with Database Engine Permissions](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
 Les rubriques suivantes sont incluses dans cette section de la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   [Rubriques de procédures relatives à la gestion des connexions, des utilisateurs et des schémas](../../../relational-databases/security/authentication-access/managing-logins-users-and-schemas-how-to-topics.md)  
  
-   [Rôles de niveau serveur](../../../relational-databases/security/authentication-access/server-level-roles.md)  
  
-   [Rôles au niveau de la base de données](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
-   [Rôles d'applications](../../../relational-databases/security/authentication-access/application-roles.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurisation de SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.sql_logins &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [Rôles de niveau serveur](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Rôles au niveau de la base de données](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
  
