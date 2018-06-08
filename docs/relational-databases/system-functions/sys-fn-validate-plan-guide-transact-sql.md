---
title: Sys.fn_validate_plan_guide (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.fn_validate_plan_guide
- sys.fn_validate_plan_guide_TSQL
- fn_validate_plan_guide
- fn_validate_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_validate_plan_guide function
- sys.fn_validate_plan_guide function
ms.assetid: 3af8b47a-936d-4411-91d1-d2d16dda5623
caps.latest.revision: 19
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 6b941fabfd4ebbd3ca41375622bf682f12b0fc26
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnvalidateplanguide-transact-sql"></a>sys.fn_validate_plan_guide (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Vérifie la validité du repère de plan spécifié. La fonction sys.fn_validate_plan_guide retourne le premier message d'erreur rencontré lorsque le repère de plan est appliqué à sa requête. Un ensemble de lignes vide est retourné lorsque le repère de plan est valide. Les repères de plan peuvent devenir non valides lorsque des modifications sont apportées à la conception physique de la base de données. Par exemple, si un repère de plan spécifie un index particulier et que cet index est ensuite supprimé, la requête ne peut plus utiliser ce repère de plan.  
  
 En validant un repère de plan, vous pouvez déterminer si le repère peut être utilisé par l'optimiseur sans modification. Selon les résultats de la fonction, vous pouvez décider de supprimer le repère de plan et paramétrer à nouveau la requête, ou de modifier la conception de base de données, par exemple, en recréant l'index spécifié dans le repère de plan.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.fn_validate_plan_guide ( plan_guide_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *plan_guide_id*  
 Est l’ID du repère de plan tel qu’indiqué dans le [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) affichage catalogue. *plan_guide_id* est **int** sans valeur par défaut.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|msgnum|**int**|ID du message d'erreur.|  
|severity|**tinyint**|Niveau de gravité du message, entre 1 et 25.|  
|state|**smallint**|Numéro d'état de l'erreur indiquant le point dans le code au niveau duquel l'erreur s'est produite.|  
|message|**nvarchar(2048)**|Texte du message de l'erreur.|  
  
## <a name="permissions"></a>Autorisations  
 Les repères de plan de portée OBJECT requièrent une autorisation VIEW DEFINITION ou ALTER sur l'objet et les autorisations référencés pour compiler la requête ou le lot fourni dans le repère de plan. Par exemple, si un lot contient des instructions SELECT, des autorisations SELECT sont requises sur les objets référencés.  
  
 Les repères de plan de portée SQL ou TEMPLATE requièrent une autorisation ALTER sur la base de données et les autorisations pour compiler la requête ou le lot fourni dans le repère de plan. Par exemple, si un lot contient des instructions SELECT, des autorisations SELECT sont requises sur les objets référencés.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-validating-all-plan-guides-in-a-database"></a>A. Validation de tous les repères de plan dans une base de données  
 L'exemple suivant vérifie la validité de tous les repères de plan dans la base de données actuelle. Si un jeu de résultats vide est retourné, tous les repères de plan sont valides.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT plan_guide_id, msgnum, severity, state, message  
FROM sys.plan_guides  
CROSS APPLY fn_validate_plan_guide(plan_guide_id);  
GO  
```  
  
### <a name="b-testing-plan-guide-validation-before-implementing-a-change-to-the-database"></a>B. Test de la validation des repères de plan avant d'implémenter une modification de la base de données  
 L'exemple suivant utilise une transaction explicite pour supprimer un index. Le `sys.fn_validate_plan_guide` fonction est exécutée pour déterminer si cette action invalidera des repères de plan dans la base de données. En fonction des résultats de la fonction, l'instruction `DROP INDEX` est validée ou la transaction est restaurée, et l'index n'est pas supprimé.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DROP INDEX IX_SalesOrderHeader_CustomerID ON Sales.SalesOrderHeader;  
-- Check for invalid plan guides.  
IF EXISTS (SELECT plan_guide_id, msgnum, severity, state, message  
           FROM sys.plan_guides  
           CROSS APPLY sys.fn_validate_plan_guide(plan_guide_id))  
    ROLLBACK TRANSACTION;  
ELSE  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Repères de plan](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
  
