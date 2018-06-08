---
title: Core.sp_create_snapshot (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_create_snapshot
- sp_create_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- management data warehouse, data collector stored procedures
- data collector [SQL Server], stored procedures
- core.sp_create_snapshot stored procedure
- sp_create_snapshot
ms.assetid: ff297bda-0ee2-4fda-91c8-7000377775e3
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 33ba9d69763a9d07cc9907aef60397b6c5b37eee
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33238566"
---
# <a name="corespcreatesnapshot-transact-sql"></a>core.sp_create_snapshot (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Insère une ligne dans la vue core.snapshots de l'entrepôt de données de gestion. Cette procédure est appelée chaque fois qu'un package de téléchargement commence à télécharger des données dans l'entrepôt de données de gestion.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
core.sp_create_snapshot [ @collection_set_uid = ] 'collection_set_uid'  
    , [ @collector_type_uid = ] 'collector_type_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @log_id = ] log_id  
    , [ @snapshot_id = ] snapshot_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_set_uid =] '*collection_set_uid*'  
 GUID du jeu d'éléments de collecte. *collection_set_uid* est **uniqueidentifier** sans valeur par défaut. Pour obtenir le GUID, interrogez la vue dbo.syscollector_collection_sets dans la base de données msdb.  
  
 [ @collector_type_uid =] '*collector_type_uid*'  
 GUID d'un type de collecteur. *collector_type_uid* est **uniqueidentifier** sans valeur par défaut. Pour obtenir le GUID, interrogez la vue dbo.syscollector_collector_types dans la base de données msdb.  
  
 [ @machine_name=] '*Nom_Ordinateur*'  
 Nom du serveur sur lequel réside le jeu d'éléments de collecte. *Nom_Ordinateur* est **sysname**, sans valeur par défaut.  
  
 [ @named_instance=] '*instance_nommée*'  
 Nom de l'instance pour le jeu d'éléments de collecte. *instance_nommée* est **sysname**, sans valeur par défaut.  
  
 [ @log_id =] *log_id*  
 Identificateur unique mappé au journal des événements de jeu d'éléments de collecte sur le serveur qui a collecté les données. *log_id* est **bigint** sans valeur par défaut. Pour obtenir la valeur de *log_id*, interrogez la vue dbo.syscollector_execution_log dans la base de données msdb.  
  
 [ @snapshot_id = ] *snapshot_id*  
 Identificateur unique pour une ligne est insérée dans la vue core.snapshots. *snapshot_id* est **int** et est retourné en tant que sortie.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Chaque fois qu'un package de téléchargement commence à télécharger des données dans l'entrepôt de données de gestion, le composant runtime du collecteur de données appelle core.sp_create_snapshot.  
  
 Cette procédure vérifie les points suivants :  
  
-   collection_set_uid correspond à une entrée existante dans la table core.source_info_internal.  
  
-   collector_type_uid correspond à une entrée existante dans la vue core.supported_collector_types.  
  
 Si l'une des vérifications précédentes échoue, la procédure échoue et retourne une erreur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **mdw_writer** (avec autorisation EXECUTE) rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un instantané pour le jeu d'éléments de collecte Utilisation du disque, l'ajoute à l'entrepôt de données de gestion, puis retourne l'identificateur de l'instantané. Dans l'exemple, l'instance par défaut est utilisée.  
  
```  
USE <management_data_warehouse>;  
DECLARE @snapshot_id int;  
EXEC core.sp_create_snapshot   
    @collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
    @collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @machine_name = '<computername>',  
    @named_instance = 'MSSQLSERVER',  
    @log_id = 11, -- ID of the log for the collection set  
    @snapshot_id = @snapshot_id OUTPUT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Entrepôt de données de gestion](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
