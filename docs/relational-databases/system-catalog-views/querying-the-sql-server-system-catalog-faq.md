---
title: Interrogation des catalogues système SQL Server FAQ | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], examples
- metadata [SQL Server], frequently asked questions
- metadata [SQL Server], example queries
- system catalogs [SQL Server], example queries
- catalog views [SQL Server], frequently asked questions
ms.assetid: ca202580-c37e-4ccd-9275-77ce79481f64
caps.latest.revision: 51
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: fc310dc86a720dbf0bd2a833a6bedd63f1875b27
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="querying-the-sql-server-system-catalog-faq"></a>Questions fréquentes sur l'interrogation des catalogues système de SQL Server
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique contient une liste de questions fréquemment posées. Les réponses à ces questions sont des requêtes basées sur des affichages catalogue.  
  
##  <a name="_TOP"></a> Forum aux Questions  
 Les sections ci-dessous présentent les questions les plus fréquemment posées par catégorie.  
  
### <a name="data-types"></a>Types de données  
  
-   [Comment pour rechercher les types de données des colonnes d’une table spécifiée ?](#_FAQ7)  
  
-   [Comment pour rechercher les types de données LOB d’une table spécifiée ?](#_FAQ14)  
  
-   [Comment pour rechercher les colonnes qui dépendent d’un type de données spécifié ?](#_FAQ22)  
  
-   [Comment pour rechercher les colonnes calculées qui dépendent d’un type CLR défini par l’utilisateur ou d’un type de données alias ?](#_FAQ23)  
  
-   [Comment pour rechercher les paramètres qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?](#_FAQ24)  
  
-   [Comment pour trouver les contraintes CHECK qui dépendent d’un type CLR défini par l’utilisateur spécifié ?](#_FAQ25)  
  
-   [Comment trouver les vues, les fonctions Transact-SQL et les procédures stockées Transact-SQL qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?](#_FAQ26)  
  
### <a name="tables-indexes-views-and-constraints"></a>Tables, index, vues et contraintes  
  
-   [Comment trouver toutes les tables définies par l’utilisateur dans une base de données spécifié ?](#_FAQ31)  
  
-   [Comment pour rechercher toutes les tables qui n’ont pas d’un index cluster dans une base de données spécifié ?](#_FAQ1)  
  
-   [Comment pour rechercher toutes les tables qui ne comportent pas d’index ?](#_FAQ4)  
  
-   [Comment pour rechercher toutes les tables qui n’ont pas d’une clé primaire ?](#_FAQ3)  
  
-   [Comment pour rechercher toutes les tables qui ont une colonne d’identité ?](#_FAQ5)  
  
-   [Comment rechercher toutes les tables et les index qui sont partitionnés ?](#_FAQ32)  
  
-   [Comment trouver toutes les vues dans une base de données ?](#_FAQ13)  
  
-   [Comment pour rechercher la définition d’une vue ?](#_FAQ35)  
  
-   [Comment faire pour rechercher toutes les entités qui ont été modifiées dans les dernier N jours ?](#_FAQ6)  
  
-   [Comment pour rechercher les colonnes d’une clé primaire pour une table spécifiée ?](#_FAQ16)  
  
-   [Comment pour rechercher les colonnes d’une clé étrangère d’une table spécifiée ?](#_FAQ17)  
  
-   [Comment déterminer si une colonne est utilisée dans une expression de colonne calculée ?](#_FAQ20)  
  
-   [Comment trouver toutes les colonnes qui sont utilisés dans une expression de colonne calculée ?](#_FAQ21)  
  
-   [Comment trouver toutes les contraintes pour une table spécifiée ?](#_FAQ27)  
  
-   [Comment trouver tous les index pour une table spécifiée ?](#_FAQ28)  
  
-   [Comment pour rechercher toutes les tables qui ont un nom de colonne spécifié ?](#_FAQ30)  
  
-   [Comment trouver toutes les statistiques sur un objet spécifié ?](#_FAQ33)  
  
-   [Comment trouver les statistiques et les colonnes de statistiques sur un objet spécifié ?](#_FAQ34)  
  
### <a name="modules-stored-procedures-user-defined-functions-and-triggers"></a>Modules (procédures stockées, fonctions définies par l'utilisateur et déclencheurs)  
  
-   [Comment pour rechercher toutes les procédures stockées dans une base de données ?](#_FAQ9)  
  
-   [Comment pour rechercher toutes les fonctions définies par l’utilisateur dans une base de données ?](#_FAQ12)  
  
-   [Comment trouver les paramètres pour une fonction ou procédure stockée spécifiée ?](#_FAQ10)  
  
-   [Comment trouver les dépendances sur une fonction spécifiée ?](#_FAQ8)  
  
-   [Comment pour afficher la définition d’un module ?](#_FAQ15)  
  
-   [Comment pour afficher la définition d’un déclencheur de niveau serveur ?](#_FAQ19)  
  
### <a name="schemas-users-roles-and-permissions"></a>Schémas, utilisateurs, rôles et autorisations  
  
-   [Comment faire pour rechercher tous les propriétaires des entités contenues dans un schéma spécifié ?](#_FAQ2)  
  
-   [Comment faire pour rechercher les autorisations accordées ou refusées à un principal spécifié ?](#_FAQ18)  
  
## <a name="answers"></a>Réponses  
  
###  <a name="_FAQ1"></a> Comment pour rechercher toutes les tables qui n’ont pas d’un index cluster dans une base de données spécifié ?  
 Avant d'exécuter les requêtes suivantes, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name, t.name AS table_name  
FROM sys.tables AS t  
WHERE NOT EXISTS   
   (  
     SELECT * FROM sys.indexes AS i  
     WHERE i.object_id = t.object_id  
     AND i.type = 1  -- or type_desc = 'CLUSTERED'  
   )  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 Vous pouvez aussi utiliser la fonction `OBJECTPROPERTY`, comme le montre l'exemple suivant.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name, name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasClustIndex') = 0  
ORDER BY schema_id, name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ2"></a> Comment faire pour rechercher tous les propriétaires des entités contenues dans un schéma spécifié ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ3"></a> Comment pour rechercher toutes les tables qui n’ont pas d’une clé primaire ?  
 Avant d'exécuter les requêtes suivantes, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name  
    ,t.name AS table_name  
FROM sys.tables t   
WHERE object_id NOT IN   
   (  
    SELECT parent_object_id   
    FROM sys.key_constraints   
    WHERE type_desc = 'PRIMARY_KEY_CONSTRAINT' -- or type = 'PK'  
    );  
GO  
  
```  
  
 Vous pouvez également exécuter la requête suivante.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ4"></a> Comment pour rechercher toutes les tables qui ne comportent pas d’index ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'IsIndexed') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ5"></a> Comment pour rechercher toutes les tables qui ont une colonne d’identité ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    , t.name AS table_name  
    , c.name AS column_name  
FROM sys.tables AS t  
JOIN sys.identity_columns c ON t.object_id = c.object_id  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 Vous pouvez également exécuter la requête suivante.  
  
> [!NOTE]  
>  Cette requête ne retourne pas le nom des colonnes.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasIdentity') = 1  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ7"></a> Comment pour rechercher les types de données des colonnes d’une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT c.name AS column_name  
    ,c.column_id  
    ,SCHEMA_NAME(t.schema_id) AS type_schema  
    ,t.name AS type_name  
    ,t.is_user_defined  
    ,t.is_assembly_type  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
FROM sys.columns AS c   
JOIN sys.types AS t ON c.user_type_id=t.user_type_id  
WHERE c.object_id = OBJECT_ID('<schema_name.table_name>')  
ORDER BY c.column_id;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ8"></a> Comment trouver les dépendances sur une fonction spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.function_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS referencing_object_name  
    ,COALESCE(COL_NAME(object_id, column_id), '(n/a)') AS referencing_column_name  
    ,*  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.function_name>')  
ORDER BY OBJECT_NAME(object_id), COL_NAME(object_id, column_id);  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ9"></a> Comment pour rechercher toutes les procédures stockées dans une base de données ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide.  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS procedure_name   
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
FROM sys.procedures;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ10"></a> Comment trouver les paramètres pour une fonction ou procédure stockée spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.object_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ12"></a> Comment pour rechercher toutes les fonctions définies par l’utilisateur dans une base de données ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ13"></a> Comment trouver toutes les vues dans une base de données ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom de base de données valide.  
  
```  
USE <database_name>;  
GO  
SELECT name AS view_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,OBJECTPROPERTYEX(object_id,'IsIndexed') AS IsIndexed  
  ,OBJECTPROPERTYEX(object_id,'IsIndexable') AS IsIndexable  
  ,create_date  
  ,modify_date  
FROM sys.views;  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ6"></a> Comment faire pour rechercher toutes les entités qui ont été modifiées dans les dernier N jours ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<n_days>` par des valeurs valides.  
  
```  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ14"></a> Comment pour rechercher les types de données LOB d’une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS column_name   
    ,column_id   
    ,TYPE_NAME(user_type_id) AS type_name  
    ,max_length  
    ,CASE   
       WHEN max_length = -1 AND TYPE_NAME(user_type_id) <> 'xml'  
            THEN 1  
            ELSE 0  
     END AS [(max)]  
FROM sys.columns  
WHERE object_id=OBJECT_ID('<schema_name.table_name>')   
    AND ( TYPE_NAME(user_type_id) IN ('xml','text', 'ntext','image')  
         OR (TYPE_NAME(user_type_id) IN ('varchar','nvarchar','varbinary')  
         AND max_length = -1)  
        );  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ15"></a> Comment pour afficher la définition d’un module ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.object_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 Vous pouvez aussi utiliser la fonction `OBJECT_DEFINITION`, comme le montre l'exemple suivant.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ19"></a> Comment pour afficher la définition d’un déclencheur de niveau serveur ?  
  
```  
SELECT definition  
FROM sys.server_sql_modules;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ16"></a> Comment pour rechercher les colonnes d’une clé primaire pour une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,ic.index_column_id  
    ,key_ordinal  
    ,c.name AS column_name  
    ,TYPE_NAME(c.user_type_id)AS column_type   
    ,is_identity  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
INNER JOIN sys.columns AS c   
    ON ic.object_id = c.object_id AND c.column_id = ic.column_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 Vous pouvez aussi utiliser la fonction `COL_NAME`, comme le montre l'exemple suivant.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,key_ordinal  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ17"></a> Comment pour rechercher les colonnes d’une clé étrangère d’une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT   
    f.name AS foreign_key_name  
   ,OBJECT_NAME(f.parent_object_id) AS table_name  
   ,COL_NAME(fc.parent_object_id, fc.parent_column_id) AS constraint_column_name  
   ,OBJECT_NAME (f.referenced_object_id) AS referenced_object  
   ,COL_NAME(fc.referenced_object_id, fc.referenced_column_id) AS referenced_column_name  
   ,is_disabled  
   ,delete_referential_action_desc  
   ,update_referential_action_desc  
FROM sys.foreign_keys AS f  
INNER JOIN sys.foreign_key_columns AS fc   
   ON f.object_id = fc.constraint_object_id   
WHERE f.parent_object_id = OBJECT_ID('<schema_name.table_name>');  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ18"></a> Comment faire pour rechercher les autorisations accordées ou refusées à un principal spécifié ?  
 L'exemple suivant crée une fonction pour renvoyer le nom de l'entité dont les autorisations sont vérifiées. La fonction est appelée dans les requêtes qui suivent. La fonction doit être créée dans chaque base de données dans laquelle vous voulez vérifier les autorisations.  
  
```  
-- Create a function to return the name of the entity on which the permissions are checked.  
IF OBJECT_ID (N'dbo.entity_instance_name', N'FN') IS NOT NULL  
    DROP FUNCTION dbo.entity_instance_name;  
GO  
CREATE FUNCTION dbo.entity_instance_name(@class_desc nvarchar(60), @major_id int)   
RETURNS sysname AS  
BEGIN  
    DECLARE @the_entity_name sysname  
    SELECT @the_entity_name = CASE  
        WHEN @class_desc = 'DATABASE' THEN DB_NAME()  
        WHEN @class_desc = 'SCHEMA' THEN SCHEMA_NAME(@major_id)  
        WHEN @class_desc = 'OBJECT_OR_COLUMN' THEN OBJECT_NAME(@major_id)  
        WHEN @class_desc = 'DATABASE_PRINCIPAL' THEN USER_NAME(@major_id)  
        WHEN @class_desc = 'ASSEMBLY' THEN   
            (SELECT name FROM sys.assemblies WHERE assembly_id=@major_id)  
        WHEN @class_desc = 'TYPE' THEN TYPE_NAME(@major_id)  
        WHEN @class_desc = 'XML_SCHEMA_COLLECTION' THEN   
            (SELECT name FROM sys.xml_schema_collections  
              WHERE xml_collection_id=@major_id)  
        WHEN @class_desc = 'MESSAGE_TYPE' THEN   
            (SELECT name FROM sys.service_message_types WHERE message_type_id=@major_id)  
        WHEN @class_desc = 'SERVICE_CONTRACT' THEN   
           (SELECT name FROM sys.service_contracts  
              WHERE service_contract_id=@major_id)  
        WHEN @class_desc = 'SERVICE' THEN  
          (SELECT name FROM sys.services WHERE service_id=@major_id)  
        WHEN @class_desc = 'REMOTE_SERVICE_BINDING' THEN  
          (SELECT name FROM sys.remote_service_bindings  
             WHERE remote_service_binding_id=@major_id)  
        WHEN @class_desc = 'ROUTE' THEN  
          (SELECT name FROM sys.routes WHERE route_id=@major_id)  
        WHEN @class_desc = 'FULLTEXT_CATALOG' THEN  
          (SELECT name FROM sys.fulltext_catalogs WHERE fulltext_catalog_id=@major_id)  
        WHEN @class_desc = 'SYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.symmetric_keys WHERE symmetric_key_id=@major_id)  
        WHEN @class_desc = 'CERTIFICATE' THEN  
          (SELECT name FROM sys.certificates WHERE certificate_id=@major_id)  
        WHEN @class_desc = 'ASYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.asymmetric_keys WHERE asymmetric_key_id=@major_id)  
        WHEN @class_desc = 'SERVER' THEN   
             (SELECT name FROM sys.servers WHERE server_id=@major_id)  
        WHEN @class_desc = 'SERVER_PRINCIPAL' THEN SUSER_NAME(@major_id)  
        WHEN @class_desc = 'ENDPOINT' THEN   
             (SELECT name FROM sys.endpoints WHERE endpoint_id=@major_id)        
        ELSE '?'  
    END  
    RETURN @the_entity_name  
END;  
GO  
-- Return server-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc, major_id) AS entity_name   
    ,minor_id  
    ,SUSER_NAME(grantee_principal_id) AS grantee  
    ,SUSER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc   
FROM sys.server_permissions   
WHERE grantee_principal_id = SUSER_ID('public');  
GO  
-- Return database-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc , major_id) AS entity_name   
    ,minor_id  
    ,USER_NAME(grantee_principal_id) AS grantee  
    ,USER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc     
FROM  sys.database_permissions   
WHERE grantee_principal_id = DATABASE_PRINCIPAL_ID('public');  
GO  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ20"></a> Comment déterminer si une colonne est utilisée dans une expression de colonne calculée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>`, `<schema_name.table_name>`, et `<column_name`> par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS computed_column   
    ,class_desc  
    ,is_selected  
    ,is_updated  
    ,is_select_all  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.table_name>')  
    AND referenced_minor_id = COLUMNPROPERTY(referenced_major_id, '<column_name>', 'ColumnId')  
    AND class = 1;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ21"></a> Comment trouver toutes les colonnes qui sont utilisés dans une expression de colonne calculée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(d.referenced_major_id) AS object_name  
    ,COL_NAME(d.referenced_major_id, d.referenced_minor_id) AS column_name  
    ,OBJECT_NAME(referenced_major_id) AS dependent_object_name   
    ,COL_NAME(d.object_id, d.column_id) AS dependent_computed_column  
    ,cc.definition AS computed_column_definition  
FROM sys.sql_dependencies AS d  
JOIN sys.computed_columns AS cc   
    ON cc.object_id = d.object_id AND cc.column_id = d.column_id AND d.object_id=d.referenced_major_id       
WHERE d.class = 1  
ORDER BY object_name, column_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ22"></a> Comment pour rechercher les colonnes qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?  
 Avant d’exécuter la requête suivante, remplacez `<database_name>` avec un nom valide et `<schema_name.data_type_name>` avec un type CLR défini par l’utilisateur valid, qualifié par un schéma, ou le nom de type qualifié par un schéma un alias. La requête suivante requiert l’appartenance à la **db_owner** rôle ou des autorisations pour voir toutes les colonnes calculées et colonnes dépendantes métadonnées dans la base de données.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,c.name AS column_name   
    ,SCHEMA_NAME(t.schema_id) AS schema_name  
    ,TYPE_NAME(c.user_type_id) AS user_type_name  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
    ,c.is_nullable  
    ,c.is_computed  
FROM sys.columns AS c  
INNER JOIN sys.types AS t ON c.user_type_id = t.user_type_id  
WHERE c.user_type_id = TYPE_ID('<schema_name.data_type_name>');   
GO  
  
```  
  
 La requête suivante retourne une vue étroite et restreinte des colonnes dépendantes d’un alias ou un type CLR défini par l’utilisateur, mais le jeu de résultats est visible pour le **public** rôle. Vous pouvez utiliser cette requête si vous avez accordé à d'autres les autorisations REFERENCE sur votre type défini par l'utilisateur et que vous n'êtes pas autorisé à afficher les métadonnées des objets créés par d'autres utilisant ce type.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,COL_NAME(object_id, column_id) AS column_name  
    ,TYPE_NAME(user_type_id) AS user_type  
FROM sys.column_type_usages  
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ23"></a> Comment pour rechercher les colonnes calculées qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide et `<schema_name.data_type_name>` par un type CLR défini par l'utilisateur qualifié par un schéma valide, ou un nom de type alias.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS column_name  
FROM sys.sql_dependencies  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(object_id, 'IsTable') = 1;   -- exclude non-table dependencies  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ24"></a> Comment pour rechercher les paramètres qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide et `<schema_name.data_type_name>` par un type CLR défini par l'utilisateur qualifié par un schéma valide, ou un nom de type alias. La requête suivante requiert l’appartenance à la **db_owner** rôle ou des autorisations pour voir toutes les colonnes calculées et colonnes dépendantes métadonnées dans la base de données.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,NULL AS procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
UNION   
SELECT OBJECT_NAME(object_id) AS object_name  
    ,procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.numbered_procedure_parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY object_name, procedure_number, param_num;  
GO  
  
```  
  
 La requête suivante retourne une vue étroite et restreinte des paramètres qui dépendent d’un type CLR défini par l’utilisateur ou un alias, mais le jeu de résultats est visible pour le **public** rôle. Vous pouvez utiliser cette requête si vous avez accordé à d'autres les autorisations REFERENCE sur votre type défini par l'utilisateur et que vous n'êtes pas autorisé à afficher les métadonnées des objets créés par d'autres utilisant ce type.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,parameter_id  
    ,TYPE_NAME(user_type_id) AS type_name  
FROM sys.parameter_type_usages   
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ25"></a> Comment pour trouver les contraintes CHECK qui dépendent d’un type CLR défini par l’utilisateur spécifié ?  
 Avant d’exécuter la requête suivante, remplacez `<database_name>` avec un nom valide et `<schema_name.data_type_name>` avec un nom de type défini par l’utilisateur CLR valid, qualifié par un schéma.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(o.parent_object_id) AS table_name  
    ,OBJECT_NAME(o.object_id) AS constraint_name  
FROM sys.sql_dependencies AS d  
JOIN sys.objects AS o ON o.object_id = d.object_id  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(o.object_id, 'IsCheckCnst') = 1; -- exclude non-CHECK dependencies  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ26"></a> Comment trouver les vues, les fonctions Transact-SQL et les procédures stockées Transact-SQL qui dépendent d’un type défini par l’utilisateur CLR ou d’un type d’alias ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide et `<schema_name.data_type_name>` par un type CLR défini par l'utilisateur qualifié par un schéma valide, ou un nom de type alias.  
  
 Les paramètres définis dans une fonction ou une procédure sont implicitement liés à un schéma. Par conséquent, les paramètres qui dépendent d’un type CLR défini par l’utilisateur ou un type d’alias peuvent être affichés à l’aide de la [sys.sql_dependencies](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md) affichage catalogue. Les procédures et les déclencheurs ne sont pas liés au schéma. Cela signifie l'interruption des dépendances entre une expression définie dans le corps de la procédure ou du déclencheur et un type d'alias ou un type CLR défini par l'utilisateur. Vues liées à un schéma et des fonctions définies par l’utilisateur qui contiennent des expressions qui dépendent d’un type défini par l’utilisateur CLR liée au schéma ou un type d’alias sont maintenues dans le **sys.sql_dependencies** affichage catalogue. Les dépendances entre des types et des fonctions et des procédures CLR ne sont pas maintenues.  
  
 La requête suivante retourne toutes les dépendances liées à un schéma dans des vues, des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] et des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] pour un type CLR défini par l'utilisateur ou un type d'alias.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS dependent_object_schema  
  ,OBJECT_NAME(o.object_id) AS dependent_object_name  
  ,o.type_desc AS dependent_object_type  
  ,d.class_desc AS kind_of_dependency  
  ,TYPE_NAME (d.referenced_major_id) AS type_name  
FROM sys.sql_dependencies AS d   
JOIN sys.objects AS o  
  ON d.object_id = o.object_id  
  AND o.type IN ('FN','IF','TF', 'V', 'P')  
WHERE d.class = 2 -- dependencies on types  
  AND d.referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY dependent_object_schema, dependent_object_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ27"></a> Comment trouver toutes les contraintes pour une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) as constraint_name  
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,OBJECT_NAME(parent_object_id) AS table_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
    ,is_ms_shipped  
    ,is_published  
    ,is_schema_published  
FROM sys.objects  
WHERE type_desc LIKE '%CONSTRAINT'   
    AND parent_object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ28"></a> Comment trouver tous les index pour une table spécifiée ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.table_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ30"></a> Comment rechercher tous les objets qui ont un nom de colonne spécifié ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<column_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id)  
FROM sys.columns  
WHERE name = '<column_name>';  
GO  
  
```  
  
 ou  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name   
    ,o.name AS object_name  
    ,type_desc  
FROM sys.objects AS o  
INNER JOIN sys.columns AS c ON o.object_id = c.object_id  
WHERE c.name = '<column_name>';  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ31"></a> Comment trouver toutes les tables définies par l’utilisateur dans une base de données spécifié ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide.  
  
```  
USE <database_name>;  
GO  
SELECT *   
FROM sys.tables;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ32"></a> Comment rechercher toutes les tables et les index qui sont partitionnés ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide.  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(p.object_id) AS table_name  
    ,i.name AS index_name  
    ,p.partition_number  
    ,rows   
FROM sys.partitions AS p  
INNER JOIN sys.indexes AS i ON p.object_id = i.object_id AND p.index_id = i.index_id  
INNER JOIN sys.partition_schemes ps ON i.data_space_id=ps.data_space_id  
INNER JOIN sys.objects AS o ON o.object_id = i.object_id  
ORDER BY index_name, partition_number;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ33"></a> Comment trouver toutes les statistiques sur un objet spécifié ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide et `<schema_name.object_name>` par une table valide, une vue indexée ou un nom de fonction table.  
  
```  
USE <database_name>;  
GO  
SELECT name AS statistics_name  
    ,stats_id  
    ,auto_created  
    ,user_created  
    ,no_recompute  
FROM sys.stats  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ34"></a> Comment trouver les statistiques et les colonnes de statistiques sur un objet spécifié ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` par un nom valide et `<schema_name.object_name>` par une table valide, une vue indexée ou un nom de fonction table.  
  
```  
USE <database_name>;  
GO  
SELECT s.name AS statistics_name  
    ,c.name AS column_name  
    ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="_FAQ35"></a> Comment pour rechercher la définition d’une vue ?  
 Avant d'exécuter la requête suivante, remplacez `<database_name>` et `<schema_name.object_name>` par des noms valides.  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 Vous pouvez aussi utiliser la fonction `OBJECT_DEFINITION`, comme le montre l'exemple suivant.  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
