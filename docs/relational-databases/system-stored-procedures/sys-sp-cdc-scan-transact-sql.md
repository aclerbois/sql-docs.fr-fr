---
title: Sys.sp_cdc_scan (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sp_cdc_scan_TSQL
- sp_cdc_scan
- sys.sp_cdc_scan
- sp_cdc_scan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_scan
ms.assetid: 46e4294c-97b8-47d6-9ed9-b436a9929353
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5a295451cc5894d55d2e61ed941fb46af93d53c1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdcscan-transact-sql"></a>sys.sp_cdc_scan (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Exécute l'opération d'analyse du journal de la capture de données modifiées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_scan [ [ @maxtrans = ] max_trans ]   
     [ , [ @maxscans = ] max_scans ]   
     [ , [ @continuous = ] continuous ]   
     [ , [ @pollinginterval = ] polling_interval ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@maxtrans=** ] *max_trans*  
 Nombre maximal de transactions à traiter dans chaque cycle d'analyse. *max_trans* est **int** avec la valeur par défaut est 500.  
  
 [  **@maxscans=** ] *max_scans*  
 Nombre maximal de cycles d'analyse à exécuter afin d'extraire toutes les lignes du journal. *max_scans* est **int** avec la valeur par défaut est 10.  
  
 [  **@continuous=** ] *continue*  
 Indique si la procédure stockée doit se terminer après l’exécution d’un cycle d’analyse unique (0) ou s’exécuter en continu, en pendant la durée spécifiée par *polling_interval* avant de réexécuter le cycle d’analyse (1). *continue* est **tinyint** avec une valeur par défaut 0.  
  
 [  **@pollinginterval=** ] *polling_interval*  
 Nombre de secondes entre les cycles d’analyse de journal. *polling_interval* est **bigint** avec une valeur par défaut 0.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 sys.sp_cdc_scan est appelé en interne par sys.sp_MScdc_capture_job si le travail de capture de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé par la capture de données modifiées. La procédure ne peut pas être exécutée de manière explicite lorsqu'une opération d'analyse du journal de capture de données modifiées est déjà active ou lorsque la base de données est activée pour la réplication transactionnelle. Cette procédure stockée doit être utilisée par les administrateurs qui souhaitent personnaliser le comportement du travail de capture configuré automatiquement.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
  
  
