---
title: sp_autostats (Transact-SQL) | Documents Microsoft
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
- sp_autostats_TSQL
- sp_autostats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_autostats
ms.assetid: d1df8c15-ee73-49eb-9d13-6e98943c3e38
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ab8716d2a4580edf9d9cdb34a849210d60f31d17
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spautostats-transact-sql"></a>sp_autostats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Affiche ou modifie l'option de mise à jour automatique des statistiques, AUTO_UPDATE_STATISTICS, pour un index, un objet de statistiques, une table ou une vue indexée.  
  
 Pour plus d’informations sur l’option AUTO_UPDATE_STATISTICS, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41; ](../../t-sql/statements/alter-database-transact-sql-set-options.md) et [statistiques](../../relational-databases/statistics/statistics.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_autostats [ @tblname = ] 'table_or_indexed_view_name'   
    [ , [ @flagc = ] 'stats_value' ]   
    [ , [ @indname = ] 'statistics_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tblname=** ] **'***table_or_indexed_view_name***'**  
 Nom de la table ou vue indexée pour laquelle afficher l'option AUTO_UPDATE_STATISTICS. *table_or_indexed_view_name* est **nvarchar(776)**, sans valeur par défaut.  
  
 [  **@flagc=** ] **'***stats_value***'**  
 Met à jour l'option AUTO_UPDATE_STATISTICS à partir de l'une de ces valeurs :  
  
 **ON** = ON  
  
 **OFF** = OFF  
  
 Lorsque *indicateur_stats* est ne pas spécifié, afficher le paramètre AUTO_UPDATE_STATISTICS actuel. *stats_value* est **varchar (10)**, avec NULL comme valeur par défaut.  
  
 [  **@indname=** ] **'***statistics_name***'**  
 Nom des statistiques pour lesquels afficher ou mettre à jour l'option AUTO_UPDATE_STATISTICS. Pour afficher les statistiques d'un index, vous pouvez utiliser le nom de l'index ; un index et son objet de statistiques correspondant portent le même nom.  
  
 *statistics_name* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *indicateur_stats* est spécifié, **sp_autostats** indique l’action qui a été effectuée mais ne retourne aucun jeu de résultats.  
  
 Si *indicateur_stats* n’est pas spécifié, **sp_autostats** retourne le jeu de résultats suivant.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Nom de l'index**|**varchar(60)**|Nom de l'index ou des statistiques.|  
|**STATISTIQUES AUTOMATIQUES**|**varchar(3)**|Valeur actuelle de l'option AUTO_UPDATE_STATISTICS.|  
|**Dernière mise à jour**|**datetime**|Date de la mise à jour des statistiques la plus récente.|  
  
 Le jeu de résultats pour une table ou vue indexée comprend les statistiques créées pour les index, les statistiques de colonne unique générés avec l’option AUTO_CREATE_STATISTICS et aux statistiques créées avec la [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) instruction.  
  
## <a name="remarks"></a>Notes  
 Si l'index spécifié est désactivé ou si la table spécifiée a un index cluster désactivé, un message d'erreur s'affiche.  
  
 AUTO_UPDATE_STATISTICS est toujours désactivé (OFF) pour les tables optimisées en mémoire.  
  
## <a name="permissions"></a>Autorisations  
 Pour modifier l’option AUTO_UPDATE_STATISTICS option requiert l’appartenance au **db_owner** fixe du rôle de base de données ou l’autorisation ALTER sur *table_name*. Pour afficher l’option AUTO_UPDATE_STATISTICS option nécessite l’appartenance dans le **public** rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-display-the-status-of-all-statistics-on-a-table"></a>A. Afficher l'état de toutes les statistiques d'une table  
 L'exemple suivant affiche l'état de toutes les statistiques de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product';  
GO  
```  
  
### <a name="b-enable-autoupdatestatistics-for-all-statistics-on-a-table"></a>B. Activer AUTO_UPDATE_STATISTICS pour toutes les statistiques d'une table  
 L'exemple suivant active l'option AUTO_UPDATE_STATISTICS pour toutes les statistiques de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'ON';  
GO  
```  
  
### <a name="c-disable-autoupdatestatistics-for-a-specific-index"></a>C. Désactiver AUTO_UPDATE_STATISTICS pour un index spécifique  
 L'exemple suivant désactive l'option AUTO_UPDATE_STATISTICS pour l'index `AK_Product_Name` de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'OFF', AK_Product_Name;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
