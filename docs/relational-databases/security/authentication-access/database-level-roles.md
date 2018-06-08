---
title: Rôles au niveau de la base de données | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
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
- sql13.swb.roleproperties.database.f1
- sql13.swb.roleproperties.object.f1
- SQL13.SWB.DBROLEPROPERTIES.GENERAL.F1
- sql13.swb.roleproperties.general.f1
helpviewer_keywords:
- db_denydatareader role
- users [SQL Server], database roles
- database-level roles [SQL Server]
- db_denydatawriter role
- roles [SQL Server], database
- principals [SQL Server], database-level
- db_backupoperator role
- credentials [SQL Server], roles
- db_accessadmin role
- schemas [SQL Server], roles
- permissions [SQL Server], roles
- database roles [SQL Server], listed
- db_datareader role
- db_ddladmin role
- db_datawriter role
- db_securityadmin role
- db_owner role
- database roles [SQL Server]
- fixed database roles [SQL Server]
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: 7f3fa5f6-6b50-43bb-9047-1544ade55e39
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b1a99af7b5758f77883da3f2a755aaa4bdfdd1a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-level-roles"></a>Rôles au niveau de la base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Pour gérer facilement les autorisations dans vos bases de données, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournit plusieurs *rôles* , qui sont des principaux de sécurité regroupant d'autres principaux. Ils sont similaires aux ***groupes*** du système d'exploitation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Les autorisations des rôles au niveau de la base de données ont une portée à l'échelle de la base de données.  

Pour ajouter et supprimer des utilisateurs à un rôle de base de données, utilisez les options `ADD MEMBER` et `DROP MEMBER` les options de l’instruction [ALTER ROLE](../../../t-sql/statements/alter-role-transact-sql.md) . [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] ne prend pas en charge cette utilisation de `ALTER ROLE`. Utilisez les anciennes procédures [sp_addrolemember](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) et [sp_droprolemember](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md) à la place.
  
 Il existe deux types de rôles au niveau de la base de données : les *rôles de base de données fixes* qui sont prédéfinis dans la base de données et les *rôles de base de données définis par l’utilisateur* que vous pouvez créer.  
  
 Les rôles de base de données fixes sont définis au niveau de la base de données et existent dans chaque base de données. Les membres du rôle de base de données **db_owner** peuvent gérer l’appartenance aux rôles de base de données fixes. La base de données msdb comprend également des rôles de base de données spéciaux.  
  
 Vous pouvez ajouter tout compte de base de données, ainsi que d’autres rôles [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans des rôles au niveau de la base de données. Chaque membre d’un rôle de base de données fixe peut ajouter des utilisateurs à ce rôle.  
  
> [!TIP]  
>  N’ajoutez pas de rôles de base de données définis par l’utilisateur en tant que membres de rôles fixes. Cela pourrait activer une élévation de privilèges involontaire.  

Les autorisations des rôles de base de données définis par l’utilisateur peuvent être personnalisées à l’aide des instructions GRANT, DENY et REVOKE. Pour plus d’informations, consultez [Autorisations (moteur de base de données)](../../../relational-databases/security/permissions-database-engine.md).

Pour obtenir une liste de toutes les autorisations, consultez l’affiche [Autorisations des moteurs de base de données](https://aka.ms/sql-permissions-poster) . (Les autorisations de niveau serveur ne peuvent pas être accordées aux rôles de base de données. Les connexions et les autres principaux au niveau du serveur (tels que les rôles de serveur) ne peuvent pas être ajoutés aux rôles de base de données. Pour la sécurité au niveau du serveur dans [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)], utilisez les [rôles de serveur](../../../relational-databases/security/authentication-access/server-level-roles.md) à la place. Les autorisations de niveau serveur ne peuvent pas être accordées par le biais des rôles dans [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../../includes/sssdw-md.md)].)

## <a name="fixed-database-roles"></a>rôles de base de données fixes
  
 Le tableau suivant répertorie les rôles de base de données fixes et leurs fonctionnalités. Ces rôles existent dans toutes les bases de données. À l’exception du rôle de base de données **public**, les autorisations affectées aux rôles de base de données fixes ne peuvent pas être changées.   
  
|Nom du rôle de base de données fixe|Description|  
|-------------------------------|-----------------|  
|**db_owner**|Les membres du rôle de base de données fixe **db_owner** peuvent effectuer toutes les activités de configuration et de maintenance sur la base de données et peuvent également supprimer la base de données dans [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]. (Dans [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../../includes/sssdw-md.md)], certaines activités de maintenance requièrent des autorisations de niveau serveur et ne peuvent pas être effectuées par **db_owners**.)|  
|**db_securityadmin**|Les membres du rôle de base de données fixe **db_securityadmin** peuvent modifier l'appartenance au rôle et gérer les autorisations. L'ajout de principaux à ce rôle pourrait activer une élévation de privilèges involontaire.|  
|**db_accessadmin**|Les membres du rôle de base de données fixe **db_accessadmin** peuvent ajouter ou supprimer l'accès à la base de données des connexions Windows, des groupes Windows et des connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|**db_backupoperator**|Les membres du rôle de base de données fixe **db_backupoperator** peuvent sauvegarder la base de données.|  
|**db_ddladmin**|Les membres du rôle de base de données fixe **db_ddladmin** peuvent exécuter n'importe quelle commande DDL (Data Definition Language) dans une base de données.|  
|**db_datawriter**|Les membres du rôle de base de données fixe **db_datawriter** peuvent ajouter, supprimer et modifier des données dans toutes les tables utilisateur.|  
|**db_datareader**|Les membres du rôle de base de données fixe **db_datareader** peuvent lire toutes les données de toutes les tables utilisateur.|  
|**db_denydatawriter**|Les membres du rôle de base de données fixe **db_denydatawriter** ne peuvent ajouter, modifier ou supprimer aucune donnée des tables utilisateur d'une base de données.|  
|**db_denydatareader**|Les membres du rôle de base de données fixe **db_denydatareader** ne peuvent lire aucune donnée des tables utilisateur d'une base de données.|  

Les autorisations accordées aux rôles de base de données fixe ne peuvent pas être modifiées. La figure suivante montre les autorisations accordées aux rôles de base de données fixe :

![fixed_database_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-database-roles.png)

## <a name="special-roles-for-includesssdsmdincludessssds-mdmd-and-includesssdwmdincludessssdw-mdmd"></a>Rôles spéciaux pour [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../../includes/sssdw-md.md)]

Ces rôles de base de données existent uniquement dans la base de données MASTER virtuelle. Leurs autorisations sont limitées aux actions effectuées dans MASTER. Seuls les utilisateurs de base de données de MASTER peuvent être ajoutés à ces rôles. Aucune connexion ne peut être ajoutée à ces rôles, toutefois les utilisateurs peuvent être créés en fonction des connexions et ensuite ces utilisateurs peuvent être ajoutés aux rôles. Les utilisateurs de base de données contenus dans MASTER peuvent également être ajoutés à ces rôles.

|Nom du rôle|Description|  
|--------------------|-----------------|
**dbmanager** | Permet de créer et de supprimer des bases de données. Un membre du rôle dbmanager qui crée une base de données devient le propriétaire de cette base de données qui permet à cet utilisateur de se connecter à cette base de données en tant qu’utilisateur dbo. L’utilisateur dbo possède toutes les autorisations de base de données dans la base de données. Les membres du rôle dbmanager n’ont pas nécessairement l’autorisation d’accéder aux bases de données qui ne leur appartiennent pas.
**loginmanager** | Peut créer et supprimer des connexions dans la base de données MASTER virtuelle.  

> [!NOTE]
> Le principal de niveau du serveur et l’administrateur Azure Active Directory (s’il est configuré) détiennent toutes les autorisations dans [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../../includes/sssdw-md.md)] sans avoir besoin d’être membres de tous les rôles. Pour plus d’informations, voir [Authentification et autorisation de base de données SQL : octroi de l’accès](https://azure.microsoft.com/documentation/articles/sql-database-manage-logins/). 
  
## <a name="msdb-roles"></a>Rôles de msdb  
 La base de données msdb contient les rôles à usages spéciaux présentés dans le tableau ci-dessous.  
  
|Nom du rôle msdb|Description|  
|--------------------|-----------------|  
|**db_ssisadmin**<br /><br /> **db_ssisoperator**<br /><br /> **db_ssisltduser**|Les membres de ces rôles de base de données peuvent administrer et utiliser [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mises à niveau à partir d’une version antérieure peuvent contenir une version plus ancienne du rôle, nommée à l’aide de DTS (Data Transformation Services) au lieu de [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Pour plus d’informations, consultez [Rôles Integration Services &#40;Service SSIS&#41;](../../../integration-services/security/integration-services-roles-ssis-service.md).|  
|**dc_admin**<br /><br /> **dc_operator**<br /><br /> **dc_proxy**|Les membres de ces rôles de base de données peuvent administrer et utiliser le collecteur de données. Pour plus d'informations, consultez [Data Collection](../../../relational-databases/data-collection/data-collection.md).|  
|**PolicyAdministratorRole**|Les membres du rôle de base de données **db_ PolicyAdministratorRole** peuvent effectuer toutes les activités de configuration et de maintenance sur les stratégies et conditions de la gestion basée sur une stratégie. Pour plus d’informations, consultez [Administrer des serveurs à l’aide de la Gestion basée sur des stratégies](../../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).|  
|**ServerGroupAdministratorRole**<br /><br /> **ServerGroupReaderRole**|Les membres de ces rôles de base de données peuvent administrer et utiliser des groupes de serveurs inscrits.|  
|**dbm_monitor**|Créé dans la base de données **msdb** lorsque la première base de données est inscrite dans le moniteur de mise en miroir de bases de données. Le rôle **dbm_monitor** ne comporte aucun membre tant qu’un administrateur système n’affecte pas d’utilisateurs au rôle.|  
  
> [!IMPORTANT]  
>  Les membres du rôle **db_ssisadmin** et du rôle **dc_admin** peuvent être en mesure d’élever leurs privilèges à sysadmin. Cette élévation de privilège peut se produire, car ces rôles peuvent modifier les packages [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] et les packages [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] peuvent être exécutés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide du contexte de sécurité sysadmin de l’Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour empêcher cette élévation de privilège lors de l’exécution de plans de maintenance, de jeux d’éléments de collecte de données et d’autres packages [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , configurez les travaux de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent qui exécutent des packages de façon à utiliser un compte proxy doté de privilèges limités ou ajoutez uniquement des membres **sysadmin** aux rôles **db_ssisadmin** et **dc_admin** .  

## <a name="working-with-r-services"></a>Utilisation de R Services  

**S’applique à :** SQL Server à partir de [!INCLUDE[ssSQLv14_md](../../../includes/sssqlv14-md.md)]   

Quand vous installez R Services, vous disposez de rôles de base de données supplémentaires pour la gestion des packages. Pour plus d’informations, consultez [Gestion des packages R pour SQL Server](../../../advanced-analytics/r-services/r-package-management-for-sql-server-r-services.md).

|Nom du rôle |Description|  
|-------------|-----------------|
|**rpkgs-users** |Permet aux utilisateurs d’employer tous les packages partagés qui ont été installés par des membres du rôle rpkgs-shared.|
|**rpkgs-private** |Fournit l’accès aux packages partagés avec les mêmes autorisations que le rôle rpkgs-users. Les membres de ce rôle peuvent également installer, supprimer et utiliser des packages à étendue privée.|
|**rpkgs-shared** |Fournit les mêmes autorisations que le rôle rpkgs-private. Les utilisateurs membres de ce rôle peuvent également installer ou supprimer des packages partagés.|
  
## <a name="working-with-database-level-roles"></a>Utilisation des rôles au niveau de la base de données  
 Le tableau ci-dessous explique les commandes, vues et fonctions permettant d'utiliser les rôles au niveau de la base de données.  
  
|Fonctionnalité|Type|Description|  
|-------------|----------|-----------------|  
|[sp_helpdbfixedrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)|Métadonnées|Retourne une liste des rôles de base de données fixes.|  
|[sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)|Métadonnées|Affiche les autorisations d'un rôle de base de données fixe.|  
|[sp_helprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)|Métadonnées|Renvoie les informations concernant les rôles de la base de données active.|  
|[sp_helprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)|Métadonnées|Retourne des informations sur les membres d'un rôle dans la base de données actuelle.|  
|[sys.database_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Métadonnées|Retourne une ligne pour chaque membre de chaque rôle de base de données.|  
|[IS_MEMBER &#40;Transact-SQL&#41;](../../../t-sql/functions/is-member-transact-sql.md)|Métadonnées|Indique si l'utilisateur actuel est membre du groupe Microsoft Windows ou du rôle de base de données Microsoft SQL Server spécifié.|  
|[CREATE ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-role-transact-sql.md)|Command|Crée un rôle de base de données dans la base de données active.|  
|[ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md)|Command|Modifie le nom ou l’appartenance à un rôle de base de données.|  
|[DROP ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-role-transact-sql.md)|Command|Supprime un rôle de la base de données.|  
|[sp_addrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)|Command|Crée un rôle de base de données dans la base de données active.|  
|[sp_droprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)|Command|Supprime un rôle de base de données de la base de données actuelle.|  
|[sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)|Command|Ajoute un utilisateur ou un rôle de base de données, un compte de connexion ou un groupe Windows à un rôle de base de données dans la base de données active. Toutes les plateformes sauf [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] doivent utiliser `ALTER ROLE` à la place.|  
|[sp_droprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)|Command|Supprime un compte de sécurité d'un rôle SQL Server dans la base de données actuelle. Toutes les plateformes sauf [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] doivent utiliser `ALTER ROLE` à la place.|
|[GRANT](../../../t-sql/statements/grant-transact-sql.md)| Autorisations | Ajoute l’autorisation à un rôle.
|[DENY](../../../t-sql/statements/deny-transact-sql.md)| Autorisations | Refuse une autorisation à un rôle.
|[REVOKE](../../../t-sql/statements/revoke-transact-sql.md)| Autorisations | Supprime des autorisations accordées ou refusées antérieurement.
  
  
## <a name="public-database-role"></a>Rôle de base de données public  
 Chaque utilisateur de base de données appartient au rôle de base de données **public** . Lorsqu'un utilisateur ne s'est pas vu accorder ou refuser des autorisations spécifiques sur un objet sécurisable, il hérite des autorisations accordées à **public** sur cet objet. Les utilisateurs de base de données ne peuvent pas être supprimés du rôle **public** . 
  
## <a name="related-content"></a>Contenu associé  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)  
  
 [Sécurisation de SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [sp_helprotect &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)  
  
  
