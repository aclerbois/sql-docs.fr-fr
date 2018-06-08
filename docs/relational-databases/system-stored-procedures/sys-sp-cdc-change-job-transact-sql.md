---
title: Sys.sp_cdc_change_job (Transact-SQL) | Documents Microsoft
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
- sys.sp_cdc_change_job_TSQL
- sys.sp_cdc_change_job
- sp_cdc_change_job_TSQL
- sp_cdc_change_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_change_job
ms.assetid: ea918888-0fc5-4cc1-b301-26b2a9fbb20d
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ca1b4bf87be7b0b102bb139aad650dfa16e5c3a1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdcchangejob-transact-sql"></a>sys.sp_cdc_change_job (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie la configuration d'un travail de capture ou de nettoyage de capture de données modifiées dans la base de données actuelle. Pour afficher la configuration actuelle d’un travail, interrogez la [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) table, ou utilisez [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_change_job [ [ @job_type = ] 'job_type' ]  
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ @threshold = ] 'delete threshold'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_type=** ] **'***type_du_travail***'**  
 Type de travail à modifier. *type_du_travail* est **nvarchar (20)** avec une valeur par défaut 'capture'. Les entrées valides sont 'capture' et 'cleanup'.  
  
 [ **@maxtrans** ] **= *** max_trans*  
 Nombre maximal de transactions à traiter dans chaque cycle d'analyse. *max_trans* est **int** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre. Si elle est spécifiée, la valeur doit être un entier positif.  
  
 *max_trans* est valide uniquement pour les travaux de capture.  
  
 [ **@maxscans** ] **= *** max_scans*  
 Nombre maximal de cycles d'analyse à exécuter afin d'extraire toutes les lignes du journal. *max_scans* est **int** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre.  
  
 *max_scan* est valide uniquement pour les travaux de capture.  
  
 [ **@continuous** ] **= *** continue*  
 Indique si le travail de capture doit être exécuté en continu (1) ou une seule fois (0). *continue* est **bits** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre.  
  
 Lorsque *continue* = 1, le [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) tâche analyse le journal et traite jusqu'à (*max_trans* \* *max_scans*) des transactions. Il attend alors pendant le nombre de secondes spécifié dans *polling_interval* avant de commencer l’analyse de journal suivante.  
  
 Lorsque *continue* = 0, la **sp_cdc_scan** s’exécute jusqu'à *max_scans* analyses du journal, en traitant un maximum de *max_trans* transactions pendant chaque analyse, puis se ferme.  
  
 Si **@continuous** passe de 1 à 0, **@pollinginterval** est automatiquement définie sur 0. Une valeur spécifiée pour **@pollinginterval** autre que 0 est ignoré.  
  
 Si **@continuous** est omis ou définie explicitement sur NULL et **@pollinginterval** est explicitement définie sur une valeur supérieure à 0, **@continuous** est automatiquement défini sur 1.  
  
 *continue* est valide uniquement pour les travaux de capture.  
  
 [ **@pollinginterval** ] **= *** polling_interval*  
 Nombre de secondes entre les cycles d’analyse de journal. *polling_interval* est **bigint** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre.  
  
 *polling_interval* n’est valide uniquement pour la capture des travaux lorsque *continue* est définie sur 1.  
  
 [ **@retention** ] **= *** rétention*  
 Nombre de minutes pendant lesquelles les lignes modifiées doivent être conservées dans les tables de modifications. *rétention* est **bigint** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre. La valeur maximale est égale à 52494800 (100 ans). Si elle est spécifiée, la valeur doit être un entier positif.  
  
 *rétention* est valide uniquement pour les travaux de nettoyage.  
  
 [  **@threshold=** ] **'***supprimer seuil***'**  
 Nombre maximal d’entrées qui peuvent être supprimées à l’aide d’une instruction unique lors du nettoyage. *supprimer le seuil* est **bigint** avec NULL comme valeur par défaut, ce qui n’indique aucune modification pour ce paramètre. *supprimer le seuil* est valide uniquement pour les travaux de nettoyage.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Si un paramètre est omis, la valeur associée dans le [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) table n’est pas mis à jour. Un paramètre qui a pour valeur explicite NULL est traité comme s'il était omis.  
  
 La spécification d'un paramètre qui n'est pas valide pour le type de travail provoquera l'échec de l'instruction.  
  
 Les modifications apportées à un travail ne prennent pas effet jusqu'à ce que la tâche est arrêtée à l’aide de [sp_cdc_stop_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md) et redémarré à l’aide de [sp_cdc_start_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **db_owner** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-a-capture-job"></a>A. Modification d'un travail de capture  
 L’exemple suivant met à jour la `@job_type`, `@maxscans`, et `@maxtrans` paramètres d’un travail de capture dans le `AdventureWorks2012` base de données. Les autres paramètres valides pour un travail de capture, `@continuous` et `@pollinginterval`, sont omis ; leurs valeurs ne sont pas modifiées.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'capture',  
    @maxscans = 1000,  
    @maxtrans = 15;  
GO  
```  
  
### <a name="b-changing-a-cleanup-job"></a>B. Modification d'un travail de nettoyage  
 L'exemple suivant met à jour un travail de nettoyage dans la base de données `AdventureWorks2012`. Tous les paramètres valides pour ce type de travail, à l’exception de **@threshold**, sont spécifiés. La valeur de **@threshold** n’est pas modifié.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'cleanup',  
    @retention = 2880;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [Sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [Sys.sp_cdc_add_job &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
