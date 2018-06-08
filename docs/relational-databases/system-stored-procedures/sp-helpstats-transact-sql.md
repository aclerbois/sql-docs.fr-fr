---
title: sp_helpstats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_helpstats
- sp_helpstats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpstats
ms.assetid: 00ab3cfd-2736-4fc0-b1b2-16dd49fb2fe5
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: eda430dedf39538c27f85d0ea11ca59bbfdc5e20
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpstats-transact-sql"></a>sp_helpstats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les informations statistiques sur les colonnes et les index de la table spécifiée.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] Pour obtenir des informations sur les statistiques, interrogez la [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) et [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) affichages catalogue.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpstats[ @objname = ] 'object_name'   
     [ , [ @results = ] 'value' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@objname=**] **'***nom_objet***'**  
 Spécifie la table au sujet de laquelle les informations statistiques doivent être fournies. *nom_objet* est **nvarchar(520)** et ne peut pas être null. Vous pouvez spécifier un nom en une ou deux parties.  
  
 [  **@results=**] **'***valeur***'**  
 Désigne l'étendue des informations à fournir. Les entrées valides sont **tous les** et **statistiques**. **Tous les** répertorie les statistiques de tous les index et également les colonnes ayant des statistiques créées sur ces derniers ; **Statistiques** répertorie uniquement les statistiques non associées à un index. *valeur* est **nvarchar (5)** avec STATS comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le tableau suivant décrit les colonnes du jeu de résultats.  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**statistics_name**|Nom des statistiques. Retourne **sysname** et ne peut pas être null.|  
|**statistics_keys**|Clés sur lesquelles sont basées les statistiques. Retourne **nvarchar (2078)** et ne peut pas être null.|  
  
## <a name="remarks"></a>Notes  
 Utilisez DBCC SHOW_STATISTICS pour afficher des informations statistiques détaillées sur l'index ou les statistiques de votre choix. Pour plus d’informations, consultez [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41; ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) et [sp_helpindex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée des statistiques, réparties sur une seule colonne, pour toutes les colonnes possibles de toutes les tables utilisateur de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en exécutant `sp_createstats`. Ensuite, `sp_helpstats` est exécutée pour rechercher les statistiques résultantes créées sur la table `Customer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_createstats;  
GO  
EXEC sp_helpstats   
@objname = 'Sales.Customer',  
@results = 'ALL';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `statistics_name               statistics_keys`  
  
 `----------------------------  ----------------`  
  
 `_WA_Sys_00000003_22AA2996     AccountNumber`  
  
 `AK_Customer_AccountNumber     AccountNumber`  
  
 `AK_Customer_rowguid           rowguid`  
  
 `CustomerType                  CustomerType`  
  
 `IX_Customer_TerritoryID       TerritoryID`  
  
 `ModifiedDate                  ModifiedDate`  
  
 `PK_Customer_CustomerID        CustomerID`  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
