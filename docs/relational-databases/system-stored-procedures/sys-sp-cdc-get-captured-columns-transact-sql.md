---
title: Sys.sp_cdc_get_captured_columns (Transact-SQL) | Documents Microsoft
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
- sp_cdc_get_captured_columns
- sys.sp_cdc_get_captured_columns
- sys.sp_cdc_get_captured_columns_TSQL
- sp_cdc_get_captured_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_get_captured_columns
- sp_cdc_get_captured_columns
- change data capture [SQL Server], querying metadata
ms.assetid: d9e680be-ab9b-4e0c-b63a-90658f241df8
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b131dce10e231bc0ee479ff0157a614bcfb2d6f4
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="sysspcdcgetcapturedcolumns-transact-sql"></a>sys.sp_cdc_get_captured_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les métadonnées de capture de données modifiées pour les colonnes sources capturées suivies par l'instance de capture spécifiée. La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_get_captured_columns   
    [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @capture_instance =] '*capture_instance*'  
 Nom de l'instance de capture associée à une table source. *capture_instance* est **sysname** et ne peut pas être NULL.  
  
 Pour générer un rapport sur les instances de capture pour la table, exécutez le [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) procédure stockée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nom du schéma de table source.|  
|source_table|**sysname**|Nom de la table source.|  
|capture_instance|**sysname**|Nom de l'instance de capture.|  
|column_name|**sysname**|Nom de la colonne source capturée.|  
|column_id|**int**|ID de la colonne dans la table source.|  
|column_ordinal|**int**|Position de la colonne dans la table source.|  
|data_type|**sysname**|Type de données de la colonne.|  
|character_maximum_length|**int**|Longueur maximale en caractères de la colonne basée sur les caractères ; sinon, NULL.|  
|numeric_precision|**tinyint**|Précision de la colonne si elle est numérique ; sinon, NULL.|  
|numeric_precision_radix|**smallint**|Base de précision de la colonne si elle est numérique ; sinon, NULL.|  
|numeric_scale|**int**|Échelle de la colonne si elle est numérique ; sinon, NULL.|  
|datetime_precision|**smallint**|Précision de la colonne si elle est basée sur datetime ; sinon, NULL.|  
  
## <a name="remarks"></a>Notes  
 Utilisez sys.sp_cdc_get_captured_columns pour obtenir des informations de colonne sur les colonnes capturées retournées en interrogeant les fonctions de requête de capture instance [cdc.fn_cdc_get_all_changes_ < instance_capture >](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) ou [cdc.fn_cdc_get_net_changes_ < instance_capture >](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md). Les noms de colonnes, ID et position restent constants pendant la vie de l'instance de capture. Seul le type de données de colonne change lorsque le type de données de la colonne source sous-jacente dans la table faisant l'objet d'un suivi change. Les colonnes qui sont ajoutés ou supprimés d’une table source n’ont aucun impact sur les colonnes capturées des instances de capture existantes.  
  
 Utilisez [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md) pour obtenir plus d’informations sur la définition de données (instructions DDL language) appliquées à une table source. Toute modification DDL qui a modifié la structure d'une colonne source faisant l'objet d'un suivi est retournée dans le jeu de résultats.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données. Lorsque l'appelant n'a pas l'autorisation de consulter les données sources, la fonction retourne l'erreur 22981 (L'objet n'existe pas ou l'accès est refusé.).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les colonnes capturées de l'instance de capture `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_get_captured_columns   
    @capture_instance = N'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
