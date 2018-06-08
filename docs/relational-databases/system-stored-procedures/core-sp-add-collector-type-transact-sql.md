---
title: Core.sp_add_collector_type (Transact-SQL) | Documents Microsoft
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
- sp_add_collector_type
- sp_add_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- core.sp_add_collector_type stored procedure
- management data warehouse, data collector stored procedures
- sp_add_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 1d981037-2147-464e-a456-7d8e479bce89
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 486a51f5f521c0a3af647bc4d7242f3696727789
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="corespaddcollectortype-transact-sql"></a>core.sp_add_collector_type (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute une nouvelle entrée à la vue core.supported_collector_types de la base de données de l'entrepôt de données de gestion. La procédure doit être exécutée dans le contexte de la base de données d'entrepôt de données de gestion.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
core.sp_add_collector_type [ @collector_type_uid = ] 'collector_type_uid'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collector_type_uid =] '*collector_type_uid*'  
 GUID pour le type de collecteur. *collector_type_uid* est **uniqueidentifier**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **mdw_admin** (avec autorisation EXECUTE) rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute le type de collecteur Requête T-SQL générique à la vue core.supported_collector_types. Par défaut, le type de collecteur Requête T-SQL générique existe déjà. Par conséquent, si vous exécutez ce code sur une installation par défaut, vous recevrez un message indiquant que le type de collecteur existe déjà.  
  
 Ce code s'exécuterait correctement si vous aviez supprimé le type de collecteur Requête T-SQL générique à l'aide de la procédure stockée core.sp_remove_collector_type et si vous aviez voulu ensuite l'ajouter à nouveau en tant que type de collecteur inscrit capable de télécharger des données dans l'entrepôt de données de gestion.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @RC int;  
DECLARE @collector_type_uid uniqueidentifier;  
SELECT @collector_type_uid = (SELECT collector_type_uid FROM msdb.dbo.syscollector_collector_types WHERE name = N'Generic T-SQL Query Collector Type');  
EXECUTE @RC = core.sp_add_collector_type @collector_type_uid;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Entrepôt de données de gestion](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
