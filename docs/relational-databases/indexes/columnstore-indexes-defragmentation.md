---
title: Index columnstore - Défragmentation | Microsoft Docs
ms.custom: ''
ms.date: 01/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d3efda1a-7bdb-47f5-80bf-f075329edee5
caps.latest.revision: 17
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a0729bd3d068c7a50c7f6e9df591909465c29003
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="columnstore-indexes---defragmentation"></a>Index columnstore - Défragmentation
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Tâches de défragmentation d’index columnstore.  
  
## <a name="use-alter-index-reorganize-to-defragment-a-columnstore-index-online"></a>Utilisation d’ALTER INDEX REORGANIZE pour défragmenter un index columnstore en ligne  
 **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]),[!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
Après avoir exécuté des charges de tout type, vous pouvez avoir plusieurs petits rowgroups dans le deltastore. Vous pouvez utiliser l’instruction `ALTER INDEX REORGANIZE` pour forcer tous les rowgroups dans le columnstore, puis pour combiner les rowgroups en un plus petit nombre de rowgroups avec plusieurs lignes.  L’opération de réorganisation supprimera également les lignes qui ont été supprimées du columnstore.  
  
Pour plus d’informations, consultez les billets de blog suivants sur le blog de l’équipe du moteur de base de données SQL.  
-   [Réduction de la fragmentation d’index dans des index columnstore (en anglais)](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/07/columnstore-index-defragmentation-using-reorganize-command/)  
-   [Index columnstore et la stratégie de fusion de rowgroups](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/08/columnstore-index-merge-policy-for-reorganize/)  
  
### <a name="recommendations-for-reorganizing"></a>Recommandations pour la réorganisation  
Réorganisez un index columnstore après un ou plusieurs chargements de données pour obtenir des gains de performance des requêtes aussi rapidement que possible. La réorganisation nécessite initialement des ressources processeur supplémentaires pour compresser les données, ce qui peut ralentir les performances globales du système. Toutefois, dès que les données sont compressées, les performances des requêtes s'améliorent.  
  
Utilisez l’exemple dans [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md) pour calculer la fragmentation. Vous pouvez déterminer ainsi s’il est judicieux d’effectuer une opération REORGANIZE.  
  
### <a name="example-how-reorganizing-works"></a>Exemple : fonctionnement de la réorganisation  
 Cet exemple montre comment l’opération ALTER INDEX REORGANIZE peut forcer tous les rowgroups deltastore dans le columnstore et combiner ensuite les rowgroups.  
  
1.  Exécutez cette instruction Transact-SQL pour créer une table de mise en lots contenant 300 000 lignes. Nous pourrons alors charger des lignes en masse dans un index columnstore.  
  
    ```sql  
    USE master;  
    GO  
  
    IF EXISTS (SELECT name FROM sys.databases  
        WHERE name = N'[columnstore]')  
        DROP DATABASE [columnstore];  
    GO  
  
    CREATE DATABASE [columnstore];  
    GO  
  
    IF EXISTS (SELECT name FROM sys.tables  
        WHERE name = N'staging'  
        AND object_id = OBJECT_ID (N'staging'))  
    DROP TABLE dbo.staging;  
    GO  
  
    CREATE TABLE [staging] (  
         AccountKey int NOT NULL,  
         AccountDescription nvarchar (50),  
         AccountType nvarchar(50),  
         AccountCodeAlternateKey int  
    );  
    GO  
  
    -- Load data  
    DECLARE @loop int;  
    DECLARE @AccountDescription varchar(50);  
    DECLARE @AccountKey int;  
    DECLARE @AccountType varchar(50);  
    DECLARE @AccountCode int;  
  
    SELECT @loop = 0;  
    BEGIN TRAN  
        WHILE (@loop < 300000)   
          BEGIN  
            SELECT @AccountKey = CAST (RAND()*10000000 AS int);  
            SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
            SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
            SELECT @AccountCode =  CAST (RAND()*10000000 AS int);  
  
            INSERT INTO staging VALUES (  
               @AccountKey,   
               @AccountDescription,   
               @AccountType,   
               @AccountCode  
            );  
  
            SELECT @loop = @loop + 1;  
          END  
    COMMIT  
    ```  
  
2.  Création d’une table stockée sous la forme d’un index columnstore.  
  
    ```sql  
    IF EXISTS (SELECT name FROM sys.tables  
        WHERE name = N'cci_target'  
        AND object_id = OBJECT_ID (N'cci_target'))  
    DROP TABLE dbo.cci_target;  
    GO  
  
    -- Create a table with a clustered columnstore index  
    -- and the same columns as the rowstore staging table.  
    CREATE TABLE cci_target (  
         AccountKey int NOT NULL,  
         AccountDescription nvarchar (50),  
         AccountType nvarchar(50),  
         AccountCodeAlternateKey int,  
         INDEX idx_cci_target CLUSTERED COLUMNSTORE  
    )  
    GO  
    ```  
  
3.  Insérez en masse les lignes de la table de mise en lots dans la table columnstore. `INSERT INTO ... SELECT` effectue une insertion en bloc (BULK INSERT). `TABLOCK` permet à `INSERT` de s’exécuter avec un parallélisme.  
  
    ```sql  
    -- Insert rows in parallel  
    INSERT INTO cci_target WITH (TABLOCK)  
    SELECT TOP (300000) * FROM staging;  
    GO  
    ```  
  
4.  Affichez les rowgroups à l’aide de la vue de gestion dynamique (DMV) *sys.dm_db_column_store_row_group_physical_stats*.  
  
    ```sql  
    -- Run this dynamic management view (DMV) to see the OPEN rowgroups.   
    -- The number of rowgroups depends on the degree of parallelism.   
    -- You will see multiple OPEN rowgroups depending on the degree of parallelism.   
    -- This is because insert operation can run in parallel in [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].  
  
    SELECT *   
    FROM sys.dm_db_column_store_row_group_physical_stats   
    WHERE object_id  = object_id('cci_target')  
    ORDER BY row_group_id;  
    ```  
  
     Dans cet exemple, les résultats indiquent 8 rowgroups ouverts comportant chacun 37 500 lignes. Le nombre de rowgroups ouverts (OPEN) dépend du paramètre *max_degree_of_parallelism*.  
  
     ![Rowgroups ouverts (OPEN)](../../relational-databases/indexes/media/cci-openrowgroups.png "Rowgroups ouverts (OPEN)")  
  
5.  Utilisez `ALTER INDEX REORGANIZE` avec l’option `COMPRESS_ALL_ROW_GROUPS` pour forcer la compression de tous les rowgroups dans le columnstore.  
  
    ```sql  
    -- This command will force all CLOSED and OPEN rowgroups into the columnstore.  
    ALTER INDEX idx_cci_target ON cci_target   
    REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
    SELECT *   
    FROM sys.dm_db_column_store_row_group_physical_stats   
    WHERE object_id  = object_id('cci_target')  
    ORDER BY row_group_id;  
    ```  
  
     Les résultats indiquent 8 rowgroups compressés et 8 rowgroups TOMBSTONE. Chaque rowgroup est compressé dans le columnstore, quelle que soit sa taille. Les rowgroups TOMBSTONE seront supprimés par le système.  
  
     ![Rowgroups TOMBSTONE et compressés (COMPRESSED)](../../relational-databases/indexes/media/cci-tombstone-compressed-rowgroups.png "Rowgroups TOMBSTONE et compressés (COMPRESSED)")  
  
6.  Il est préférable de combiner de petits rowgroups pour optimiser les performances des requêtes. `ALTER INDEX REORGANIZE` combine les rowgroups `COMPRESSED`. Maintenant que les rowgroups delta sont compressés dans le columnstore, exécutez ALTER INDEX REORGANIZE pour combiner les petits rowgroups compressés. Cette fois, vous n’avez pas besoin d’utiliser l’option `COMPRESS_ALL_ROW_GROUPS`.  
  
    ```sql  
    -- Run this again and you will see that smaller rowgroups   
    -- combined into one compressed rowgroup with 300,000 rows  
    ALTER INDEX idx_cci_target ON cci_target REORGANIZE;  
  
    SELECT *   
    FROM sys.dm_db_column_store_row_group_physical_stats   
    WHERE object_id  = object_id('cci_target')  
    ORDER BY row_group_id;  
    ```  
  
     Les résultats indiquent que les 8 rowgroups compressés sont maintenant regroupées dans un rowgroup compressé.  
  
     ![Rowgroups combinés ](../../relational-databases/indexes/media/cci-compressed-rowgroups.png "Rowgroups combinés ")  
  
## <a name="rebuild"></a> Utilisation d’ALTER INDEX REBUILD pour défragmenter l’index columnstore hors connexion  
 Pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et les versions ultérieures, la reconstruction de l’index columnstore n’est généralement pas nécessaire, car `REORGANIZE` effectue l’essentiel de la reconstruction en arrière-plan sous la forme d’une opération en ligne.  
  
 La reconstruction d’un index columnstore supprime la fragmentation et déplace toutes les lignes dans le columnstore. Utilisez [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md) or [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) pour effectuer une reconstruction complète d’un index columnstore cluster existant. En outre, vous pouvez utiliser ALTER INDEX... REBUILD pour reconstruire une partition spécifique.  
  
### <a name="rebuild-process"></a>Processus de reconstruction  
 Pour reconstruire un index columnstore, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
1.  Acquiert un verrou exclusif sur la table ou la partition lorsque la reconstruction se produit. Les données sont hors connexion et indisponibles pendant la reconstruction, même si vous utilisez `NOLOCK`, RCSI ou SI.  
  
2.  Recompresse toutes les données dans le columnstore. Il existe deux copies de l'index columnstore pendant la reconstruction. Lorsque la reconstruction est terminée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime l'index columnstore d'origine.  
  
### <a name="recommendations-for-rebuilding-a-columnstore-index"></a>Recommandations pour la reconstruction d’un index columnstore  
 Reconstruire un index columnstore est utile pour supprimer une fragmentation et pour déplacer toutes les lignes dans le columnstore. Tenez compte des recommandations suivantes :  
  
1.  Reconstruisez une partition au lieu de la table entière.  
    -   Reconstruire un index columnstore cluster entier prend beaucoup de temps si l'index est volumineux, et cela nécessite suffisamment d'espace disque pour stocker une copie supplémentaire de l'index pendant la reconstruction. Généralement il est nécessaire de reconstruire que la dernière partition utilisée.  
    -   Pour les tables partitionnées, vous n'avez pas besoin de reconstruire l'index columnstore entier, car la fragmentation se produira probablement uniquement dans les partitions modifiées récemment. Les tables de faits et de dimension volumineuses sont généralement partitionnées pour exécuter des opérations de sauvegarde et de gestion sur les segments de la table.  

2.  Reconstruisez une partition après des opérations DML lourdes.  
    -   Reconstruire une partition la défragmentera et réduira la mémoire sur disque. La reconstruction supprimera toutes les lignes marquées pour suppression du columnstoren et déplacera tous les rowgroups du deltastore dans le columnstore. Notez qu’il peut y avoir plusieurs rowgroups dans le deltastore comportent chacun moins d’un million de lignes.  
  
3.  Reconstruisez une partition après le chargement des données.  
    -   Cela garantit que toutes les données sont stockées dans le columnstore. Lorsque des processus simultanés chargent moins 100 000 lignes chacun dans la même partition en même temps, la partition peut se retrouver avec plusieurs deltastores. La reconstruction déplacera toutes les lignes du deltastore dans le columnstore.  

## <a name="automatic-index-and-statistics-management"></a>Gestion automatique des index et des statistiques

Tirez parti de solutions comme [Adaptive Index Defrag](http://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) pour gérer automatiquement la défragmentation des index et les mises à jour des statistiques pour une ou plusieurs bases de données. Cette procédure choisit automatiquement s’il faut reconstruire ou réorganiser un index en fonction de son niveau de fragmentation, entre autres, et mettre à jour les statistiques avec un seuil linéaire.

## <a name="see-also"></a> Voir aussi        
[Index columnstore - Nouveautés](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)    
[Performances des requêtes d’index columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
[Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
[Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
[Index columnstore - Architecture](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)    
[Adaptive Index Defrag](http://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)    
  
  
