---
title: Sys.fn_get_audit_file (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_get_audit_file_TSQL
- sys.fn_get_audit_file_TSQL
- fn_get_audit_file
- sys.fn_get_audit_file
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_get_audit_file function
- fn_get_audit_file function
ms.assetid: d6a78d14-bb1f-4987-b7b6-579ddd4167f5
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e09491347fec046bef09dc2fca06756998055150
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfngetauditfile-transact-sql"></a>sys.fn_get_audit_file (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations à partir d'un fichier d'audit créé par un audit du serveur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SQL Server Audit &#40moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn_get_audit_file ( file_pattern,   
    { default | initial_file_name | NULL },   
    { default | audit_record_offset | NULL } )  
```  
  
## <a name="arguments"></a>Arguments  
 *file_pattern*  
 Spécifie le répertoire ou chemin d'accès et nom de fichier du jeu de fichiers d'audit à lire. Est de type **nvarchar (260)**. 
 
 - **SQL Server**:
    
    Cet argument doit inclure à la fois un chemin d'accès (lettre de lecteur ou partage réseau) et un nom de fichier qui peut inclure un caractère générique. Un astérisque (*) peut être utilisé pour recueillir plusieurs fichiers à partir d’un jeu de fichiers d’audit. Par exemple :  
  
    -   **\<chemin d’accès >\\ \***  - collecter tous les fichiers d’audit à l’emplacement spécifié.  
  
    -   **\<chemin d’accès > \LoginsAudit_{GUID}** - collecter tous les fichiers ayant le nom spécifié et une paire GUID d’audit.  
  
    -   **\<chemin d’accès > \LoginsAudit_{GUID}_00_29384.sqlaudit** -collecter un fichier d’audit spécifiques.  
  
 - **Base de données SQL Azure**:
 
    Cet argument est utilisé pour spécifier une URL de blob (y compris le point de terminaison de stockage et le conteneur). Pendant qu’il ne prend pas en charge un astérisque comme caractère générique, vous pouvez utiliser un préfixe de nom de fichier partiel (blob) (au lieu du nom de l’objet blob complet) pour recueillir plusieurs fichiers (BLOB) qui commencent par ce préfixe. Par exemple :
 
      - **\<Storage_endpoint\>/\<conteneur\>/\<nom_serveur\>/\<DatabaseName\> /**  -collecte tous les fichiers d’audit (BLOB) pour la base de données spécifique.    
      
      - **\<Storage_endpoint\>/\<conteneur\>/\<nom_serveur\>/\<DatabaseName\> / \< AuditName\>/\<CreationDate\>/\<nom de fichier\>.xel** -collecte un fichier d’audit (blob).
  
> [!NOTE]  
>  Le passage d'un chemin d'accès sans modèle de nom de fichier génère une erreur.  
  
 *initial_file_name*  
 Spécifie le chemin d'accès et le nom d'un fichier spécifique dans le jeu de fichiers d'audit à partir duquel commencer à lire des enregistrements d'audit. Est de type **nvarchar (260)**.  
  
> [!NOTE]  
>  Le *initial_file_name* argument doit contenir les entrées valides ou doit contenir la valeur par défaut | Valeur NULL.  
  
 *audit_record_offset*  
 Spécifie un emplacement connu avec le fichier spécifié pour l'initial_file_name. Lorsque cet argument est utilisé, la fonction commence à lire au premier enregistrement de la mémoire tampon qui suit le décalage spécifié.  
  
> [!NOTE]  
>  Le *audit_record_offset* argument doit contenir les entrées valides ou doit contenir la valeur par défaut | Valeur NULL. Est de type **bigint**.  
  
## <a name="tables-returned"></a>Tables retournées  
 Le tableau suivant décrit le contenu de fichier d'audit qui peut être retourné par cette fonction.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|event_time|**datetime2**|Date et heure auxquelles l'action pouvant être auditée est déclenchée. N'accepte pas la valeur NULL.|  
|sequence_number|**int**|Assure le suivi de la séquence d'enregistrements dans un enregistrement d'audit unique qui était trop grand pour la mémoire tampon d'écriture pour audits. N'accepte pas la valeur NULL.|  
|action_id|**varchar(4)**|ID de l'action. N'accepte pas la valeur NULL.|  
|succeeded|**bit**|Indique si l’action qui a déclenché l’événement a réussi. N'accepte pas la valeur NULL. Pour tous les événements autres que les événements de connexion, cet argument signale uniquement le succès ou l'échec de la vérification des autorisations, mais n'indique rien sur l'opération.<br /> 1 = succès<br /> 0 = échec|  
|permission_bitmask|**varbinary(16)**|Dans certaines actions, il s'agit des autorisations qui ont été accordées, refusées ou révoquées.|  
|is_column_permission|**bit**|Indicateur qui indique s'il s'agit d'une autorisation de niveau colonne. N'accepte pas la valeur NULL. Retourne 0 lorsque le permission_bitmask = 0.<br /> 1 = vrai<br /> 0 = faux|  
|session_id|**smallint**|ID de la session au cours de laquelle l'événement s'est produit. N'accepte pas la valeur NULL.|  
|server_principal_id|**int**|ID du contexte de connexion dans lequel l'action est effectuée. N'accepte pas la valeur NULL.|  
|database_principal_id|**int**|ID du contexte de l'utilisateur de base de données dans lequel l'action est effectuée. N'accepte pas la valeur NULL. Retourne 0 si cela ne s'applique pas. Par exemple, une opération de serveur.|  
|target_server_principal_id|**int**|Principal serveur sur lequel est effectuée l'opération GRANT/DENY/REVOKE. N'accepte pas la valeur NULL. Retourne 0 si non applicable.|  
|target_database_principal_id|**int**|Principal de la base de données sur lequel est effectuée l'opération GRANT/DENY/REVOKE. N'accepte pas la valeur NULL. Retourne 0 si non applicable.|  
|object_id|**int**|ID de l'entité sur laquelle l'audit s'est produit. Notamment :<br /> Objets de serveur<br /> Bases de données<br /> Objets de base de données<br /> Objets de schéma<br /> N'accepte pas la valeur NULL. Retourne 0 si l'entité est le serveur lui-même ou si l'audit n'est pas effectué à un niveau objet. Par exemple, Authentification.|  
|class_type|**varchar(2)**|Type d'entité pouvant être auditée sur laquelle l'audit se produit. N'accepte pas la valeur NULL.|  
|session_server_principal_name|**sysname**|Principal du serveur pour la session. Autorise la valeur NULL.|  
|server_principal_name|**sysname**|Connexion actuelle. Autorise la valeur NULL.|  
|server_principal_sid|**varbinary**|SID de la connexion actuelle. Autorise la valeur NULL.|  
|database_principal_name|**sysname**|Utilisateur actuel. Autorise la valeur NULL. Retourne NULL si non disponible.|  
|target_server_principal_name|**sysname**|Connexion cible de l’action. Autorise la valeur NULL. Retourne NULL si non applicable.|  
|target_server_principal_sid|**varbinary**|SID de connexion cible. Autorise la valeur NULL. Retourne NULL si non applicable.|  
|target_database_principal_name|**sysname**|Utilisateur cible de l’action. Autorise la valeur NULL. Retourne NULL si non applicable.|  
|server_instance_name|**sysname**|Nom de l'instance de serveur où l'audit s'est produit. Le format de server\instance standard est utilisé.|  
|database_name|**sysname**|Contexte de base de données dans lequel l'action s'est produite. Autorise la valeur NULL. Retourne NULL pour les audits qui ont lieu au niveau serveur.|  
|schema_name|**sysname**|Contexte de schéma dans lequel l'action s'est produite. Autorise la valeur NULL. Retourne NULL pour les audits qui ont lieu à l'extérieur d'un schéma.|  
|object_name|**sysname**|Nom de l'entité sur laquelle l'audit s'est produit. Notamment :<br /> Objets de serveur<br /> Bases de données<br /> Objets de base de données<br /> Objets de schéma<br /> Autorise la valeur NULL. Retourne NULL si l'entité est le serveur lui-même ou si l'audit n'est pas effectué à un niveau objet. Par exemple, Authentification.|  
|instruction|**nvarchar(4000)**|Instruction TSQL si elle existe. Autorise la valeur NULL. Retourne NULL si non applicable.|  
|additional_information|**nvarchar(4000)**|Les informations uniques qui s'appliquent seulement à un événement unique sont retournées au format XML. Un petit nombre d'actions pouvant être auditées contient ce type d'informations.<br /><br /> Un niveau de pile TSQL est affiché au format XML pour les actions auxquelles la pile TSQL est associée. Le format XML est le suivant :<br /><br /> `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br /> Niveau_imbrication_cadre indique le niveau d'imbrication actuel du cadre. Le nom du module est représenté dans un format en trois parties (nom_base_de_données, nom_schéma et nom_objet).  Le nom du module sera analysé pour échapper les caractères xml non valide comme `'\<'`, `'>'`, `'/'`, `'_x'`. Ils sont placés sous `_xHHHH\_`. HHHH représente le code UCS-2 hexadécimal à quatre chiffres du caractère.<br /><br /> Autorise la valeur NULL. Retourne NULL lorsqu'il n'y a pas d'informations supplémentaires signalées par l'événement.|  
|file_name|**varchar(260)**|Chemin d'accès et nom du fichier journal d'audit d'où provenait l'enregistrement. N'accepte pas la valeur NULL.|  
|audit_file_offset|**bigint**|Offset de la mémoire tampon dans le fichier qui contient l'enregistrement d'audit. N'accepte pas la valeur NULL.|  
|user_defined_event_id|**smallint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Id d’événement défini par utilisateur est passé comme argument à **sp_audit_write**. **NULL** pour les événements du système (par défaut) et différent de zéro pour l’événement défini par l’utilisateur. Pour plus d’informations, consultez [sp_audit_write &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md).|  
|user_defined_information|**nvarchar(4000)**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Permet d’enregistrer toute information supplémentaire que l’utilisateur souhaite enregistrer dans |journal d’audit à l’aide de la **sp_audit_write** procédure stockée.|  
|audit_schema_version |**int** | |  
|sequence_group_id |**varbinary** | **S’applique à**: SQL Server uniquement (à partir de 2016) |  
|transaction_id |**bigint** | **S’applique à**: SQL Server uniquement (à partir de 2016) |  
|client_ip |**nvarchar(128)** | **S’applique à**: base de données SQL Azure + SQL Server (à partir de 2017) |  
|application_name |**nvarchar(128)** | **S’applique à**: base de données SQL Azure + SQL Server (à partir de 2017) |  
|duration_milliseconds |**bigint** | **S’applique à**: base de données SQL Azure uniquement |  
|response_rows |**bigint** | **S’applique à**: base de données SQL Azure uniquement |  
|affected_rows |**bigint** | **S’applique à**: base de données SQL Azure uniquement |  
|connection_id |GUID | **S’applique à**: base de données SQL Azure uniquement |
|data_sensitivity_information |nvarchar(4000) | **S’applique à**: base de données SQL Azure uniquement |
  
## <a name="remarks"></a>Notes  
 Si le *file_pattern* argument passé à **fn_get_audit_file** fait référence à un chemin d’accès ou un fichier qui n’existe pas, ou si le fichier n’est pas un fichier d’audit, le **MSG_INVALID_AUDIT_FILE** message d’erreur est retourné.  
  
## <a name="permissions"></a>Autorisations  
 - **SQL Server**: requiert la **CONTROL SERVER** autorisation.  
 - **Base de données SQL Azure**: requiert la **base de données contrôle** autorisation.     
    - Les administrateurs de serveur peuvent accéder à des journaux d’audit de toutes les bases de données sur le serveur.
    - Administrateurs de serveur non accessible uniquement les journaux d’audit à partir de la base de données en cours.
    - Objets BLOB qui ne répondent pas aux critères ci-dessus seront ignorées (une liste d’objets BLOB a été ignorée s’affichera dans le message de sortie de requête), et la fonction retournera journaux uniquement à partir d’objets BLOB pour lesquels l’accès est autorisé.  
  
## <a name="examples"></a>Exemples

- **SQL Server**

  Cet exemple lit à partir d'un fichier nommé `\\serverName\Audit\HIPPA_AUDIT.sqlaudit`.  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('\\serverName\Audit\HIPPA_AUDIT.sqlaudit',default,default);  
  GO  
  ```  

- **Azure SQL Database**

  Cet exemple lit à partir d’un fichier nommé `ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel`:  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default);
  GO  
  ```  

  Cet exemple lit dans le même fichier, comme indiqué ci-dessus, mais avec les autres clauses de T-SQL (**haut**, **ORDER BY**, et **où** clause pour filtrer les enregistrements d’audit retournés par le fonction) :
  
  ```  
  SELECT TOP 10 * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default)
  WHERE server_principal_name = 'admin1'
  ORDER BY event_time
  GO
  ```  

  Cet exemple lit tous les rapports d’audit à partir des serveurs qui commencent par `Sh`: 
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/Sh',default,default);
  GO  
  ```

Pour obtenir un exemple complet de création d’audit, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).

Pour plus d’informations sur la configuration de l’audit de base de données SQL Azure, consultez [prise en main l’audit de base de données SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
