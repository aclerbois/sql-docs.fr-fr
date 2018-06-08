---
title: sp_createstats (Transact-SQL) | Documents Microsoft
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
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
caps.latest.revision: 47
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6c37e65276e14bc8687f9ffceb1f5a2a5f3ca655
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcreatestats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Appelle le [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) instruction pour créer des statistiques de colonne unique sur les colonnes qui ne sont pas déjà la première colonne dans un objet de statistiques. Création des statistiques de colonne unique augmente le nombre d’histogrammes, ce qui peut améliorer les estimations de cardinalité, les plans de requête et les performances des requêtes. La première colonne d'un objet de statistiques comporte un histogramme ; les autres colonnes n'ont pas d'histogramme.  
  
 sp_createstats est utile aux applications de tests des performances, par exemple, pour lesquelles les délais d'exécution des requêtes sont critiques, car il n'est pas possible d'attendre que l'optimiseur de requête génère des statistiques de colonnes uniques. Dans la plupart des cas, il n’est pas nécessaire d’utiliser sp_createstats ; l’optimiseur de requête génère des statistiques de colonnes uniques selon les besoins pour améliorer les plans lorsque le **AUTO_CREATE_STATISTICS** option est activée.  
  
 Pour plus d’informations sur les statistiques, consultez [statistiques](../../relational-databases/statistics/statistics.md). Pour plus d’informations sur la génération de statistiques de colonne unique, consultez le **AUTO_CREATE_STATISTICS** option [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@indexonly=** ] **'indexonly'**  
 Crée des statistiques uniquement sur les colonnes qui se trouvent dans un index existant et qui ne constituent pas la première colonne d'une définition d'index. **indexonly** est **char (9)**. La valeur par défaut est NO.  
  
 [  **@fullscan=** ] **« fullscan »**  
 Utilise le [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) instruction avec le **FULLSCAN** option. **FULLSCAN** est **char (9)**.  La valeur par défaut est NO.  
  
 [  **@norecompute=** ] **'norecompute'**  
 Utilise le [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) instruction avec le **NORECOMPUTE** option. **NORECOMPUTE** est **char(12)**.  La valeur par défaut est NO.  
  
 [  **@incremental=** ] **'incremental'**  
 Utilise le [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) instruction avec le **INCREMENTAL = ON** option. **Incrémentielle** est **char(12)**.  La valeur par défaut est NO.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Chaque nouvel objet de statistiques porte le même nom que la colonne sur laquelle il est créé.  
  
## <a name="remarks"></a>Notes  
 sp_createstats ne pas créer ou mettre à jour les statistiques sur les colonnes qui constituent la première colonne dans un objet de statistiques existant ;  Cela inclut la première colonne de statistiques créées pour les index, les colonnes ayant des statistiques de colonne unique générés avec l’option AUTO_CREATE_STATISTICS et la première colonne de statistiques créées avec l’instruction CREATE STATISTICS. sp_createstats ne crée pas de statistiques sur les premières colonnes d’index désactivés, sauf si cette colonne est utilisée dans un autre index activé. sp_createstats ne crée pas de statistiques sur les tables avec un index cluster désactivé.  
  
 Lorsque la table contient un jeu de colonnes, sp_createstats ne crée pas de statistiques sur les colonnes éparses. Pour plus d’informations sur les jeux de colonnes et les colonnes éparses, consultez [utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md) et [utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>A. Créer des statistiques de colonnes uniques sur toutes les colonnes appropriées  
 L'exemple suivant crée des statistiques de colonnes uniques pour toutes les colonnes appropriées de la base de données active.  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. Créer des statistiques de colonnes uniques sur toutes les colonnes d'index appropriées  
 L'exemple suivant crée des statistiques de colonnes uniques pour toutes les colonnes appropriées qui sont déjà dans un index et qui ne constituent pas la première colonne de cet index.  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
