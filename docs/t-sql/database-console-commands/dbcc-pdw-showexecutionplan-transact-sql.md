---
title: DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
caps.latest.revision: 12
author: edmacauley
ms.author: umajay
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: e6b9036cc68c187471f3531aafdc787fbabc26e2
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="dbcc-pdwshowexecutionplan-transact-sql"></a>DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

Affiche le plan d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une requête exécutée sur un nœud de contrôle ou un nœud de calcul [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] spécifique. Cela permet de résoudre les problèmes de performances des requêtes pendant leur exécution sur les nœuds de calcul et le nœud de contrôle.
  
Une fois les problèmes de performances des requêtes compris pour les requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP exécutées sur les nœuds de calcul, il existe plusieurs façons d’améliorer les performances. Les méthodes possibles pour améliorer les performances des requêtes sur les nœuds de calcul incluent la création de statistiques multicolonnes, la création d’index non cluster ou l’utilisation d’indicateurs de requête.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
Syntaxe de SQL Server :

```sql
DBCC PDW_SHOWEXECUTIONPLAN ( distribution_id, spid )  
[;]  
```  
Syntaxe Azure Parallel Data Warehouse :
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( pdw_node_id, spid )  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *distribution_id*  
 Identificateur de la distribution qui exécute le plan de requête. Il s’agit d’un entier différent de Null. Utilisé quand la cible est [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
 *pdw_node_id*  
 Identificateur du nœud qui exécute le plan de requête. Il s’agit d’un entier différent de Null. Utilisé quand la cible est une appliance.  
  
 *spid*  
 Identificateur de la session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécute le plan de requête. Il s’agit d’un entier différent de Null.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation CONTROL sur [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
Nécessite l’autorisation VIEW-SERVER-STATE sur l’appliance.
  
## <a name="examples-includesssdwincludessssdw-mdmd"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]  
  
### <a name="a-dbcc-pdwshowexecutionplan-basic-syntax"></a>A. Syntaxe de base DBCC PDW_SHOWEXECUTIONPLAN  
 Lors de l’exécution sur une instance de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], modifiez la requête ci-dessus pour sélectionner aussi l’argument distribution_id.  
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status], [distribution_id]  
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
order by request_id, [dms_step_index];  
```  
  
Vous retournez ainsi l’identificateur spid pour chaque distribution activement exécutée. Si vous aviez la curiosité de savoir ce que la distribution 1 exécutait dans la session 375, vous exécuteriez la commande suivante.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 1, 375 );  
```  

## <a name="examples-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="b-dbcc-pdwshowexecutionplan-basic-syntax"></a>B. Syntaxe de base DBCC PDW_SHOWEXECUTIONPLAN  
 La requête dont l’exécution est trop longue exécute une opération de plan de requête DMS ou SQL.  
  
Si la requête exécute une opération de plan de requête DMS, vous pouvez utiliser la requête suivante pour récupérer une liste des ID de nœud et ID de session pour les étapes qui ne sont pas terminées.
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status]   
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
AND pdw_node_id = 201001   
order by request_id, [dms_step_index], [distribution_id];  
```  
  
Selon les résultats de la requête précédente, utilisez sql_spid et pdw_node_id en tant que paramètres pour DBCC PDW_SHOWEXECUTIONPLAN. Par exemple, la commande suivante indique le plan d’exécution pour pdw_node_id 201001 et sql_spid 375.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 201001, 375 );  
```  

## <a name="see-also"></a>Voir aussi
[DBCC PDW_SHOWPARTITIONSTATS &#40;Transact-SQL&#41;](dbcc-pdw-showpartitionstats-transact-sql.md)  
[DBCC PDW_SHOWSPACEUSED &#40;Transact-SQL&#41;](dbcc-pdw-showspaceused-transact-sql.md)
