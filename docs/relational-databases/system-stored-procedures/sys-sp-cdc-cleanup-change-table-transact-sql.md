---
title: Sys.sp_cdc_cleanup_change_table (Transact-SQL) | Documents Microsoft
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
- sp_cdc_cleanup_change_table
- sp_cdc_cleanup_change_table_TSQL
- sys.sp_cdc_cleanup_change_table
- sys.sp_cdc_cleanup_change_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_cleanup_change_tables
- sp_cdc_cleanup_change_tables
ms.assetid: 02295794-397d-4445-a3e3-971b25e7068d
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 0f0fc4f6a24143e23bb118a24f6061f5458eebe2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcdccleanupchangetable-transact-sql"></a>sys.sp_cdc_cleanup_change_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime des lignes à partir de la table de modifications dans la base de données actuelle selon le *low_water_mark* valeur. Cette procédure stockée est fournie aux utilisateurs qui souhaitent gérer le processus du nettoyage de la table de modifications directement. Toutefois, soyez vigilant, car la procédure affecte tous les consommateurs des données de la table de modifications.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_cleanup_change_table   
  [ @capture_instance = ] 'capture_instance',   
  [ @low_water_mark = ] low_water_mark ,  
  [ @threshold = ]'delete threshold'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @capture_instance =] '*capture_instance*'  
 Est le nom de l'instance de capture associée à la table de modifications. *capture_instance* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 *capture_instance* doit désigner une instance de capture qui existe dans la base de données actuelle.  
  
 [ @low_water_mark =] *low_water_mark*  
 Est un numéro de séquence de journal (LSN) qui doit être utilisé en tant que la nouvelle limite inférieure pour la *instance de capture*. *low_water_mark* est **Binary (10)**, sans valeur par défaut.  
  
 Si la valeur est non nulle, il doit apparaître en tant que la valeur start_lsn d’une entrée actuelle dans le [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) table. Si d'autres entrées dans cdc.lsn_time_mapping partagent la même heure de validation que l'entrée identifiée par la nouvelle borne inférieure, le plus petit numéro séquentiel dans le journal associé à ce groupe d'entrées est choisi comme borne inférieure.  
  
 Si la valeur est définie explicitement sur NULL, en cours *inférieure* pour le *instance de capture* est utilisé pour définir la limite supérieure pour l’opération de nettoyage.  
  
 [ @threshold=] '*supprimer seuil*'  
 Est le nombre maximal d'entrées qui peuvent être supprimées à l'aide d'une instruction unique lors du nettoyage. *delete_threshold* est **bigint**, avec la valeur par défaut est 5000.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 sys.sp_cdc_cleanup_change_table effectue les opérations suivantes :  
  
1.  Si le @low_water_mark paramètre n’est pas NULL, elle définit la valeur de start_lsn pour le *instance de capture* au nouveau *inférieure*.  
  
    > [!NOTE]  
    >  La nouvelle limite inférieure ne peut pas être la limite inférieure spécifiée dans l'appel de procédure stockée. Si d'autres entrées dans la table cdc.lsn_time_mapping partagent la même heure de validation, le plus petit start_lsn représenté dans le groupe d'entrées est sélectionné comme limite inférieure ajustée. Si le @low_water_mark paramètre est NULL ou la limite inférieure actuelle est supérieure à la nouvelle limite inférieure, la valeur start_lsn pour l’instance de capture reste inchangée.  
  
2.  Les entrées de table de modifications avec des valeurs __$start_lsn inférieures à la limite inférieure sont ensuite supprimées. Le seuil de suppression est utilisé pour limiter le nombre de lignes supprimées dans une transaction unique. Un échec de suppression des entrées est signalé, mais n'affecte pas les modifications de la limite inférieure d'instance de capture ayant pu être apportées selon l'appel.  
  
 Utilisez sys.sp_cdc_cleanup_change_table dans les circonstances suivantes :  
  
-   Le travail de l'agent de nettoyage signale des échecs de suppression.  
  
     Un administrateur peut exécuter explicitement cette procédure stockée pour réessayer d'effectuer une opération ayant échoué. Pour réessayer de nettoyage pour une instance de capture donnée, exécutez sys.sp_cdc_cleanup_change_table et spécifiez la valeur NULL pour le @low_water_mark paramètre.  
  
-   La stratégie simple basée sur la rétention utilisée par le travail de l'agent de nettoyage n'est pas adéquate.  
  
     Cette procédure stockée effectuant le nettoyage pour une instance de capture unique, elle peut être utilisée pour générer une stratégie de nettoyage personnalisée qui adapte les règles de nettoyage à l'instance de capture individuelle.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_get_min_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [sys.fn_cdc_increment_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)  
  
  
