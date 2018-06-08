---
title: Sys.sp_cdc_enable_table (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sp_cdc_enable_table_TSQL
- sp_cdc_enable_table_TSQL
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
ms.assetid: 26150c09-2dca-46ad-bb01-3cb3165bcc5d
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 0ed70b21e667a1738433335e3e3c869c3b410523
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdcenabletable-transact-sql"></a>sys.sp_cdc_enable_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Active la capture de données modifiées pour la table source spécifiée dans la base de données actuelle. Lorsqu'une table est activée pour la capture de données modifiées, un enregistrement de chaque opération DML (Data Manipulation Language) appliquée à la table est écrit dans le journal des transactions. Le processus de capture de données modifiées extrait ces informations du journal et les écrit dans les tables de modifications accédées à l'aide d'un ensemble de fonctions.  
  
 La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_enable_table   
  [ @source_schema = ] 'source_schema',   
  [ @source_name = ] 'source_name' ,  [,[ @capture_instance = ] 'capture_instance' ]  
  [,[ @supports_net_changes = ] supports_net_changes ]  
  , [ @role_name = ] 'role_name'  
  [,[ @index_name = ] 'index_name' ]  
  [,[ @captured_column_list = ] 'captured_column_list' ]  
  [,[ @filegroup_name = ] 'filegroup_name' ]  
  [,[ @allow_partition_switch = ] 'allow_partition_switch' ]  
  [;]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@source_schema =** ] **'***source_schema***'**  
 Est le nom du schéma auquel appartient la table source. *source_schema* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@source_name =** ] **'***source_name***'**  
 Nom de la table source sur laquelle activer la capture de données modifiées. *source_name* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 *source_name* doit exister dans la base de données actuelle. Les tables dans le **cdc** le schéma ne peut pas être activé pour la capture de données modifiées.  
  
 [  **@role_name =** ] **'***nom_rôle***'**  
 Nom du rôle de base de données utilisé pour réguler l'accès aux données modifiées. *nom_rôle* est **sysname** et doit être spécifié. En cas de définition explicitement sur NULL, aucun rôle de régulation n'est utilisé pour limiter l'accès aux données de modifications.  
  
 Si le rôle existe actuellement, il est utilisé. Si le rôle n'existe pas, une tentative est faite pour créer un rôle de base de données avec le nom spécifié. L'espace blanc est retiré à la droite de la chaîne dans le nom de rôle avant d'essayer de créer le rôle. Si l'appelant n'est pas autorisé à créer un rôle dans la base de données, la procédure stockée échoue.  
  
 [  **@capture_instance =** ] **'***capture_instance***'**  
 Nom de l'instance de capture utilisée pour nommer les objets de capture de données modifiées spécifiques à l'instance. *capture_instance* est **sysname** et ne peut pas être NULL.  
  
 Si non spécifié, le nom est dérivé du nom de schéma ainsi que le nom de la table source au format *schemaname_sourcename*. *capture_instance* ne peut pas dépasser 100 caractères et doit être unique au sein de la base de données. Si spécifié ou dérivé, *capture_instance* n’importe quel espace blanc situé à droite de la chaîne est retiré.  
  
 Une table source peut avoir un maximum de deux instances de capture. Pour plus d’informations, consultez [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md).  
  
 [  **@supports_net_changes =** ] *supports_net_changes*  
 Indique si la prise en charge de l'interrogation des modifications nettes doit être activée pour cette instance de capture. *supports_net_changes* est **bits** avec une valeur par défaut 1 si la table possède une clé primaire ou la table possède un index unique identifié à l’aide de le @index_name paramètre. Sinon, le paramètre a comme valeur par défaut 0.  
  
 S'il a la valeur 0, seules les fonctions de prise en charge d'interrogation de toutes les modifications sont générées.  
  
 S'il a la valeur 1, les fonctions nécessaires pour interroger les modifications nettes sont générées également.  
  
 Si *supports_net_changes* est définie sur 1, *index_name* doit être spécifié, ou la table source doit avoir une clé primaire définie.  
  
 [  **@index_name =** ] **' *** index_name*'  
 Nom d'un index unique utilisé pour identifier de manière unique les lignes dans la table source. *index_name* est **sysname** et peut être NULL. Si spécifié, *index_name* doit être un index unique valid sur la table source. Si *index_name* est spécifié, les colonnes d’index identifiées prévaut sur toute colonne de clé primaire définie comme identificateur de ligne unique pour la table.  
  
 [  **@captured_column_list =** ] **'***captured_column_list***'**  
 Identifie les colonnes de la table source qui doivent être incluses dans la table de modifications. *captured_column_list* est **nvarchar (max)** et peut être NULL. Si la valeur est NULL, toutes les colonnes sont incluses dans la table de modifications.  
  
 Les noms de colonnes doivent être des colonnes valides dans la table source. Les colonnes définies dans un index de clé primaire ou les colonnes définies dans un index référencé par *index_name* doit être inclus.  
  
 *captured_column_list* est une liste séparée par des virgules de noms de colonnes. Des noms de colonnes individuels dans la liste peuvent être cités en utilisant des guillemets doubles ("") ou des crochets ([]). Si un nom de colonne contient une virgule incorporée, il doit être entouré de guillemets.  
  
 *captured_column_list* ne peut pas contenir les noms de colonnes réservés suivants : **__ $start_lsn**, **__ $end_lsn**, **__ $seqval**, **__ $ opération**, et **__ $update_mask**.  
  
 [  **@filegroup_name =** ] **'***filegroup_name***'**  
 Groupe de fichiers à utiliser pour la table de modifications créée pour l'instance de capture. *FILEGROUP_NAME* est **sysname** et peut être NULL. Si spécifié, *filegroup_name* doit être définie pour la base de données actuelle. Si la valeur est NULL, le groupe de fichiers par défaut est utilisé.  
  
 Nous recommandons de créer un groupe de fichiers séparé pour les tables de modifications de capture des données modifiées.  
  
 [  **@allow_partition_switch=** ] **'***allow_partition_switch***'**  
 Indique si la commande SWITCH PARTITION d'ALTER TABLE peut être exécutée sur une table activée pour la capture de données modifiées. *allow_partition_switch* est **bits**, avec 1 comme valeur par défaut.  
  
 Pour les tables non partitionnées, le paramètre de commutation est toujours 1 et le paramètre réel est ignoré. Si la commutation est définie explicitement sur 0 pour une table non partitionnée, un avertissement 22857 est émis pour indiquer que le paramètre de commutation a été ignoré. Si la commutation est définie explicitement sur 0 pour une table partitionnée, un avertissement 22356 est émis pour indiquer que les opérations de commutation de partition sur la table source seront rejetées. Pour finir, si le paramètre de commutation est défini explicitement sur 1 ou autorisé à avoir comme valeur par défaut 1 et que la table active est partitionnée, un avertissement 22855 est émis pour indiquer que les commutations de partition ne seront pas bloquées. Si une commutation de partition se produit, la capture de données modifiées n'effectue pas le suivi des modifications qui résultent de la commutation. Cela provoquera des incohérences de données lors de la consommation des données de modifications.  
  
> [!IMPORTANT]  
>  SWITCH PARTITION est une opération de métadonnées, mais elle entraîne des modifications de données. Les modifications de données associées à cette opération ne sont pas capturées dans les tables de modifications de capture de données modifiées. Pensez à une table qui comprend trois partitions et à laquelle des modifications sont apportées. Le processus de capture effectuera le suivi des insertions, mises à jour et suppressions exécutées par l'utilisateur sur cette table. Toutefois, si une partition est extraite dans une autre table (par exemple, pour effectuer une suppression en bloc), les lignes déplacées dans le cadre de cette opération ne seront pas capturées en tant que lignes supprimées dans la table de modifications. De la même façon, si une nouvelle partition qui comprend des lignes préremplies est ajoutée à la table, ces lignes ne se seront pas répercutées dans la table de modifications. Cette situation peut aboutir à des données incohérentes lorsque les modifications sont utilisées par une application et appliquées à une destination.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Pour pouvoir activer une table pour la capture de données modifiées, la base de données doit être activée. Pour déterminer si la base de données est activée pour la capture de données modifiées, interrogez la **is_cdc_enabled** colonne dans la [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) vue de catalogue. Pour activer la base de données, utilisez la [sys.sp_cdc_enable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) procédure stockée.  
  
 Lorsque la capture de données modifiées est activée pour une table, une table de modifications et une ou deux fonctions de requêtes sont générées. La table de modifications sert de base de données de référentiel pour les modifications de table source extraites du journal des transactions par le processus de capture. Les fonctions de requête sont utilisées pour extraire des données de la table de modifications. Les noms de ces fonctions sont dérivés de la *capture_instance* paramètre comme suit :  
  
-   Fonction de toutes les modifications : **cdc.fn_cdc_get_all_changes_ < instance_capture >**  
  
-   Fonction des modifications nettes : **cdc.fn_cdc_get_net_changes_ < instance_capture >**  
  
 **Sys.sp_cdc_enable_table** crée également les tâches de capture et de nettoyage pour la base de données si la table source est la première table dans la base de données doit être activée pour la capture de données modifiées et qu’aucune publication transactionnelle n’existe pour la base de données. Il définit les **is_tracked_by_cdc** colonne dans la [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) affichage 1 catalogue.  
  
> [!NOTE]  
>  Il n'est pas nécessaire que l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit en cours d'exécution lorsque la capture de données modifiées est activée pour une table. Toutefois, le processus de capture ne traitera pas le journal des transactions et n'écrira pas d'entrées dans la table de modifications si l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne s'exécute pas.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **db_owner** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-change-data-capture-by-specifying-only-required-parameters"></a>A. Activation de la capture des données modifiées en spécifiant uniquement les paramètres requis  
 L'exemple suivant active la capture des données modifiées pour la table `HumanResources.Employee`. Seuls les paramètres requis sont spécifiés.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Employee'  
  , @role_name = N'cdc_Admin';  
GO  
```  
  
### <a name="b-enabling-change-data-capture-by-specifying-additional-optional-parameters"></a>B. Activation de la capture de données modifiées en spécifiant des paramètres facultatifs supplémentaires  
 L'exemple suivant active la capture des données modifiées pour la table `HumanResources.Department`. Tous les paramètres sauf `@allow_partition_switch` sont spécifiés.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Department'  
  , @role_name = N'cdc_admin'  
  , @capture_instance = N'HR_Department'   
  , @supports_net_changes = 1  
  , @index_name = N'AK_Department_Name'   
  , @captured_column_list = N'DepartmentID, Name, GroupName'   
  , @filegroup_name = N'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_disable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Sys.sp_cdc_help_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)  
  
  
