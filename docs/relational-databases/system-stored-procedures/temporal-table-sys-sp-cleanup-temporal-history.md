---
title: Sys.sp_cleanup_temporal_history | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: ''
ms.prod_service: sql-database
ms.service: sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6eff30b4-b261-4f1f-b93c-1f69d754298d
caps.latest.revision: 4
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 6216ca6584c2bf6d78bb66096145cd49428398dc
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspcleanuptemporalhistory-transact-sql"></a>Sys.sp_cleanup_temporal_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

Supprime toutes les lignes de table d’historique temporelle qui correspondent au délai HISTORY_RETENTION configuré dans une transaction unique.
  
## <a name="syntax"></a>Syntaxe  
```  
sp_cleanup_temporal_history [@schema_name = ] schema_name, [@table_name = ] table_name [, [@row_count=] @row_count_var [OUTPUT]]
```  
  
## <a name="arguments"></a>Arguments  

*@table_name*

Le nom de la table temporelle de rétention qui est appelé nettoyage.

*schema_name*

Le nom du schéma de table temporelle actuelle appartient à

*row_count_var* [sortie]

Le paramètre de sortie qui retourne le nombre de lignes supprimées. Si la table d’historique a clustered index columnstore, ce paramètre retourne toujours 0.
  
## <a name="remarks"></a>Notes
Cette procédure stockée peut être utilisée uniquement avec les tables temporelles qu’ont fini période de rétention spécifiée.
Utilisez cette procédure stockée uniquement si vous devez immédiatement nettoyer toutes les lignes anciennes de la table d’historique. Vous devez savoir qu’il peut avoir un impact significatif sur le journal de la base de données et le sous-système d’e/s comme il supprime toutes les lignes éligibles dans la même transaction. 

Il est recommandé de toujours s’appuient sur une tâche en arrière-plan interne pour le nettoyage que supprime les anciennes lignes avec un impact minime sur les charges de travail normales et de la base de données en général.

## <a name="permissions"></a>Autorisations  
 Nécessite les autorisations db_owner.  

## <a name="example"></a>Exemple

```
declare @rowcnt int
EXEC sys.sp_cleanup_temporal_history 'dbo', 'Department', @rowcnt output
select @rowcnt
```

## <a name="see-also"></a>Voir aussi

[Stratégie de rétention des tables temporelles](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
