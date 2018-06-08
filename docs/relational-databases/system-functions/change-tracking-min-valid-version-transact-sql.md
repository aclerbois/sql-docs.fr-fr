---
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL) | Documents Microsoft
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
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
caps.latest.revision: 16
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7809bd218e1f7c8c81ee6c30a7ddcc514e2f085d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="changetrackingminvalidversion-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne la version minimale sur le client est valide pour une utilisation de l’obtention des informations de suivi à partir de la table spécifiée, lorsque vous utilisez la [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) (fonction).  
    
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *table_object_id*  
 Est l’ID d’objet de la table. *table_object_id* est un **int**.  
  
## <a name="return-type"></a>Type de retour  
 **bigint**  
  
## <a name="remarks"></a>Notes  
 Utilisez cette fonction pour valider la valeur de la *last_sync_version* paramètre CHANGETABLE. Si *last_sync_version* est inférieure à celle qui est signalée par cette fonction, les résultats sont retournés à partir d’un appel ultérieur à CHANGETABLE n’est peut-être pas valides.  
  
 CHANGE_TRACKING_MIN_VALID_VERSION utilise les informations suivantes pour déterminer la valeur de retour :  
  
-   Lorsque la table a été activée pour le suivi des modifications.  
  
-   Lorsque la tâche de nettoyage en arrière-plan s'est exécutée pour supprimer des informations de suivi des modifications antérieures à la période de rétention spécifiée pour la base de données.  
  
-   Si la table a été tronquée. Cette instruction supprime toutes les informations de suivi des modifications associées à la table.  
  
 La fonction retourne la valeur NULL si l'une des conditions suivantes est remplie :  
  
-   Le suivi des modifications n'est pas activé pour la base de données.  
  
-   L'ID d'objet de table spécifié n'est pas valide pour la base de données active.  
  
-   Autorisation insuffisante pour la table spécifiée par l'ID d'objet.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant détermine si la version spécifiée est valide. L'exemple obtient la version valide minimale de la table `dbo.Employees`, puis la compare à la valeur de la variable `@last_sync_version`. Si la valeur de `@last_sync_version` est inférieure à la valeur de `@min_valid_version`, la liste des lignes modifiées n'est pas valide.  
  
> [!NOTE]  
>  Généralement, vous pouvez obtenir la valeur à partir d'une table ou d'un autre emplacement dans lequel vous avez stocké le dernier numéro de version utilisé pour synchroniser les données.  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de suivi des modifications &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
