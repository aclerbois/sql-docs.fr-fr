---
title: Sys.sp_cdc_add_job (Transact-SQL) | Documents Microsoft
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
- sp_cdc_add_job_TSQL
- sys.sp_cdc_add_job
- sys.sp_cdc_add_job_TSQL
- sp_cdc_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_add_job
ms.assetid: c4458738-ed25-40a6-8294-a26ca5a05bd9
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7c42d2150a148f1a288c1fa447f4bb1d9020fe09
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdcaddjob-transact-sql"></a>sys.sp_cdc_add_job (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un travail de nettoyage ou de capture de données modifiées dans la base de données active.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_add_job [ @job_type = ] 'job_type'  
    [ , [ @start_job = ] start_job ]   
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ , [ @threshold ] = 'delete_threshold' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_type=** ] **'***type_du_travail***'**  
 Type du travail à ajouter. *type_du_travail* est **nvarchar (20)** et ne peut pas être NULL. Les entrées valides sont **'capture'** et **'cleanup'**.  
  
 [  **@start_job=** ] *start_job*  
 Indicateur qui spécifie si le travail doit être démarré immédiatement après avoir été ajouté. *START_JOB* est **bits** avec 1 comme valeur par défaut.  
  
 [ **@maxtrans** ] = *max_trans*  
 Nombre maximal de transactions à traiter dans chaque cycle d'analyse. *max_trans* est **int** avec la valeur par défaut est 500. Si elle est spécifiée, la valeur doit être un entier positif.  
  
 *max_trans* est valide uniquement pour les travaux de capture.  
  
 [ **@maxscans** ] **= *** max_scans*  
 Nombre maximal de cycles d'analyse à exécuter afin d'extraire toutes les lignes du journal. *max_scans* est **int** avec la valeur par défaut est 10.  
  
 *max_scan* est valide uniquement pour les travaux de capture.  
  
 [ **@continuous** ] **= *** continue*  
 Indique si le travail de capture doit être exécuté en continu (1) ou une seule fois (0). *continue* est **bits** avec 1 comme valeur par défaut.  
  
 Lorsque *continue* = 1, le [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) tâche analyse le journal et traite jusqu'à (*max_trans* \* *max_scans*) des transactions. Il attend alors pendant le nombre de secondes spécifié dans *polling_interval* avant de commencer l’analyse de journal suivante.  
  
 Lorsque *continue* = 0, la **sp_cdc_scan** s’exécute jusqu'à *max_scans* analyses du journal, en traitant un maximum de *max_trans* transaction pendant chaque analyse, puis se ferme.  
  
 *continue* est valide uniquement pour les travaux de capture.  
  
 [ **@pollinginterval** ] **= *** polling_interval*  
 Nombre de secondes entre les cycles d’analyse de journal. *polling_interval* est **bigint** avec la valeur par défaut est 5.  
  
 *polling_interval* n’est valide uniquement pour la capture des travaux lorsque *continue* est définie sur 1. Si elle est spécifiée, la valeur ne peut pas être négative et ne peut pas dépasser 24 heures. Si une valeur de 0 est spécifiée, il n'y a aucune attente entre les analyses de journal.  
  
 [ **@retention** ] **= *** rétention*  
 Nombre de minutes pendant lesquelles les lignes de données modifiées doivent être conservées dans les tables de modifications. *rétention* est **bigint** avec 4320 (72 heures) par défaut. La valeur maximale est égale à 52494800 (100 ans). Si elle est spécifiée, la valeur doit être un entier positif.  
  
 *rétention* est valide uniquement pour les travaux de nettoyage.  
  
 [  **@threshold =** ] **'***delete_threshold***'**  
 Nombre maximal d’entrées qui peuvent être supprimées à l’aide d’une instruction unique lors du nettoyage. *delete_threshold* est **bigint** avec la valeur par défaut est 5000.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Un travail de nettoyage est créé en utilisant les valeurs par défaut lorsque la première table dans la base de données est activée pour la capture des données modifiées. Un travail de capture est créé en utilisant les valeurs par défaut lorsque la première table dans la base de données est activée pour la capture des données modifiées et qu'aucune publication transactionnelle n'existe pour la base de données. Lorsqu'une publication transactionnelle existe, le lecteur de journal transactionnel est utilisé pour conduire le mécanisme de capture et un travail de capture séparé n'est ni requis ni autorisé.  
  
 Dans la mesure où les travaux de capture et de nettoyage sont créés par défaut, cette procédure stockée est nécessaire uniquement lorsqu'un travail a été supprimé explicitement et doit être recréé.  
  
 Le nom de la tâche est **cdc. ***< nom_base_de_données >***_cleanup** ou **cdc. ***< nom_base_de_données >***_capture**, où *< nom_base_de_données >* est le nom de la base de données actuelle. Si un travail portant le même nom existe déjà, le nom est ajouté avec une période (**.**) suivi d’un identificateur unique, par exemple : **capture de données modifiées. AdventureWorks_capture. A1ACBDED-13FC-428C-8302-10100EF74F52**.  
  
 Pour afficher la configuration actuelle d’une tâche de nettoyage ou de capture, utilisez [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md). Pour modifier la configuration d’un travail, utilisez [sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **db_owner** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-capture-job"></a>A. Création d'un travail de capture  
 L'exemple suivant crée un travail de capture. Cet exemple suppose que le travail de nettoyage existant a été supprimé explicitement et doit être recréé. Le travail est créé en utilisant les valeurs par défaut.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job @job_type = N'capture';  
GO  
```  
  
### <a name="b-creating-a-cleanup-job"></a>B. Création d'un travail de nettoyage  
 L'exemple suivant crée un travail de nettoyage dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Le paramètre `@start_job` a la valeur 0 et `@retention` a pour valeur 5760 minutes (96 heures). Cet exemple suppose que le travail de nettoyage existant a été supprimé explicitement et doit être recréé.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job  
     @job_type = N'cleanup'  
    ,@start_job = 0  
    ,@retention = 5760;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [Sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
