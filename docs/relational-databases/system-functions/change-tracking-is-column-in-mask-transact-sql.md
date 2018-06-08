---
title: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_IS_COLUMN_IN_MASK_TSQL
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_IS_COLUMN_IN_MASK
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
ms.assetid: 649b370b-da54-4915-919d-1b597a39d505
caps.latest.revision: 15
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 00c55659ddc52fb5e6299b82be8102d526bd9e43
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="changetrackingiscolumninmask-transact-sql"></a>CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Interprète la valeur SYS_CHANGE_COLUMNS retournée par la fonction CHANGETABLE(CHANGES ...). Cela permet à une application de déterminer si la colonne spécifiée est incluse dans les valeurs retournées pour SYS_CHANGE_COLUMNS.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CHANGE_TRACKING_IS_COLUMN_IN_MASK ( column_id , change_columns )  
```  
  
## <a name="arguments"></a>Arguments  
 *column_id*  
 ID de la colonne en cours de vérification. La colonne ID peut être obtenu à l’aide de la [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) (fonction).  
  
 *change_columns*  
 Les données binaires de la colonne SYS_CHANGE_COLUMNS de la [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) données.  
  
## <a name="return-type"></a>Type de retour  
 **bit**  
  
## <a name="return-values"></a>Valeurs de retour  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK retourne les valeurs suivantes.  
  
|Valeur retournée| Description|  
|------------------|-----------------|  
|0|La colonne spécifiée n’est pas dans le *change_columns* liste.|  
|1|La colonne spécifiée est dans le *change_columns* liste.|  
  
## <a name="remarks"></a>Notes  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK n’effectue pas de contrôle pour valider la *column_id* valeur ou qui le *change_columns* paramètre a été obtenu à partir de la table à partir de laquelle le *column_id* a été obtenu.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant détermine si la colonne `Salary` de la table `Employees` a été mise à jour. Le `COLUMNPROPERTY` fonction retourne l’ID de colonne de la `Salary` colonne. La variable locale `@change_columns` doit être définie en fonction des résultats d'une requête en utilisant CHANGETABLE comme source de données.  
  
```sql  
SET @SalaryChanged = CHANGE_TRACKING_IS_COLUMN_IN_MASK  
    (COLUMNPROPERTY(OBJECT_ID('Employees'), 'Salary', 'ColumnId')  
    ,@change_columns);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de suivi des modifications &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
