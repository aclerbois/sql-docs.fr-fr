---
title: Autorisations GRANT T-SQL - Parallel Data Warehouse | Documents Microsoft
description: Autorisations GRANT T-SQL pour les opérations de base de données dans Parallel Data Warehouse.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 01ef7b199a07be8bbc2dc1dee40d9c4d5771db1b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="grant-t-sql-permissions-for-parallel-data-warehouse"></a>Autorisations GRANT T-SQL pour Parallel Data Warehouse
Autorisations GRANT T-SQL pour les opérations de base de données dans Parallel Data Warehouse.

## <a name="grant-permissions-to-submit-database-queries"></a>Accorder des autorisations pour soumettre des requêtes de base de données
Cette section explique comment accorder des autorisations aux rôles de base de données et aux utilisateurs d’interroger des données sur l’appliance SQL Server PDW.  
  
Les instructions utilisées pour accorder des autorisations pour interroger des données dépendant de l’étendue d’accès souhaité. Les instructions SQL suivantes créent une connexion nommée KimAbercrombie qui peut accéder à l’appareil, créez un utilisateur de base de données nommé KimAbercrombie dans le **AdventureWorksPDW2012** de base de données, créer un rôle de base de données nommé PDWQueryData, ajoute l’utilisation KimAbercrombie au PDWQueryData rôle, puis afficher les options pour accorder l’accès de la requête, selon si l’accès est accordé à l’objet, ou niveau base de données.  
  
```sql  
USE master;  
GO  
  
CREATE LOGIN KimAbercrombie WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,  
CHECK_EXPIRATION = ON,  
CHECK_POLICY = ON;  
GO  
  
USE AdventureWorksPDW2012;  
GO  
  
CREATE USER KimAbercrombie;  
GO  
  
CREATE ROLE PDWQueryData;  
GO  
  
EXEC sp_addrolemember 'PDWQueryData', 'KimAbercrombie'  
GO  
  
-- If the permission is granted against a table or view only, use this syntax:  
GRANT SELECT ON OBJECT::AdventureWorksPDW2012..DimEmployee TO PDWQueryData;  
GO  
  
-- If the permission is granted for the database, use this syntax:  
GRANT SELECT ON DATABASE::AdventureWorksPDW2012 TO PDWQueryData;  
GO  
  
-- If KimAbercrombie is the only user that needs to access a table, use this syntax:  
GRANT SELECT ON OBJECT::AdventureWorksPDW2012..DimEmployee TO KimAbercrombie;  
GO  
```  
  
## <a name="grant-permissions-to-use-the-admin-console"></a>Accorder des autorisations pour utiliser la Console d’administration
Cette section explique comment accorder des autorisations à des connexions pour utiliser la Console d’administration.  
  
**Utilisez la Console d’administration**  
  
Pour utiliser la Console d’administration une connexion requiert le niveau de serveur **VIEW SERVER STATE** autorisation. L’instruction SQL suivante accorde le **VIEW SERVER STATE** autorisation à la connexion `KimAbercrombie` afin que Kim peut utiliser la Console d’administration pour analyser l’appliance SQL Server PDW.  
  
```sql  
USE master;  
GO  
GRANT VIEW SERVER STATE TO KimAbercrombie;  
GO  
```  
  
**Arrêt des Sessions**  
  
Pour autoriser une connexion à supprimer des sessions, accorder la **ALTER ANY CONNECTION** autorisation comme suit :  
  
```sql  
GRANT ALTER ANY CONNECTION TO KimAbercrombie;  
```  
  
## <a name="grant-permissions-to-load-data"></a>Accorder des autorisations pour charger des données
Cette section décrit comment accorder des autorisations pour les rôles de base de données et les utilisateurs de base de données pour charger des données sur le PDWappliance du serveur SQL.  
  
Le script ci-dessous les autorisations sont requises pour chaque option de chargement. Vous pouvez modifier cette option pour répondre à vos besoins spécifiques.  
  
```sql  
-- Create server login for the examples that follow.  
USE master;  
CREATE LOGIN BI_ETLUser WITH PASSWORD = '******';  
  
--Grant BULK Load permissions   
GRANT ADMINISTER BULK OPERATIONS TO BI_ETLUser;  
  
--Grant Staging database permissions  
USE stagedb;  
CREATE USER BI_ETLUser for login BI_ETLUser;  
EXEC sp_addrolemember 'db_datareader','BI_ETLUser';  
EXEC sp_addrolemember 'db_datawriter','BI_ETLUser';  
EXEC sp_addrolemember 'db_ddladmin','BI_ETLUser';  
  
-- The CREATE TABLE permission is required for the database hosting the temporary table.  
-- This may be the staging database (if specified) or destination database.   
-- The CREATE permission is not required if loading in fastappend mode.  
GRANT CREATE TABLE ON database::stagedb TO BI_ETLUser;  
  
-- If loading in append or fastappend mode, the INSERT permission is required on the destination table.  
GRANT INSERT ON database::stagedb TO BI_ETLUser;  
  
-- If loading in reload mode, the INSERT and DELETE permissions are required on the destination table.  
GRANT INSERT ON database::stagedb TO BI_ETLUser;  
GRANT DELETE ON database::stagedb TO BI_ETLUser;  
  
-- If loading in upsert mode, the INSERT and UPDATE permissions are required on the destination table.  
GRANT INSERT ON database::stagedb TO BI_ETLUser;  
GRANT UPDATE ON database::stagedb TO BI_ETLUser;  
  
-- Destination DB  
USE tpch_1gb;  
CREATE USER BI_ETLUser FOR LOGIN BI_ETLUser;  
EXEC sp_addrolemember 'db_datareader','BI_ETLUser';  
EXEC sp_addrolemember 'db_datawriter','BI_ETLUser';  
```  
  
## <a name="grant-permissions-to-copy-data-off-the-appliance"></a>Accorder des autorisations nécessaires pour copier des données à partir de l’application
Cette section explique comment accorder des autorisations à un rôle d’utilisateur ou de la base de données pour copier des données à partir de l’appliance SQL Server PDW.  
  
Pour déplacer des données vers un autre emplacement nécessite **sélectionnez** autorisation sur la table contenant les données à déplacer.  
  
Si la destination pour les données est un autre SQL Server PDW, l’utilisateur doit avoir **CREATE TABLE** autorisation à la destination et **ALTER SCHEMA** autorisation sur le schéma qui contient la table.  
  
## <a name="grant-permissions-to-manage-databases"></a>Accorder des autorisations pour gérer les bases de données
Cette section décrit comment accorder des autorisations à un utilisateur de base de données pour gérer une base de données sur le dispositif de SQL Server PDW.  
  
Dans certains cas, une société affecte un gestionnaire pour une base de données. Le Gestionnaire de contrôle d’accès des autres connexions à la base de données, ainsi que les données et les objets de la base de données. Pour gérer tous les objets, de rôles, et accorder aux utilisateurs dans une base de données à l’utilisateur le **contrôle** autorisation sur la base de données. L’instruction suivante accorde le **contrôle** l’autorisation sur le **AdventureWorksPDW2012** base de données à l’utilisateur `KimAbercrombie`.  
  
```sql
USE AdventureWorksPDW2012;  
GO  
GRANT CONTROL ON DATABASE:: AdventureWorksPDW2012 TO KimAbercrombie;  
```  
  
Pour autoriser une personne à contrôler toutes les bases de données sur l’appareil, vous devez accorder le **ALTER ANY DATABASE** autorisation dans la base de données master.  
  
## <a name="grant-permissions-to-manage-logins-users-and-database-roles"></a>Accorder des autorisations pour gérer les connexions, utilisateurs et rôles de base de données
Cette section explique comment accorder des autorisations pour gérer les connexions, les utilisateurs de base de données et les rôles de base de données.  
  
### <a name="PermsAdminConsole"></a>Accorder des autorisations pour gérer des connexions  
**Ajouter ou gérer des connexions**  
  
Les instructions SQL suivantes créent une connexion nommée KimAbercrombie qui peut créer des connexions à l’aide de la [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md) instruction et modifier les connexions existantes à l’aide de la [ALTER LOGIN](../t-sql/statements/alter-login-transact-sql.md) instruction.  
  
Le **ALTER ANY LOGIN** autorisation accorde la possibilité de créer des connexions et drop existant. Une fois qu’une connexion existe, la connexion peut être gérée par les connexions avec le **ALTER ANY LOGIN** autorisation ou le **ALTER** autorisation sur cette connexion. Un utilisateur peut modifier la base de données par défaut et le mot de passe pour sa propre connexion.  
  
```sql 
CREATE LOGIN KimAbercrombie   
WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,  
CHECK_EXPIRATION = ON,  
CHECK_POLICY = ON;  
GO  
  
GRANT ALTER ANY LOGIN TO KimAbercrombie;  
```  
  
### <a name="grant-permissions-to-manage-login-sessions"></a>Accorder des autorisations pour gérer les Sessions de connexion  
Pour que la possibilité d’afficher toutes les sessions sur le serveur nécessite le **VIEW SERVER STATE** autorisation. Pour pouvoir terminer les sessions d’autres connexions, la **ALTER ANY CONNECTION** autorisation. L’exemple suivant utilise la `KimAbercrombie` connexion créée précédemment.  
  
```sql  
-- Grant permissions to view sessions and queries  
GRANT VIEW SERVER STATE TO KimAbercrombie;  
  
-- Grant permission to end sessions  
GRANT ALTER ANY CONNECTION TO KimAbercrombie;  
```  
  
### <a name="grant-permission-to-manage-database-users"></a>Accorder l’autorisation de gérer les utilisateurs de base de données  
Créer et supprimer des utilisateurs de base de données requièrent le **ALTER ANY USER** autorisation. La gestion des utilisateurs existants nécessite la **ALTER ANY USER** autorisation ou le **ALTER** autorisation sur cet utilisateur. L’exemple suivant utilise la `KimAbercrombie` connexion créée précédemment.  
  
```sql  
-- Create a user  
USE AdventureWorksPDW2012;  
GO  
CREATE USER KimAbercrombie;  
  
-- Grant permissions to create and drop users   
GRANT ALTER ANY USER TO KimAbercrombie;  
```  
  
### <a name="grant-permisson-to-manage-database-roles"></a>Accorder l’autorisation pour gérer les rôles de base de données  
Créer et supprimer des rôles de base de données défini par l’utilisateur requiert la **ALTER ANY ROLE** autorisation. L’exemple suivant utilise la `KimAbercrombie` connexion et utilisez créé précédemment.  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
-- Grant permissions to create and drop roles  
GRANT ALTER ANY ROLE TO KimAbercrombie;  
```  
  
### <a name="login-user-and-role-permission-charts"></a>Connexion, utilisateur et les graphiques d’autorisation de rôle  
Les graphiques suivants peuvent prêter à confus, mais elles affichent des autorisations de levier comment supérieur (par exemple, CONTROL) incluent des autorisations plus précises qui peuvent être accordées séparément (par exemple, ALTER). Il est recommandé de toujours accorder des autorisations pour une personne effectuer leurs tâches nécessaires minimum. Pour ce faire, accordez des autorisations plus spécifiques, plutôt que les autorisations de niveau supérieur.  
  
**Autorisations de connexion :**  
  
![Autorisations de connexion de sécurité APS](./media/grant-permissions/APS_security_login_perms.png "APS_security_login_perms")  
  
**Autorisations d’utilisateur :**  
  
![Autorisations utilisateur de sécurité APS](./media/grant-permissions/APS_security_user_perms.png "APS_security_user_perms")  
  
**Autorisations de rôle :**  
  
![Autorisations de rôle de sécurité APS](./media/grant-permissions/APS_security_role_perms.png "APS_security_role_perms")  
  
<!-- MISSING LINKS
For a list of all permissions, see [Permissions: GRANT, DENY, REVOKE &#40;SQL Server PDW&#41;](../sqlpdw/permissions-grant-deny-revoke-sql-server-pdw.md).  
  
-->

## <a name="grant-permissions-to-monitor-the-appliance"></a>Accorder des autorisations pour contrôler le matériel
SQL Server PDW peut être surveillée à l’aide des vues système SQL Server PDW ou Console d’administration. Connexions requièrent le niveau de serveur **VIEW SERVER STATE** autorisation d’analyse de l’application. Connexions nécessitent la **ALTER ANY CONNECTION** autorisé à mettre fin aux connexions à l’aide de la Console d’administration ou le **KILL** commande. Pour plus d’informations sur les autorisations requises pour utiliser la Console d’administration, consultez [accorder des autorisations pour utiliser la Console d’administration &#40;SQL Server PDW&#41;](#grant-permissions-to-use-the-admin-console).  
  
### <a name="PermsAdminConsole"></a>Accorder l’autorisation d’analyse de l’application à l’aide de vues système  
Les instructions SQL suivantes créent une connexion nommée `monitor_login` et accorde le **VIEW SERVER STATE** autorisé au `monitor_login` connexion.  
  
```sql  
USE master;  
GO  
CREATE LOGIN monitor_login WITH PASSWORD='Password4321';  
GRANT VIEW SERVER STATE TO monitor_login;  
GO  
```  
  
### <a name="grant-permission-to-monitor-the-appliance-by-using-system-views-and-to-terminate-connections"></a>Accorder l’autorisation pour contrôler le matériel à l’aide de vues système et arrêter les connexions  
Les instructions SQL suivantes créent une connexion nommée `monitor_and_terminate_login` et accorde le **VIEW SERVER STATE** et **ALTER ANY CONNECTION** des autorisations pour le `monitor_and_terminate_login` connexion.  
  
```sql  
USE master;  
GO  
CREATE LOGIN monitor_and_terminate_login WITH PASSWORD='Password1234';   
GRANT VIEW SERVER STATE TO monitor_and_terminate_login;   
GRANT ALTER ANY CONNECTION TO monitor_and_terminate_login;  
GO  
```  
  
Pour créer des connexions d’administration, consultez [rôles serveur fixes](pdw-permissions.md#fixed-server-roles).  
  
## <a name="see-also"></a>Voir aussi
[CRÉER UNE CONNEXION](../t-sql/statements/create-login-transact-sql.md)  
[CRÉER UN UTILISATEUR](../t-sql/statements/create-user-transact-sql.md)  
[CRÉER DES RÔLES](../t-sql/statements/create-role-transact-sql.md)  
[Charge](load-overview.md)  
