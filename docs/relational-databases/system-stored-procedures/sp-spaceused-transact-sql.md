---
title: sp_spaceused (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_spaceused_TSQL
- sp_spaceused
dev_langs:
- TSQL
helpviewer_keywords:
- sp_spaceused
ms.assetid: c6253b48-29f5-4371-bfcd-3ef404060621
caps.latest.revision: 62
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b9e5b528afe048052b3709886a85d7ab6b853777
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spspaceused-transact-sql"></a>sp_spaceused (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Affiche le nombre de lignes, l'espace disque réservé et l'espace disque utilisé par une table, une vue indexée ou une file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)] de la base de données active, ou affiche l'espace disque réservé et utilisé par l'ensemble de la base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_spaceused [[ @objname = ] 'objname' ]   
[, [ @updateusage = ] 'updateusage' ]  
[, [ @mode = ] 'mode' ]  
[, [ @oneresultset = ] oneresultset ]  
[, [ @include_total_xtp_storage = ] include_total_xtp_storage ]
```  
  
## <a name="arguments"></a>Arguments  

Pour [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)], `sp_spaceused` doit spécifier des paramètres nommés (par exemple `sp_spaceused (@objname= N'Table1');` au lieu de vous fier à la position ordinale de paramètres. 

 [  **@objname=**] **'***objname***'** 
   
 Nom qualifié ou non qualifié de la table, de la vue indexée ou de la file d'attente pour laquelle des informations d'utilisation d'espace sont demandées. Les guillemets ne sont nécessaires que si un nom d'objet qualifié est spécifié. Si un nom d'objet complet (incluant un nom de base de données) est fourni, le nom de la base de données doit être celui de la base de données actuelle.  
Si *objname* n’est pas spécifié, les résultats sont renvoyés par la base de données.  
*objname* est **nvarchar(776)**, avec NULL comme valeur par défaut.  
> [!NOTE]  
> [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] prennent uniquement en charge les objets de base de données et de table.
  
 [  **@updateusage=**] **'***updateusage***'**  
 Indique que DBCC UPDATEUSAGE doit être exécuté pour mettre à jour les informations d'utilisation de l'espace. Lorsque *objname* est ne pas spécifié, l’instruction est exécutée sur la base de données ; sinon, l’instruction est exécutée *objname*. Les valeurs peuvent être **true** ou **false**. *UPDATEUSAGE* est **varchar (5)**, avec une valeur par défaut **false**.  
  
 [  **@mode=**] **'***mode***'**  
 Indique l’étendue des résultats. Pour une table étendue ou de la base de données, le *mode* paramètre vous permet d’inclure ou exclure de la partie distante de l’objet. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
 Le *mode* argument peut prendre les valeurs suivantes :  
  
|Valeur| Description|  
|-----------|-----------------|  
|ALL|Retourne les statistiques de stockage de l’objet ou de la base de données, y compris la partie locale et la partie distante.|  
|LOCAL_ONLY|Retourne les statistiques de stockage d’uniquement la partie locale de l’objet ou de la base de données. Si l’objet ou la base de données n’est pas compatible avec Stretch, retourne les statistiques de mêmes que lorsque @mode = ALL.|  
|REMOTE_ONLY|Retourne les statistiques de stockage de seulement la partie distante de l’objet ou de la base de données. Cette option génère une erreur lorsqu’une des conditions suivantes est vraie :<br /><br /> La table n’est pas activée pour l’extension.<br /><br /> La table est activée pour l’extension, mais vous n’avez jamais activé la migration des données. Dans ce cas, la table distante n’a pas encore un schéma.<br /><br /> L’utilisateur a supprimé manuellement de la table distante.<br /><br /> L’approvisionnement de l’archive de données distante retourné un état de réussite, mais en fait il a échoué.|  
  
 *mode* est **varchar(11)**, avec une valeur par défaut **n’all '**.  
  
 [  **@oneresultset=**] *oneresultset*  
 Indique s’il faut retourner un jeu de résultats unique. Le *oneresultset* argument peut prendre les valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Lorsque *@objname* a la valeur null ou n’est ne pas spécifié, les deux jeux de résultats est retournés. Deux jeux de résultats est le comportement par défaut.|  
|1|Lorsque *@objname* = null ou n’est ne pas spécifié, un seul jeu de résultats est retourné.|  
  
 *oneresultset* est **bits**, avec une valeur par défaut **0**.  

[ **@include_total_xtp_storage**] **'***include_total_xtp_storage***'**  
**S’applique à :** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)], [!INCLUDE[sssds-md](../../includes/sssds-md.md)].  
  
 Lorsque @oneresultset= 1, le paramètre @include_total_xtp_storage détermine si le jeu de résultats unique inclut des colonnes pour le stockage MEMORY_OPTIMIZED_DATA. La valeur par défaut est 0, qui, par défaut (si le paramètre est omis) les colonnes XTP ne sont pas inclus dans le jeu de résultats.  

## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *objname* est omis et la valeur de *oneresultset* est 0, les jeux de résultats suivants sont renvoyés pour fournir des informations sur la taille de base de données actuelle.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar(18)**|Taille de la base de données actuelle en mégaoctets. **database_size** inclut les fichiers journaux et de données.|  
|**espace non alloué**|**varchar(18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données.|  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Réservé**|**varchar(18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar(18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar(18)**|Quantité totale d'espace qu'occupent les index.|  
|**Non utilisé**|**varchar(18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|  
  
 Si *objname* est omis et la valeur de *oneresultset* est 1, le jeu de résultats unique suivant est retourné pour fournir des informations de taille de base de données actuelle.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar(18)**|Taille de la base de données actuelle en mégaoctets. **database_size** inclut les fichiers journaux et de données.|  
|**espace non alloué**|**varchar(18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données.|  
|**Réservé**|**varchar(18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar(18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar(18)**|Quantité totale d'espace qu'occupent les index.|  
|**Non utilisé**|**varchar(18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|  
  
 Si *objname* est spécifié, le jeu de résultats suivant est retourné pour l’objet spécifié.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**nvarchar(128)**|Nom de l'objet pour lequel ont été demandées les informations relatives à l'utilisation de l'espace.<br /><br /> Le nom de schéma de l'objet n'est pas renvoyé. Si le nom du schéma est requis, utilisez le [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) ou [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) des vues de gestion dynamique pour obtenir des informations de taille équivalente.|  
|**Lignes**|**char(20)**|Nombre de lignes existant dans la table. Si l'objet spécifié est une file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)], cette colonne indique le nombre de messages en file d'attente.|  
|**Réservé**|**varchar(18)**|Quantité totale d’espace réservé pour *objname*.|  
|**data**|**varchar(18)**|Quantité totale d’espace utilisé par les données dans *objname*.|  
|**index_size**|**varchar(18)**|Quantité totale d’espace utilisé par les index dans *objname*.|  
|**Non utilisé**|**varchar(18)**|Quantité totale d’espace réservé pour *objname* mais pas encore utilisé.|  
 
Il s’agit du mode par défaut, lorsque vous ne spécifiez aucun paramètre. Les jeux de résultats suivants sont retournées les informations sur la taille de détail de la base de données sur le disque. 

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar(18)**|Taille de la base de données actuelle en mégaoctets. **database_size** inclut les fichiers journaux et de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille sur disque totale de tous les fichiers de point de contrôle dans le groupe de fichiers.|  
|**espace non alloué**|**varchar(18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque des fichiers de point de contrôle avec l’état PRECREATED dans le groupe de fichiers.|  

Espace utilisé par les tables dans la base de données : (ce jeu de résultats ne reflète pas les tables mémoire optimisées, car il n’existe aucune gestion de comptes par table de l’utilisation du disque) 

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Réservé**|**varchar(18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar(18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar(18)**|Quantité totale d'espace qu'occupent les index.|  
|**Non utilisé**|**varchar(18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|

Le jeu de résultats suivant est retourné **uniquement si** la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA au moins un conteneur : 

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**xtp_precreated**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec l’état PRECREATED, en Ko. Compte vers l’espace non alloué dans la base de données dans son ensemble. [Par exemple, s’il existe de 600 000 Ko de fichiers de point de contrôle précréés, cette colonne contient ' 600000 Ko ']|  
|**xtp_used**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec les États UNDER CONSTRUCTION, ACTIVE et MERGE TARGET, en Ko. Il s’agit de l’espace disque activement utilisé pour les données dans des tables optimisées en mémoire.|  
|**xtp_pending_truncation**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec l’état WAITING_FOR_LOG_TRUNCATION, en Ko. Il s’agit de l’espace disque utilisé pour les fichiers de point de contrôle qui sont en attente de nettoyage, une fois que la troncation du journal se produit.|

Si *objname* est omis, la valeur d’oneresultset est 1, et *include_total_xtp_storage* est 1, le jeu de résultats unique suivant est retourné pour fournir des informations de taille de base de données actuelle. Si `include_total_xtp_storage` est 0 (la valeur par défaut), les trois dernières colonnes sont omis. 

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nom de la base de données en cours.|  
|**database_size**|**varchar(18)**|Taille de la base de données actuelle en mégaoctets. **database_size** inclut les fichiers journaux et de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille sur disque totale de tous les fichiers de point de contrôle dans le groupe de fichiers.|
|**espace non alloué**|**varchar(18)**|Espace de la base de données qui n'a pas été réservé pour des objets de base de données. Si la base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA, cela inclut la taille totale sur disque des fichiers de point de contrôle avec l’état PRECREATED dans le groupe de fichiers.|  
|**Réservé**|**varchar(18)**|Quantité totale d'espace allouée par les objets dans la base de données.|  
|**data**|**varchar(18)**|Quantité totale d'espace qu'occupent les données.|  
|**index_size**|**varchar(18)**|Quantité totale d'espace qu'occupent les index.|  
|**Non utilisé**|**varchar(18)**|Quantité totale d'espace réservée pour les objets dans la base de données, mais non encore utilisé.|
|**xtp_precreated**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec l’état PRECREATED, en Ko. Ce compte dans l’espace non alloué dans la base de données dans son ensemble. Retourne NULL si la base de données ne dispose pas d’un groupe de fichiers memory_optimized_data au moins un conteneur. *Cette colonne est uniquement inclus if @include_total_xtp_storage= 1*.| 
|**xtp_used**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec les États UNDER CONSTRUCTION, ACTIVE et MERGE TARGET, en Ko. Il s’agit de l’espace disque activement utilisé pour les données dans des tables optimisées en mémoire. Retourne NULL si la base de données ne dispose pas d’un groupe de fichiers memory_optimized_data au moins un conteneur. *Cette colonne est uniquement inclus if @include_total_xtp_storage= 1*.| 
|**xtp_pending_truncation**|**varchar(18)**|Taille totale des fichiers de point de contrôle avec l’état WAITING_FOR_LOG_TRUNCATION, en Ko. Il s’agit de l’espace disque utilisé pour les fichiers de point de contrôle qui sont en attente de nettoyage, une fois que la troncation du journal se produit. Retourne NULL si la base de données ne dispose pas d’un groupe de fichiers memory_optimized_data au moins un conteneur. Cette colonne est uniquement incluse en cas `@include_total_xtp_storage=1`.|

## <a name="remarks"></a>Notes  
 **database_size** est toujours supérieure à la somme des **réservé** + **espace non alloué** , car il inclut la taille des fichiers journaux, mais **réservé**et **unallocated_space** prendre en compte uniquement les pages de données.  
  
 Les pages qui sont utilisés par les index XML et les index de recherche en texte intégral sont inclus dans **index_size** pour les deux jeux de résultats. Lorsque *objname* est spécifié, les pages d’index XML et les index de recherche en texte intégral pour l’objet sont également comptées dans le total **réservé** et **index_size** résultats.  
  
 Si l’utilisation de l’espace est calculée pour une base de données ou un objet qui a un index spatial, les colonnes de la taille de l’espace, tel que **database_size**, **réservé**, et **index_size**, inclut la taille de l’index spatial.  
  
 Lors de la *updateusage* est spécifié, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] analyse les données des pages dans la base de données et les rend nécessaire corrections à la **sys.allocation_units** et **sys.partitions** vues relatives à l’espace de stockage utilisé par chacune des tables du catalogue. Il existe des cas, par exemple après la suppression d'un index, où les informations d'espace de la table peuvent ne plus être à jour. *UPDATEUSAGE* peut prendre un certain temps à s’exécuter sur grandes tables ou bases de données. Utilisez *updateusage* uniquement lorsque vous suspectez que des valeurs incorrectes sont retournées et lorsque le processus n’aura d’effet négatif sur les autres utilisateurs ou processus dans la base de données. Il est également possible d'exécuter DBCC UPDATEUSAGE séparément.  
  
> [!NOTE]  
>  Lorsque vous supprimez ou reconstruisez des index volumineux ou lorsque vous supprimez ou tronquez des tables volumineuses, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations des pages actives et de leurs blocs associés jusqu'à ce que la transaction soit validée. Les opérations de suppression différées ne libèrent pas immédiatement l'espace alloué. Par conséquent, les valeurs retournées par **sp_spaceused** immédiatement après la suppression ou la troncature d’un objet volumineux ne reflètent pas l’espace disque réellement disponible.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation d’exécuter **sp_spaceused** est accordée au rôle **public** . Seuls les membres du rôle de base de données fixe **db_owner** peuvent spécifier le paramètre **@updateusage** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-disk-space-information-about-a-table"></a>A. Affichage d'informations sur l'espace disque occupé par une table  
 L'exemple qui suit donne des informations sur l'espace disque pour la table `Vendor` et ses index.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
### <a name="b-displaying-updated-space-information-about-a-database"></a>B. Affichage d'informations mises à jour sur l'espace occupé par une base de données  
 L'exemple suivant récapitule l'espace utilisé dans la base de données actuelle et utilise le paramètre facultatif `@updateusage` pour forcer le retour de valeurs actualisées.  
  
```sql  
USE AdventureWorks008R2;  
GO  
EXEC sp_spaceused @updateusage = N'TRUE';  
GO  
```  
  
### <a name="c-displaying-space-usage-information-about-the-remote-table-associated-with-a-stretch-enabled-table"></a>C. Affichage des informations sur l’utilisation de l’espace sur la table distante associée à une table compatible Stretch  
 L’exemple suivant récapitule l’espace utilisé par la table distante associée à une table compatible Stretch à l’aide de la **@mode** argument afin de spécifier la cible distante. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
```sql  
USE StretchedAdventureWorks2016  
GO  
EXEC sp_spaceused N'Purchasing.Vendor', @mode = 'REMOTE_ONLY'  
```  
  
### <a name="d-displaying-space-usage-information-for-a-database-in-a-single-result-set"></a>D. Affichage des informations sur l’utilisation de l’espace pour une base de données dans un seul résultat défini  
 L’exemple suivant récapitule l’utilisation de l’espace pour la base de données en cours dans un seul jeu de résultats.  
  
```sql  
USE AdventureWorks2016  
GO  
EXEC sp_spaceused @oneresultset = 1  
```  

### <a name="e-displaying-space-usage-information-for-a-database-with-at-least-one-memoryoptimized-file-group-in-a-single-result-set"></a>E. Affichage des informations sur l’utilisation de l’espace pour une base de données au moins un groupe de fichiers optimisées en mémoire dans un seul jeu de résultats 
 L’exemple suivant récapitule l’utilisation de l’espace pour la base de données en cours au moins un groupe de fichiers optimisées en mémoire dans un seul jeu de résultats.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused @updateusage = 'FALSE', @mode = 'ALL', @oneresultset = '1', @include_total_xtp_storage = '1';
GO
``` 

### <a name="f-displaying-space-usage-information-for-a-memoryoptimized-table-object-in-a-database"></a>F. Affichage des informations sur l’utilisation de l’espace pour un objet de table optimisées en mémoire dans une base de données.
 L’exemple suivant récapitule l’utilisation de l’espace pour un objet de table optimisées en mémoire dans la base de données en cours au moins un groupe de fichiers optimisées en mémoire.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused
@objname = N'VehicleTemparatures',
@updateusage = 'FALSE',
@mode = 'ALL',
@oneresultset = '0',
@include_total_xtp_storage = '1';
GO
```  

## <a name="see-also"></a>Voir aussi  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC UPDATEUSAGE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [Sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
