---
title: CREATE TABLE AS SELECT (Azure SQL Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 10/07/2016
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.component: t-sql|statements
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: fb158bb438482f2edd0d77a0480b04390647fc55
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-table-as-select-azure-sql-data-warehouse"></a>CREATE TABLE AS SELECT (Azure SQL Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

CREATE TABLE AS SELECT (CTAS) est l’une des fonctionnalités de T-SQL les plus importantes. Il s’agit d’une opération entièrement parallélisée qui crée une table en fonction de la sortie d’une instruction SELECT. CTAS offre le moyen le plus simple et le plus rapide de créer une copie de table.   
 
 Par exemple, CTAS vous permet d’effectuer les opérations suivantes :  
  
-   Recréer une table avec une colonne de distribution de hachage différente.
-   Recréer une table comme étant répliquée.   
-   Créer un index columnstore uniquement sur certaines colonnes de la table.  
-   Interroger ou importer des données externes.  

> [!NOTE]  
> CTAS s’ajoute aux fonctionnalités de création de table. Ainsi, au lieu de répéter le contenu de la rubrique CREATE TABLE, cette rubrique décrit les différences entre les instructions CTAS et CREATE TABLE. Pour plus d’informations sur l’instruction CREATE TABLE, consultez [CREATE TABLE (Azure SQL Data Warehouse)](https://msdn.microsoft.com/library/mt203953/). 
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>Syntaxe   

```  
CREATE TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name   
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>   
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for SQL Data Warehouse 
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>Arguments  
Pour plus d’informations, consultez la [section Arguments](https://msdn.microsoft.com/library/mt203953/#Arguments) de la rubrique CREATE TABLE.  

<a name="column-options-bk"></a>

### <a name="column-options"></a>Options de colonne
`column_name` [ ,...`n` ]   
 Les noms de colonne n’autorisent pas les [options de colonne](https://msdn.microsoft.com/library/mt203953/#ColumnOptions) mentionnées dans CREATE TABLE.  À la place, vous pouvez fournir une liste facultative d’un ou plusieurs noms de colonne pour la nouvelle table. Les colonnes de la nouvelle table prennent alors les noms que vous spécifiez. Quand vous spécifiez des noms de colonne, le nombre de colonnes figurant dans la liste de colonnes doit correspondre au nombre de colonnes figurant dans les résultats de l’instruction select. Si vous ne spécifiez pas de noms de colonne, la nouvelle table cible utilise les noms de colonne figurant dans les résultats de l’instruction select. 
  
 Vous ne pouvez spécifier aucune autre option de colonne comme les types de données, le classement ou la possibilité de valeur NULL. Chacun de ces attributs est dérivé des résultats de l’instruction `SELECT`. Cependant, vous pouvez utiliser l’instruction SELECT pour modifier les attributs. Pour obtenir un exemple, consultez [Utiliser CTAS pour modifier des attributs de colonne](#ctas-change-column-attributes-bk).   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>Options de distribution de table

`DISTRIBUTION` = `HASH` ( *distribution_column_name* ) | ROUND_ROBIN | REPLICATE      
L’instruction CTAS nécessite une option de distribution et n’a pas de valeurs par défaut. Elle se distingue en cela de CREATE TABLE qui en possède. 

Pour savoir comment choisir la colonne de distribution la plus appropriée, consultez la section [Options de distribution de table](https://msdn.microsoft.com/library/mt203953/#TableDistributionOptions) de la rubrique CREATE TABLE. 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>Options de partition de table
L’instruction CTAS crée par défaut une table non partitionnée, même si la table source est partitionnée. Pour créer une table partitionnée avec l’instruction CTAS, vous devez spécifier l’option de partition. 

Pour plus d’informations, consultez la section [Options de partition de table](https://msdn.microsoft.com/library/mt203953/#TablePartitionOptions) de la rubrique CREATE TABLE.

<a name="select-options-bk"></a>

### <a name="select-options"></a>Options select
L’instruction select représente la différence fondamentale entre CTAS et CREATE TABLE.  

 `WITH` *common_table_expression*  
 Spécifie un jeu de résultats nommé temporaire, désigné par le terme d'expression de table commune (CTE, Common Table Expression). Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 `SELECT` *select_criteria*  
 Remplit la nouvelle table avec les résultats d’une instruction SELECT. *select_criteria* correspond au corps de l’instruction SELECT qui détermine les données qui sont copiées dans la nouvelle table. Pour plus d’informations sur les instructions SELECT, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>Autorisations  
CTAS exige une autorisation `SELECT` sur les objets référencés dans *select_criteria*.

Pour plus d’informations sur les autorisations permettant de créer une table, consultez [Autorisations](https://msdn.microsoft.com/library/mt203953/#Permissions) dans la rubrique CREATE TABLE. 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>Remarques d'ordre général
Pour plus d’informations, consultez [Remarques d’ordre général](https://msdn.microsoft.com/library/mt203953/#GeneralRemarks) dans la rubrique CREATE TABLE.

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
Azure SQL Data Warehouse ne prend pas encore en charge la création ou la mise à jour automatiques des statistiques.  Pour optimiser les performances de vos requêtes, il est important de créer des statistiques sur toutes les colonnes de toutes les tables après avoir exécuté CTAS et après que les données ont subi des modifications importantes. Pour plus d’informations, consultez [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md).

[SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) ne produit aucun effet sur CTAS. Pour obtenir un comportement similaire, utilisez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
 
Pour plus d’informations, consultez [Limitations et restrictions](https://msdn.microsoft.com/library/mt203953/#LimitationsRestrictions) dans la rubrique CREATE TABLE.

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>Comportement de verrouillage  
 Pour plus d’informations, consultez [Comportement de verrouillage](https://msdn.microsoft.com/library/mt203953/#LockingBehavior) dans la rubrique CREATE TABLE.
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>Performances 

Pour une table de hachage distribuée, CTAS vous permet de choisir une colonne de distribution différente pour bénéficier de meilleures performances pour les jointures et les agrégations. Si votre objectif n’est pas de choisir une colonne de distribution différente, vous bénéficierez de meilleures performances avec CTAS si vous spécifiez la même colonne de distribution, car vous éviterez ainsi une redistribution des lignes. 

Si vous utilisez CTAS pour créer une table et que les performances ne sont pas un facteur déterminant, vous pouvez spécifier `ROUND_ROBIN` pour éviter d’avoir à choisir une colonne de distribution.

Pour éviter un déplacement de données dans les requêtes suivantes, vous pouvez spécifier `REPLICATE` au prix d’un stockage accru pour charger une copie complète de la table sur chaque nœud de calcul.  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>Exemples de copie d’une table

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>A. Utiliser CTAS pour copier une table 
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse

L’une des utilisations les plus courantes de `CTAS` est peut-être celle qui consiste à créer une copie d’une table dans le but de pouvoir modifier le langage de définition de données (DDL). Si par exemple vous avez créé au départ une table de type `ROUND_ROBIN` et que vous voulez maintenant la modifier pour en faire une table distribuée sur une colonne, `CTAS` est la méthode qui vous permettra de modifier la colonne de distribution. `CTAS` permet aussi de modifier le partitionnement, l’indexation ou les types de colonnes.

Supposons que vous avez créé cette table en utilisant le type de distribution par défaut `ROUND_ROBIN` sachant qu’aucune colonne de distribution n’était spécifiée dans l’instruction `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Maintenant, vous voulez créer une copie de cette table avec un index cluster columnstore de façon à profiter des performances offertes par les tables cluster columnstore. Vous voulez aussi distribuer cette table sur ProductKey, car des jointures sont prévues dans cette colonne et vous souhaitez éviter que des données soient déplacées à ces occasions. En dernier lieu, vous souhaitez aussi ajouter le partitionnement à OrderDateKey de façon à pouvoir supprimer rapidement les anciennes données en éliminant les anciennes partitions. Voici l’instruction CTAS permettant de copier l’ancienne table dans une nouvelle.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Enfin, vous pouvez renommer vos tables pour spécifier la nouvelle table et supprimer l’ancienne.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>Exemples d’options de colonne

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>B. Utiliser CTAS pour modifier des attributs de colonne 
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse

Cet exemple utilise CTAS pour modifier des types de données, la possibilité de valeur NULL et le classement pour plusieurs colonnes de la table DimCustomer2.  
  
```  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] int NOT NULL,  
    [GeographyKey] int NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] int NOT NULL, 
    [CustomerKeyChangeNullable] int NULL, 
    [CustomerKeyChangeDataTypeNullable] decimal(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] decimal(10, 2) NOT NULL, 
    [GeographyKeyNoChange] int NULL, 
    [GeographyKeyChangeNotNullable] int NOT NULL, 
    [CustomerAlternateKeyNoChange] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] nvarchar(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
En dernier lieu, vous pouvez utiliser [RENAME &#40;Transact-SQL&#41; ](../../t-sql/statements/rename-transact-sql.md) pour permuter les noms de tables. DimCustomer2 devient ainsi la nouvelle table.

```
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>Exemples de distribution de table

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>C. Utiliser CTAS pour modifier la méthode de distribution d’une table
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse

Cet exemple simple montre comment modifier la méthode de distribution d’une table. Pour détailler la procédure, il transforme une table de hachage distribuée en table round robin (tourniquet), puis reconvertit cette dernière en table de hachage distribuée. La table finale correspond à la table d’origine. 

Dans la plupart des cas, il n’est pas nécessaire de transformer une table de hachage distribuée en table round robin. En revanche, vous serez plus souvent amené à transformer une table round robin en table de hachage distribuée. Par exemple, vous pouvez décider dans un premier temps de charger une nouvelle table sous forme de table round robin pour dans un second temps la convertir en table de hachage distribuée pour bénéficier de meilleures performances de jointure.

Cet exemple utilise l’exemple de base de données AdventureWorks. Pour charger la version SQL Data Warehouse, consultez [Charger des exemples de données dans SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/)
 
```
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];

```  
Ensuite, reconvertissez-la en table de hachage distribuée.

```
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>D. Utiliser CTAS pour convertir une table en table répliquée  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse 

Cet exemple vaut pour la conversion de tables round robin ou de hachage distribuées en table répliquée. Cet exemple précis va encore plus loin que la méthode précédente de modification du type de distribution.  DimSalesTerritory étant une dimension et probablement une table de plus petite taille, vous pouvez choisir de la recréer sous forme de table répliquée pour éviter que des données soient déplacées au moment de la joindre à d’autres tables. 

```
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>E. Utiliser CTAS pour créer une table avec moins de colonnes
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse 

L’exemple suivant crée une table distribuée de type round robin nommée `myTable (c, ln)`. La nouvelle table contient seulement deux colonnes. Elle utilise les alias des colonnes dans l’instruction SELECT à la place des noms des colonnes.  
  
```  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer;  
  
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>Exemples d’indicateurs de requête

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>F. Utiliser un indicateur de requête avec CREATE TABLE AS SELECT (CTAS)  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse
  
Cette requête présente la syntaxe de base pour utiliser un indicateur de jointure de requête avec l’instruction CTAS. Une fois la requête envoyée, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] applique la stratégie de jointure hachée au moment de générer le plan de requête pour chaque distribution individuelle. Pour plus d’informations sur l’indicateur de requête de jointure hachée, consultez [Clause OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
```  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>Exemples de tables externes

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>G. Utiliser CTAS pour importer des données à partir du stockage Blob Azure  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse  

Pour importer des données à partir d’une table externe, utilisez simplement CREATE TABLE AS SELECT pour effectuer une sélection dans la table externe. La syntaxe à utiliser pour sélectionner des données dans une table externe à destination de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] est la même que celle permettant de sélectionner des données dans une table normale.  
  
 L’exemple suivant définit une table externe parmi des données situées dans un compte de stockage Blob Azure. Il utilise ensuite CREATE TABLE AS SELECT pour effectuer une sélection dans la table externe. Les données sont alors importées à partir de fichiers délimités par du texte dans le stockage Blob Azure, puis stockées dans une nouvelle table [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
```  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url varchar(50),  
    event_date date,  
    user_IP varchar(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--SQL Data Warehouse table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>H. Utiliser CTAS pour importer des données Hadoop à partir d’une table externe  
S’applique à : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
Pour importer des données à partir d’une table externe, utilisez simplement CREATE TABLE AS SELECT pour effectuer une sélection dans la table externe. La syntaxe à utiliser pour sélectionner des données dans une table externe à destination de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] est la même que celle permettant de sélectionner des données dans une table normale.  
  
 L’exemple suivant définit une table externe sur un cluster Hadoop. Il utilise ensuite CREATE TABLE AS SELECT pour effectuer une sélection dans la table externe. Les données sont alors importées à partir de fichiers délimités par du texte Hadoop, puis stockées dans une nouvelle table [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
```  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url varchar(50),  
    event_date date,  
    user_IP varchar(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>Exemples d’utilisation de CTAS pour remplacer du code SQL Server

CTAS permet de pallier l’absence de prise en charge de certaines fonctionnalités. En plus de permettre l’exécution de votre code dans l’entrepôt de données, le fait de réécrire le code existant pour utiliser CTAS aura généralement pour effet d’améliorer les performances. C’est le résultat de sa conception entièrement parallélisée. 

> [!NOTE]
> Essayez de penser à CTAS en priorité. Si vous pensez que vous pouvez résoudre un problème avec `CTAS`, c’est qu’il s’agit généralement de la meilleure façon de l’aborder, même si cela sous-entend d’écrire plus de données.
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>I. Utiliser CTAS plutôt que SELECT..INTO  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse

Le code SQL Server utilise généralement SELECT..INTO pour remplir une table avec les résultats d’une instruction SELECT. Voici un exemple d’instruction SQL Server SELECT..INTO.

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Cette syntaxe n’est pas prise en charge dans SQL Data Warehouse et Parallel Data Warehouse. Cet exemple montre comment réécrire l’instruction SELECT..INTO précédente pour en faire une instruction CTAS. Vous pouvez choisir l’une des options DISTRIBUTION décrites dans la syntaxe CTAS. Cet exemple utilise la méthode de distribution ROUND_ROBIN.

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>J. Utiliser CTAS et des jointures implicites pour remplacer des jointures ANSI dans la clause `FROM` d’une instruction `UPDATE`  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse  

Imaginez que vous êtes en présence d’une mise à jour complexe qui joint plus de deux tables et exécute l’instruction UPDATE ou DELETE à l’aide de la syntaxe de jointure ANSI.

Vous devez mettre à jour cette table :

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

La requête initiale peut se présenter comme suit :

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Sachant que SQL Data Warehouse ne prend pas en charge les jointures ANSI dans la clause `FROM` d’une instruction `UPDATE`, vous ne pouvez pas utiliser ce code SQL Server sans le modifier légèrement.

Vous pouvez remplacer ce code en combinant `CTAS` et une jointure implicite :

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>K. Utiliser CTAS pour spécifier les données à conserver au lieu d’utiliser des jointures ANSI dans la clause FROM d’une instruction DELETE  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse  

Parfois, la meilleure approche pour supprimer des données est d’utiliser `CTAS`. Au lieu de supprimer les données, sélectionnez simplement les données que vous voulez conserver. Cela est particulièrement vrai pour les instructions `DELETE` qui utilisent la syntaxe de jointure ANSI, car SQL Data Warehouse ne prend pas en charge les jointures ANSI dans la clause `FROM` d’une instruction `DELETE`.

Voici un exemple d’instruction DELETE convertie :

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>L. Utiliser CTAS pour simplifier les instructions merge  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse  

Les instructions merge peuvent être remplacées, du moins en partie, à l’aide de `CTAS`. Vous pouvez regrouper `INSERT` et `UPDATE` dans une même instruction. Les enregistrements supprimés doivent être fermés dans une deuxième instruction.

Voici un exemple avec `UPSERT` :

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>M. Déclarer explicitement un type de données et la possibilité de valeur NULL de la sortie  
S’applique à : Azure SQL Data Warehouse et Parallel Data Warehouse  

Au moment de migrer du code SQL Server vers SQL Data Warehouse, il se peut que vous rencontriez un modèle de codage de ce type :

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Vous pourriez penser instinctivement que ce code doit être migré vers CTAS, et vous auriez raison. Or, il y a un problème caché.

Le code suivant NE produit PAS le même résultat :

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Notez que la colonne « result » reprend le type de données et la possibilité de valeur NULL de l’expression. Cela peut occasionner de légers écarts dans les valeurs si vous ne faites pas attention.

Faites un essai avec l’exemple suivant :

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Les valeurs de résultats enregistrées sont différentes. Comme la valeur persistante dans la colonne « result » est utilisée dans d’autres expressions, l’erreur devient plus significative.

![Résultats de CREATE TABLE AS SELECT](../../t-sql/statements/media/create-table-as-select-results.png)

Cela est particulièrement important pour les migrations de données. Même si la deuxième requête est sans doute plus précise, il y a un problème. Les données sont différentes par rapport au système source, ce qui soulève la question de l’intégrité de la migration. Il s’agit de l’un des rares cas où la « mauvaise » réponse est en fait la bonne réponse !

Cette différence entre les deux résultats est liée à la conversion de type (transtypage) implicite. Dans le premier exemple, la table spécifie la définition de colonne. Au moment où la ligne est insérée, une conversion de type implicite se produit. Dans le deuxième exemple, il n’y a pas de conversion de type implicite, car l’expression définit le type de données de la colonne. Il est aussi à noter que la colonne dans le deuxième exemple a été définie en tant que colonne pouvant accepter les valeurs NULL, ce qui n’est pas le cas dans le premier exemple. Quand la table a été créée dans le premier exemple, la possibilité de valeur NULL dans la colonne était définie explicitement. Dans le deuxième exemple, elle a juste été laissée dans l’expression ce qui, par défaut, donne une définition NULL.  

Pour éviter ce type de problème, vous devez définir explicitement la conversion de type et la possibilité de valeur NULL dans la partie `SELECT` de l’instruction `CTAS`. Vous ne pouvez pas définir ces propriétés dans la partie « create table ».

L’exemple ci-dessous montre comment corriger le code :

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Notez les points suivants :
- CAST ou CONVERT aurait pu être utilisé
- ISNULL est utilisé pour forcer la possibilité de valeur NULL et non COALESCE
- ISNULL est la fonction la plus à l’extérieur
- La deuxième partie de ISNULL est une constante, c’est-à-dire 0

> [!NOTE]
> Pour que la possibilité de valeur NULL soit correctement définie, il est indispensable d’utiliser `ISNULL` et non `COALESCE`. `COALESCE` n’est pas une fonction déterministe. De ce fait, le résultat de l’expression peut toujours prendre la valeur NULL. La fonction `ISNULL` est différente. Elle est déterministe. Par conséquent, quand la deuxième partie de la fonction `ISNULL` est une constante ou un littéral, la valeur obtenue n’est pas NULL.

Ce conseil n’est pas seulement utile pour assurer l’intégrité de vos calculs. Il est aussi important pour le basculement de partition de table. Imaginez que vous avez défini cette table :

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Or, il s’avère que le champ de valeur est une expression calculée ; il ne fait pas partie des données sources.

Pour créer un jeu de données partitionné, voici ce que vous devez faire :

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

La requête s’exécuterait parfaitement, mais le problème se manifesterait quand vous tenteriez de procéder au basculement de partition. Les définitions de table ne correspondent pas. Pour que les définitions de table correspondent, CTAS doit être modifié.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Comme vous pouvez le remarquer, la cohérence des types et le maintien des propriétés de possibilité de valeur NULL au niveau de CTAS constituent une bonne pratique d’ingénierie. Elle vous permet de préserver l’intégrité de vos calculs et garantit aussi la possibilité d’un basculement de partition.
 
## <a name="see-also"></a> Voir aussi  
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/4ae1b23c-67f6-41d0-b614-7a8de914d145)  
  
  


