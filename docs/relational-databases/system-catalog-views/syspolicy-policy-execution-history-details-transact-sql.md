---
title: syspolicy_policy_execution_history_details (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- syspolicy_policy_execution_history_details
- syspolicy_policy_execution_history_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policy_execution_history_details view
ms.assetid: 97ef6573-5e8b-4ba5-8ae0-7901e79a9683
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1c3c5836dd2811e95db27392e9bd77cbe91f6e38
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syspolicypolicyexecutionhistorydetails-transact-sql"></a>syspolicy_policy_execution_history_details (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche les expressions de condition exécutées, les cibles des expressions, le résultat de chaque exécution et les détails des erreurs, le cas échéant. Le tableau suivant décrit les colonnes dans la vue syspolicy_execution_history_details.  
  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|detail_id|**bigint**|Identificateur de cet enregistrement. Chaque enregistrement représente la tentative pour évaluer ou appliquer une expression de condition dans une stratégie. Si elle est appliquée à plusieurs cibles, chaque condition contient un enregistrement de détail pour chaque cible.|  
|history_id|**bigint**|Identificateur de l'événement d'historique. Chaque événement d'historique représente une tentative d'exécution d'une stratégie. Comme une condition peut avoir plusieurs expressions de condition et plusieurs cibles, un history_id peut créer plusieurs enregistrements de détail. Utilisez la colonne history_id pour joindre cette vue à la [syspolicy_policy_execution_history](../../relational-databases/system-catalog-views/syspolicy-policy-execution-history-transact-sql.md) vue.|  
|target_query_expression|**nvarchar(max)**|Cible de la stratégie et vue syspolicy_policy_execution_history.|  
|execution_date|**datetime**|Date et heure de création de cet enregistrement de détail.|  
|result|**bit**|Succès ou échec de cette cible et évaluation d'expression de condition :<br /><br /> 0 (succès) ou 1 (échec).|  
|result_detail|**nvarchar(max)**|Message de résultat. Disponible uniquement s'il est fourni par la facette.|  
|exception_message|**nvarchar(max)**|Message généré par l'exception si celle-ci se produit.|  
|exception|**nvarchar(max)**|Description de l'exception si celle-ci se produit.|  
  
## <a name="remarks"></a>Notes  
 Lorsque vous dépannez la Gestion basée sur des stratégies, interrogez la vue syspolicy_policy_execution_history_details pour déterminer quelles combinaisons de cible et d'expression de condition ont échoué, quand elles ont échoué, et passez en revue les erreurs associées.  
  
 La requête suivante combine la vue `syspolicy_policy_execution_history_details` avec les vues `syspolicy_policy_execution_history_details` et `syspolicy_policies` pour afficher le nom de la stratégie, le nom de la condition et les détails se rapportant aux échecs.  
  
```  
SELECT Pol.name AS Policy,   
Cond.name AS Condition,   
PolHistDet.target_query_expression,   
PolHistDet.execution_date,   
PolHistDet.result,   
PolHistDet.result_detail,   
PolHistDet.exception_message,   
PolHistDet.exception   
FROM msdb.dbo.syspolicy_policies AS Pol  
JOIN msdb.dbo.syspolicy_conditions AS Cond  
    ON Pol.condition_id = Cond.condition_id  
JOIN msdb.dbo.syspolicy_policy_execution_history AS PolHist  
    ON Pol.policy_id = PolHist.policy_id  
JOIN msdb.dbo.syspolicy_policy_execution_history_details AS PolHistDet  
    ON PolHist.history_id = PolHistDet.history_id  
WHERE PolHistDet.result = 0 ;  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vues de la Gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
