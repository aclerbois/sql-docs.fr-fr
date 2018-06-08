---
title: Migrer vers une base de données partiellement à relation contenant-contenu | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- contained database, migrating to
ms.assetid: 90faac38-f79e-496d-b589-e8b2fe01c562
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 774aa0280fbf94f723710188d984fe637b9ee513
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-to-a-partially-contained-database"></a>Migrate to a Partially Contained Database
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment se préparer à passer au modèle de base de données partiellement à relation contenant-contenu, puis indique la procédure de migration.  
  
 **Dans cette rubrique :**  
  
-   [Préparation de la migration d'une base de données](#prepare)  
  
-   [Activer les bases de données à relation contenant-contenu](#enable)  
  
-   [Conversion d'une base de données au modèle partiellement à relation contenant-contenu](#convert)  
  
-   [Migration des utilisateurs vers des utilisateurs de base de données à relation contenant-contenu](#users)  
  
##  <a name="prepare"></a> Préparation de la migration d'une base de données  
 Passez en revue les éléments suivants lorsque vous envisagez de migrer une base de données vers le modèle de base de données partiellement à relation contenant-contenu.  
  
-   Vous devez comprendre le modèle de base de données partiellement à relation contenant-contenu. Pour plus d’informations, consultez [Contained Databases](../../relational-databases/databases/contained-databases.md).  
  
-   Vous devez connaître les risques qui sont propres aux bases de données partiellement à relation contenant-contenu. Pour plus d'informations, consultez [Bonnes pratiques de sécurité avec les bases de données à relation contenant-contenu](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
-   Les bases de données à relation contenant-contenu ne prennent pas en charge la réplication, la capture de données modifiées ou le suivi des modifications. Vérifiez que la base de données n'utilise pas ces fonctionnalités.  
  
-   Passez en revue la liste des fonctionnalités de base de données qui sont modifiées pour les bases de données partiellement à relation contenant-contenu. Pour plus d’informations, consultez [Fonctionnalités modifiées &#40;base de données à relation contenant-contenu&#41;](../../relational-databases/databases/modified-features-contained-database.md).  
  
-   Requête [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) permettant de rechercher des objets ou des fonctionnalités sans relation contenant-contenu dans la base de données. Pour plus d'informations, consultez  
  
-   Surveillez le XEvent **database_uncontained_usage** pour voir quand des fonctionnalités sans relation contenant-contenu sont utilisées.  
  
##  <a name="enable"></a> Activer les bases de données à relation contenant-contenu  
 Les bases de données à relation contenant-contenu doivent être activées sur l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], avant que les bases de données à relation contenant-contenu puissent être créées.  
  
### <a name="enabling-contained-databases-using-transact-sql"></a>Activation de bases de données à relation contenant-contenu à l'aide de Transact-SQL  
 L'exemple suivant active des bases de données à relation contenant-contenu sur l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
```sql  
sp_configure 'contained database authentication', 1;  
GO  
RECONFIGURE ;  
GO  
```  
  
#### <a name="enabling-contained-databases-using-management-studio"></a>Activation de bases de données à relation contenant-contenu à l'aide de Management Studio  
 L'exemple suivant active des bases de données à relation contenant-contenu sur l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom du serveur, puis sélectionnez **Propriétés**.  
  
2.  Dans la page **Avancé** , dans la section **Relation contenant-contenu** , affectez à l’option **Activer les bases de données à relation contenant-contenu** la valeur **True**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="convert"></a> Conversion d'une base de données au modèle partiellement à relation contenant-contenu  
 Une base de données est convertie en base de données à relation contenant-contenu en modifiant l’option **CONTAINMENT** .  
  
### <a name="converting-a-database-to-partially-contained-using-transact-sql"></a>Conversion d'une base de données au modèle partiellement à relation contenant-contenu à l'aide de Transact-SQL  
 L'exemple suivant convertit une base de données nommée `Accounting` en base de données partiellement à relation contenant-contenu.  
  
```sql  
USE [master]  
GO  
ALTER DATABASE [Accounting] SET CONTAINMENT = PARTIAL  
GO  
```  
  
### <a name="converting-a-database-to-partially-contained-using-management-studio"></a>Conversion d'une base de données au modèle partiellement à relation contenant-contenu à l'aide de Management Studio  
 L'exemple suivant convertit une base de données en base de données partiellement à relation contenant-contenu.  
  
1.  Dans Explorateur d’objets, développez **Bases de données**, cliquez avec le bouton droit sur la base de données à convertir, puis sélectionnez **Propriétés**.  
  
2.  Dans la page **Options** , modifiez l’option **Type de relation contenant-contenu** en **Partiel**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="users"></a> Migration des utilisateurs vers des utilisateurs de base de données à relation contenant-contenu  
 L'exemple suivant migre tous les utilisateurs basés sur des comptes de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers des utilisateurs de base de données à relation contenant-contenu avec mots de passe. L'exemple exclut les comptes de connexion qui ne sont pas activés. L'exemple doit être exécuté dans la base de données à relation contenant-contenu.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Contained Databases](../../relational-databases/databases/contained-databases.md)   
 [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)   
 [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)  
  
  
