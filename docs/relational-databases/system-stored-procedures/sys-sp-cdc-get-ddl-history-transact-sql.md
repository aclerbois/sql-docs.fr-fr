---
title: Sys.sp_cdc_get_ddl_history (Transact-SQL) | Documents Microsoft
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
- sp_cdc_get_ddl_history
- sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sp_cdc_get_ddl_history
- sys.sp_cdc_get_ddl_history
ms.assetid: 4dee5e2e-d7e5-4fea-8037-a4c05c969b3a
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2cafbdcab4ad480493181cc7b2f6b160e34ea681
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdcgetddlhistory-transact-sql"></a>sys.sp_cdc_get_ddl_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne l'historique des modifications de langage de définition de données (DDL) associé à l'instance de capture spécifiée depuis que la capture de données modifiées a été activée pour cette instance de capture. La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_get_ddl_history [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @capture_instance =] '*capture_instance*'  
 Nom de l'instance de capture associée à une table source. *capture_instance* est **sysname** et ne peut pas être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nom du schéma de table source.|  
|source_table|**sysname**|Nom de la table source.|  
|capture_instance|**sysname**|Nom de l'instance de capture.|  
|required_column_update|**bit**|Indique que la modification DDL a nécessité la modification d'une colonne dans la table de modifications pour refléter une modification de type de données effectuée sur la colonne source.|  
|ddl_command|**nvarchar(max)**|Instruction DDL appliquée à la table source.|  
|ddl_lsn|**binary(10)**|Numéro séquentiel dans le journal associé à la modification DDL.|  
|ddl_time|**datetime**|Heure associée à la modification DDL.|  
  
## <a name="remarks"></a>Notes  
 Les modifications DDL pour la table source qui modifient la structure de colonne de table source, telles que l’ajout ou la suppression d’une colonne ou la modification du type de données d’une colonne existante, sont conservées dans le [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) table. Ces modifications peuvent être signalées en utilisant cette procédure stockée. Les entrées dans cdc.ddl_history sont effectuées au moment où le processus de capture lit la transaction DDL dans le journal.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe db_owner pour retourner les lignes de toutes les instances de capture dans la base de données. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute une colonne à la table source `HumanResources.Employee`, puis exécute la procédure stockée `sys.sp_cdc_get_ddl_history` pour signaler les modifications DDL qui s'appliquent à la table source associée à l'instance de capture `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
ALTER TABLE HumanResources.Employee  
ADD Test_Column int NULL;  
GO  
-- Pause 10 seconds to allow the event to be logged.   
WAITFOR DELAY '00:00:10';  
GO   
EXECUTE sys.sp_cdc_get_ddl_history   
    @capture_instance = 'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
