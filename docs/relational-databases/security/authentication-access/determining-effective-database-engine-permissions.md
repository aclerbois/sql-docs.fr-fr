---
title: Détermination des autorisations effectives du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- permissions, effective
- effective permissions
ms.assetid: 273ea09d-60ee-47f5-8828-8bdc7a3c3529
caps.latest.revision: 5
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0a2e2c205558f81b2141739bdc9b53bbb85d55f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="determining-effective-database-engine-permissions"></a>Détermination des autorisations effectives du moteur de base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Cet article explique comment déterminer les détenteurs d’autorisations sur différents objets dans le moteur de base de données SQL Server. SQL Server met en œuvre deux systèmes d’autorisations pour ce moteur de base de données. Un ancien système de rôles fixes dispose d’autorisations pré-configurées. Depuis SQL Server 2005, un système plus flexible et plus précis est disponible. (Les informations contenues dans cet article s’appliquent à SQL Server, à partir de 2005. Quelques types d’autorisations ne sont pas disponibles dans certaines versions de SQL Server.)

>  [!IMPORTANT] 
>  * Les autorisations effectives représentent l’agrégat des deux systèmes d’autorisations. 
>  * Les refus d’autorisation se substituent aux octrois d’autorisation. 
>  * Si un utilisateur est membre du rôle serveur fixe sysadmin, les autorisations ne sont pas vérifiées, et les refus d’accès ne sont donc pas appliqués. 
>  * L’ancien et le nouveau système présentent des similitudes. Par exemple, l’appartenance au rôle serveur fixe `sysadmin` revient à posséder l’autorisation `CONTROL SERVER`. Les systèmes, cependant, ne sont pas identiques. Par exemple, si une connexion ne dispose que de l’autorisation `CONTROL SERVER` et qu’une procédure stockée vérifie l’appartenance au rôle serveur fixe `sysadmin`, la vérification des autorisations échoue. L’inverse est également vrai. 


## <a name="summary"></a>Résumé   
* L’autorisation de niveau serveur peut provenir de l’appartenance aux rôles serveur fixes ou aux rôles serveur définis par l’utilisateur. Tout le monde appartient au rôle serveur fixe `public` et reçoit les autorisations qui y sont attribuées.   
* Les autorisations de niveau serveur peuvent provenir d’une autorisation attribuée à des connexions ou à des rôles serveur définis par l’utilisateur.   
* L’autorisation de niveau base de données peut émaner de l’appartenance aux rôles base de données fixes ou définis par l’utilisateur dans chaque base de données. Tout le monde appartient au rôle base de données fixe `public` et reçoit les autorisations qui y sont attribuées.   
* Les autorisations de niveau base de données peuvent provenir d’une autorisation attribuée à des utilisateurs ou à des rôles base de données définis par l’utilisateur dans chaque base de données.   
* Les autorisations peuvent être reçues à partir de la connexion `guest` ou de l’utilisateur de base de données `guest` s’ils sont activés. La connexion et les utilisateurs `guest` sont désactivés par défaut.   
* Les utilisateurs Windows peuvent être des membres de groupes Windows disposant de connexions. SQL Server détecte l’appartenance au groupe Windows lorsqu’un utilisateur Windows se connecte et présente un jeton Windows avec l’identificateur de sécurité d’un groupe Windows. Du fait que SQL Server ne gère pas et ne reçoit pas les mises à jour automatiques relatives aux appartenances de groupes Windows, il ne peut pas signaler de manière fiable les autorisations des utilisateurs Windows qui ont été reçues de l’appartenance au groupe Windows.   
* Les autorisations peuvent être acquises en basculant sur un rôle d’application et en fournissant le mot de passe associé.   
* Les autorisations peuvent être acquises en exécutant une procédure stockée qui comprend la clause `EXECUTE AS`.   
* Les autorisations peuvent être acquises par le biais de connexions ou d’utilisateurs dotés de l’autorisation `IMPERSONATE`.   
* Les membres du groupe des administrateurs locaux peuvent toujours élever leurs privilèges à `sysadmin`. (Ne s’applique pas à la base de données SQL.)  
* Les membres du rôle serveur fixe `securityadmin` peuvent élever la plupart de leurs privilèges et dans certains cas les élever jusqu’à `sysadmin`. (Ne s’applique pas à la base de données SQL.)   
* Les administrateurs SQL Server peuvent voir les informations se rapportant à toutes les connexions et à tous les utilisateurs. Les utilisateurs avec moins de privilèges ne voient généralement que les informations relatives à leur propre identité.

## <a name="older-fixed-role-permission-system"></a>Ancien système d’autorisations de rôle fixe

Il est impossible de modifier les rôles serveur et base de données fixes qui disposent d’autorisations pré-configurées. Pour déterminer qui est membre d’un rôle serveur fixe, exécutez la requête suivante :    
>  [!NOTE] 
>  Ne s’applique pas à SQL Database ni à SQL Data Warehouse, où les autorisations de niveau serveur ne sont pas disponibles. La colonne `is_fixed_role` de `sys.server_principals` a été ajoutée dans SQL Server 2012. Elle n’est pas nécessaire pour les versions antérieures de SQL Server.  
```sql
SELECT SP1.name AS ServerRoleName, 
 isnull (SP2.name, 'No members') AS LoginName   
 FROM sys.server_role_members AS SRM
 RIGHT OUTER JOIN sys.server_principals AS SP1
   ON SRM.role_principal_id = SP1.principal_id
 LEFT OUTER JOIN sys.server_principals AS SP2
   ON SRM.member_principal_id = SP2.principal_id
 WHERE SP1.is_fixed_role = 1 -- Remove for SQL Server 2008
 ORDER BY SP1.name;
```
>  [!NOTE] 
>  * Toutes les connexions sont membres du rôle public et il est impossible de les supprimer. 
>  * Cette requête vérifie les tables dans la base de données master, mais elle peut être exécutée dans n’importe quelle base de données du produit local. 

Pour déterminer qui est membre d’un rôle base de données fixe, exécutez la requête suivante dans chaque base de données.
```sql
SELECT DP1.name AS DatabaseRoleName, 
   isnull (DP2.name, 'No members') AS DatabaseUserName 
 FROM sys.database_role_members AS DRM
 RIGHT OUTER JOIN sys.database_principals AS DP1
   ON DRM.role_principal_id = DP1.principal_id
 LEFT OUTER JOIN sys.database_principals AS DP2
   ON DRM.member_principal_id = DP2.principal_id
 WHERE DP1.is_fixed_role = 1
 ORDER BY DP1.name;
```
Pour comprendre les autorisations accordées à chaque rôle, consultez les descriptions de rôle dans les illustrations de la documentation en ligne ([Rôles au niveau du serveur](../../../relational-databases/security/authentication-access/server-level-roles.md) et [Rôles au niveau de la base de données](../../../relational-databases/security/authentication-access/database-level-roles.md)).

## <a name="newer-granular-permission-system"></a>Système d’autorisations granulaire plus récent

Ce système est souple, ce qui signifie qu’il peut être plus complexe si la configuration voulue doit être précise. Pour simplifier les choses, il permet de créer des rôles, d’attribuer des autorisations aux rôles, puis d’ajouter des groupes d’utilisateurs à ces rôles. Et c’est encore plus simple si l’équipe de développement de la bases de données sépare l’activité par schémas, pour accorder ensuite des autorisations de rôle à un schéma entier plutôt qu’à des procédures ou des tables individuelles. Les scénarios réels sont complexes et les besoins de l’entreprise peuvent aboutir à des exigences de sécurité inattendues.   

[!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]


### <a name="security-classes"></a>Classes de sécurité

Vous pouvez accorder des autorisations au niveau serveur, base de données, schéma, objet, etc. Il existe 26 niveaux (appelés classes). La liste complète des classes dans l’ordre alphabétique est la suivante : `APPLICATION ROLE`, `ASSEMBLY`, `ASYMMETRIC KEY`, `AVAILABILITY GROUP`, `CERTIFICATE`, `CONTRACT`, `DATABASE`, `DATABASE` `SCOPED CREDENTIAL`, `ENDPOINT`, `FULLTEXT CATALOG`, `FULLTEXT STOPLIST`, `LOGIN`, `MESSAGE TYPE`, `OBJECT`, `REMOTE SERVICE BINDING`, `ROLE`, `ROUTE`, `SCHEMA`, `SEARCH PROPERTY LIST`, `SERVER`, `SERVER ROLE`, `SERVICE`, `SYMMETRIC KEY`, `TYPE`, `USER`, `XML SCHEMA COLLECTION`. (Certaines classes ne sont pas disponibles sur plusieurs types de serveurs SQL.) Une autre requête est nécessaire pour fournir des informations complètes sur chaque classe.

### <a name="principals"></a>Principaux

Des autorisations sont accordées aux principaux. Ces principaux peuvent être des rôles de serveur, des connexions, des rôles de base de données ou des utilisateurs. Les connexions peuvent représenter des groupes Windows incluant de nombreux utilisateurs Windows. Comme la plateforme SQL Server ne tient pas à jour les groupes Windows, elle ne sait pas toujours qui est membre d’un groupe Windows. Lorsqu’un utilisateur Windows se connecte à SQL Server, le paquet de connexion associé contient les jetons de l’appartenance de l’utilisateur au groupe Windows.

Lorsqu’un utilisateur Windows se connecte au moyen d’une connexion de groupe Windows, certaines activités peuvent obliger SQL Server à créer une connexion ou un utilisateur pour représenter l’utilisateur Windows individuel. Par exemple, un groupe Windows (Techniciens) contient les utilisateurs (Mary, Todd, Pat) et ce groupe de techniciens dispose d’un compte d’utilisateur de base de données. Si Mary a l’autorisation et crée une table, vous pouvez créer un utilisateur (Mary) pour qu’il soit propriétaire de la table. Par ailleurs, si une autorisation est refusée à Todd alors qu’elle est détenue par le reste du groupe de techniciens, vous devez créer l’utilisateur Todd afin de suivre le refus d’autorisation.

N’oubliez pas qu’un utilisateur Windows peut être membre de plusieurs groupes Windows (par exemple, le groupe des techniciens et celui des responsables). Qu’elles soient accordées ou refusées à la connexion des techniciens, à celle des responsables, à l’utilisateur individuellement, aux rôles dont l’utilisateur est membre, les autorisations seront toutes agrégées et évaluées pour les autorisations effectives. La fonction `HAS_PERMS_BY_NAME` permet de savoir si un utilisateur ou une connexion dispose d’une autorisation particulière. Toutefois, il n’existe aucun moyen avéré de déterminer la source de l’octroi ou du refus d’une autorisation. Examinez la liste des autorisations et procédez éventuellement par tâtonnements.

## <a name="useful-queries"></a>Requêtes utiles

### <a name="server-permissions"></a>Autorisations de serveur

La requête suivante retourne la liste des autorisations qui ont été accordées ou refusées au niveau serveur. Vous devez exécuter cette requête dans la base de données master.   
>  [!NOTE] 
>  Les autorisations de niveau serveur ne peuvent pas être accordées ou faire l’objet d’une requête sur la base de données SQL ou SQL Data Warehouse.   
```sql
SELECT pr.type_desc, pr.name, 
 isnull (pe.state_desc, 'No permission statements') AS state_desc, 
 isnull (pe.permission_name, 'No permission statements') AS permission_name 
 FROM sys.server_principals AS pr
 LEFT OUTER JOIN sys.server_permissions AS pe
   ON pr.principal_id = pe.grantee_principal_id
 WHERE is_fixed_role = 0 -- Remove for SQL Server 2008
 ORDER BY pr.name, type_desc;
```

### <a name="database-permissions"></a>Autorisations de base de données

La requête suivante retourne la liste des autorisations qui ont été accordées ou refusées au niveau base de données. Vous devez exécuter cette requête dans chaque base de données.   
```sql
SELECT pr.type_desc, pr.name, 
 isnull (pe.state_desc, 'No permission statements') AS state_desc, 
 isnull (pe.permission_name, 'No permission statements') AS permission_name 
FROM sys.database_principals AS pr
LEFT OUTER JOIN sys.database_permissions AS pe
    ON pr.principal_id = pe.grantee_principal_id
WHERE pr.is_fixed_role = 0 
ORDER BY pr.name, type_desc;
```

Chaque classe d’autorisations de la table d’autorisations peut être jointe à d’autres vues système qui fournissent des informations relatives à cette classe d’éléments sécurisables. Par exemple, la requête suivante fournit le nom de l’objet de base de données qui est concerné par l’autorisation.    
```sql
SELECT pr.type_desc, pr.name, pe.state_desc, 
 pe.permission_name, s.name + '.' + oj.name AS Object, major_id
 FROM sys.database_principals AS pr
 JOIN sys.database_permissions AS pe
   ON pr.principal_id = pe.grantee_principal_id
 JOIN sys.objects AS oj
   ON oj.object_id = pe.major_id
 JOIN sys.schemas AS s
   ON oj.schema_id = s.schema_id
 WHERE class_desc = 'OBJECT_OR_COLUMN';
```
Utilisez la fonction `HAS_PERMS_BY_NAME` pour déterminer si un utilisateur particulier (dans ce cas `TestUser`) dispose d’une autorisation. Exemple :   
```sql
EXECUTE AS USER = 'TestUser';
SELECT HAS_PERMS_BY_NAME ('dbo.T1', 'OBJECT', 'SELECT');
REVERT;
```
Pour obtenir des détails sur la syntaxe, consultez [HAS_PERMS_BY_NAME](../../../t-sql/functions/has-perms-by-name-transact-sql.md).

## <a name="see-also"></a>Voir aussi :

[Bien démarrer avec les autorisations du moteur de base de données](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)    
[Didacticiel : Mise en route du moteur de base de données](Tutorial:%20Getting%20Started%20with%20the%20Database%20Engine.md) 

