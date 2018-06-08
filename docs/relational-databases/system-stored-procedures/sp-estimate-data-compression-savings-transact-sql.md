---
title: sp_estimate_data_compression_savings (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_estimate_data_compression_savings_TSQL
- sp_estimate_data_compression_savings
dev_langs:
- TSQL
helpviewer_keywords:
- compression [SQL Server], estimating
- sp_estimate_data_compression_savings
ms.assetid: 6f6c7150-e788-45e0-9d08-d6c2f4a33729
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 25798372f2b949446b746164665dbfe6752443d7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spestimatedatacompressionsavings-transact-sql"></a>sp_estimate_data_compression_savings (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la taille actuelle de l'objet demandé et estime la taille de l'objet pour l'état de compression demandé. La compression peut être évaluée pour des tables entières ou des parties de tables. Cela inclut des segments de mémoire, des index cluster, des index non cluster, des vues indexées et des partitions de table et d'index. Les objets peuvent être compressés à l'aide de la compression de ligne ou de la compression de page. Si la table, l'index ou la partition sont déjà compressés, vous pouvez utiliser cette procédure pour estimer la taille de la table, de l'index ou de la partition s'ils sont décompressés.  
  
> [!NOTE]  
>  La compression et **sp_estimate_data_compression_savings** ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Pour estimer la taille de l'objet s'il devait utiliser le paramètre de compression demandé, cette procédure stockée échantillonne l'objet source et charge ces données dans une table et un index équivalents, créés dans tempdb. La table ou l'index créés dans tempdb sont ensuite compressés au paramètre demandé et les gains de compression estimés sont calculés.  
  
 Pour modifier l’état de compression de table, l’index ou partition, utilisez le [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) instructions. Pour obtenir des informations générales sur la compression, consultez [la Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
> [!NOTE]  
>  Si les données existantes sont fragmentées, vous pouvez être en mesure de réduire leur taille sans utiliser la compression en reconstruisant l'index. Pour les index, le facteur de remplissage sera appliqué pendant une reconstruction d'index. Cela pourrait augmenter la taille de l'index.  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_estimate_data_compression_savings   
     [ @schema_name = ] 'schema_name'    
   , [ @object_name = ] 'object_name'   
   , [@index_id = ] index_id   
   , [@partition_number = ] partition_number   
   , [@data_compression = ] 'data_compression'   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @schema_name=] '*nom_schéma*'  
 Nom du schéma de base de données qui contient la table ou la vue indexée. *schema_name* est **sysname**. Si *nom_schéma* est NULL, le schéma par défaut de l’utilisateur actuel est utilisé.  
  
 [ @object_name=] '*nom_objet*'  
 Nom du schéma de la table ou de la vue indexée sur laquelle se trouve l'index. *object_name* est de type **sysname**.  
  
 [ @index_id=] '*index_id*'  
 Identificateur de l’index. *index_id* est **int**, et peut prendre l’une des valeurs suivantes : le numéro d’identification d’un index, NULL ou 0 si *object_id* est un segment de mémoire. Pour retourner des informations sur tous les index d'une table de base ou d'une vue, spécifiez la valeur NULL. Si vous spécifiez la valeur NULL, vous devez également spécifier NULL pour *partition_number*.  
  
 [ @partition_number=] '*partition_number*'  
 Numéro de partition dans l'objet. *partition_number* est **int**, et peut prendre l’une des valeurs suivantes : le numéro de partition d’un index ou segment de mémoire, NULL ou 1 pour un segment de mémoire ou un index non partitionné.  
  
 Pour spécifier la partition, vous pouvez également spécifier le [$partition](../../t-sql/functions/partition-transact-sql.md) (fonction). Pour retourner des informations pour toutes les partitions de l'objet propriétaire, spécifiez NULL.  
  
 [ @data_compression=] '*data_compression*'  
 Type de compression à évaluer. *DATA_COMPRESSION* peut prendre l’une des valeurs suivantes : NONE, ROW ou PAGE.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le jeu de résultats suivant est retourné pour fournir la taille actuelle et estimée de la table, de l'index ou de la partition.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_name|**sysname**|Nom de la table ou de la vue indexée.|  
|schema_name|**sysname**|Schéma de la table ou de la vue indexée.|  
|index_id|**int**|ID d'index d'un index :<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = index cluster<br /><br /> > 1 = Index non cluster|  
|partition_number|**int**|Numéro de partition. Retourne 1 pour une table ou un index non partitionnés.|  
|size_with_current_compression_setting (Ko)|**bigint**|Taille de la table, de l'index ou de la partition demandés tels qu'ils existent actuellement.|  
|size_with_requested_compression_setting (Ko)|**bigint**|Taille estimée de la table, de l'index ou de la partition qui utilise le paramètre de compression demandé et, le cas échéant, le facteur de remplissage existant, en supposant qu'il n'y a pas de fragmentation.|  
|sample_size_with_current_compression_setting (Ko)|**bigint**|Taille de l'exemple avec le paramètre de compression actuel. Cela inclut toute fragmentation éventuelle.|  
|sample_size_with_requested_compression_setting (Ko)|**bigint**|Taille de l'échantillon créé à l'aide du paramètre de compression demandé et, le cas échéant, du facteur de remplissage existant, sans fragmentation.|  
  
## <a name="remarks"></a>Notes  
 Utilisez sp_estimate_data_compression_savings pour estimer les gains potentiels lorsque vous activez une table ou une partition pour la compression de ligne ou de page. Par exemple, si la taille moyenne de la ligne peut être réduite de 40 %, vous pouvez réduire la taille de l'objet de 40 %. Vous n'économiserez peut-être pas d'espace car cela dépend du facteur de remplissage et de la taille de la ligne. Par exemple, si vous disposez d'une ligne d'une longueur de 8 000 octets et que vous réduisez sa taille de 40 %, vous ne pouvez toujours pas intégrer plus d'une ligne dans une page de données. Vous ne bénéficiez d'aucun gain.  
  
 Si les résultats de l'exécution de sp_estimate_data_compression_savings indiquent que la taille de la table augmentera, cela signifie que de nombreuses lignes de la table utilisent quasiment la précision complète des types de données, et l'ajout de la faible surcharge requise pour le format compressé dépasse les gains dérivés de la compression. Dans ce cas très peu fréquent, n'activez pas la compression.  
  
 Si une table est activée pour la compression, utilisez sp_estimate_data_compression_savings pour estimer la taille moyenne de la ligne si la table est décompressée.  
  
 Un verrou (IS) est acquis sur la table pendant cette opération. Si un verrou (IS) ne peut pas être obtenu, la procédure sera bloquée. La table est analysée sous le niveau d'isolement de lecture validée.  
  
 Si le paramètre de compression demandé est identique au paramètre de compression actuel, la procédure stockée retournera la taille estimée sans fragmentation de données et en utilisant le facteur de remplissage existant.  
  
 Si l'ID d'index ou de partition n'existe pas, aucun résultat n'est retourné.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur la table.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Cette procédure ne s'applique pas aux tables columnstore et, par conséquent, n'accepte pas les paramètres de compression de données COLUMNSTORE et COLUMNSTORE_ARCHIVE.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous estime la taille de la table `Production.WorkOrderRouting` si elle est compressée à l'aide de la compression `ROW`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_estimate_data_compression_savings 'Production', 'WorkOrderRouting', NULL, NULL, 'ROW' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Implémentation de la compression Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)  
  
  
