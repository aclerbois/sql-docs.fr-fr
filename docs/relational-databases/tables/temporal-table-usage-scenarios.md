---
title: Scénarios d’utilisation de table temporelle | Microsoft Docs
ms.custom: ''
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4b8fa2dd-1790-4289-8362-f11e6d63bb09
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: e990322a00140571599726a231543cadd5fd525c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="temporal-table-usage-scenarios"></a>Scénarios d’utilisation de table temporelle
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Les tables temporelles sont généralement utiles dans les scénarios qui exigent un suivi de l’historique des modifications de données.    
Nous vous recommandons d’envisager les tables temporelles dans les cas d’usage suivants, car elles offrent des avantages importants au niveau de la productivité.  
  
## <a name="data-audit"></a>Audit des données  
 Utilisez la gestion système des versions temporelle sur les tables qui stockent des informations critiques pour lesquelles vous devez effectuer le suivi de ce qui a changé et des moments où les modifications ont eu lieu, et mener des investigations légales sur les données à tout moment.    
Les tables avec version système temporelles vous permettent de planifier les scénarios d’audit de données dans les premières étapes du cycle de développement ou d’ajouter un audit de données à des applications ou solutions existantes quand vous en avez besoin.  
  
 Le diagramme suivant illustre un scénario avec une table Employee, dont l’échantillon de données comprend des versions de ligne actuelles (bleu) et historiques (gris).   
La partie droite du diagramme indique les versions de ligne sur l’axe du temps et les lignes qui sont sélectionnées avec différents types de requêtes sur la table temporelle avec ou sans la clause SYSTEM_TIME.  
  
 ![TemporalUsageScenario1](../../relational-databases/tables/media/temporalusagescenario1.png "TemporalUsageScenario1")  
  
### <a name="enabling-system-versioning-on-a-new-table-for-data-audit"></a>Activation de la gestion système des versions sur une nouvelle table en vue d’un audit des données  
 Si vous avez identifié des informations qui exigent un audit de données, créez des tables de base de données en tant que tables avec version système temporelles. L’exemple simple suivant illustre un scénario dont les informations se trouvent dans une table Employee appartenant à une base de données de ressources humaines hypothétique :  
  
```  
CREATE TABLE Employee   
(    
  [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED   
  , [Name] nvarchar(100) NOT NULL  
  , [Position] varchar(100) NOT NULL   
  , [Department] varchar(100) NOT NULL  
  , [Address] nvarchar(1024) NOT NULL  
  , [AnnualSalary] decimal (10,2) NOT NULL  
  , [ValidFrom] datetime2 (2) GENERATED ALWAYS AS ROW START  
  , [ValidTo] datetime2 (2) GENERATED ALWAYS AS ROW END  
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)  
 )    
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));  
```  
  
 Diverses options pour créer une table avec gestion système des versions temporelle sont décrites dans [Création d’une table temporelle avec versions gérées par le système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md).  
  
### <a name="enabling-system-versioning-on-an-existing-table-for-data-audit"></a>Activation de la gestion système des versions sur une table existante en vue d’un audit des données  
 Si vous avez besoin d’effectuer un audit des données dans des bases de données existantes, utilisez ALTER TABLE pour convertir les tables non temporelles en tables avec version système. Pour préserver votre application, ajoutez des colonnes de période avec l’option HIDDEN, comme expliqué dans [Modifier une table non temporelle pour la convertir en table temporelle avec versions gérées par le système](https://msdn.microsoft.com/library/mt590957.aspx#Anchor_3). L’exemple suivant illustre l’activation de la gestion système des versions sur une table Employee existante appartenant à une base de données de ressources humaines hypothétique :  
  
```  
/*   
Turn ON system versioning in Employee table in two steps   
(1) add new period columns (HIDDEN)   
(2) create default history table   
*/   
ALTER TABLE Employee   
ADD   
    ValidFrom datetime2 (2) GENERATED ALWAYS AS ROW START HIDDEN    
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())  
    , ValidTo datetime2 (2)  GENERATED ALWAYS AS ROW END HIDDEN     
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'  
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);   
  
ALTER TABLE Employee    
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Employee_History));  
```  
  
 Une fois exécuté le script ci-dessus, toutes les modifications de données sont collectées en toute transparence dans la table de l’historique.    
Dans un scénario d’audit de données standard, vous souhaitez connaître toutes les modifications de données qui ont été appliquées à une ligne spécifique au cours d’une période digne d’intérêt. La table de l’historique par défaut est créée avec un arbre B (B-tree) rowstore cluster pour traiter efficacement ce cas d’usage.  
  
### <a name="performing-data-analysis"></a>Analyse des données  
 Une fois que vous avez activé la gestion système des versions à l’aide d’une des méthodes ci-dessus, une requête vous suffit pour lancer l’audit des données. La requête suivante recherche les versions de ligne dans la table Employee pour lesquelles EmployeeID vaut 1000 et qui ont été actives pendant au moins une partie de la période comprise entre le 1er janvier 2014 et le 1er janvier 2015 (limite supérieure comprise) :  
  
```  
SELECT * FROM Employee   
    FOR SYSTEM_TIME    
        BETWEEN '2014-01-01 00:00:00.0000000' AND '2015-01-01 00:00:00.0000000'   
            WHERE EmployeeID = 1000 ORDER BY ValidFrom;  
```  
  
 Remplacez FOR SYSTEM_TIME BETWEEN...AND par FOR SYSTEM_TIME ALL pour analyser l’historique complet des modifications de données pour cet employé :  
  
```  
SELECT * FROM Employee   
    FOR SYSTEM_TIME ALL WHERE    
        EmployeeID = 1000 ORDER BY ValidFrom;  
```  
  
 Pour rechercher les versions de ligne qui étaient actives uniquement pendant une période (et pas en dehors de celle-ci), utilisez CONTAINED IN. Cette requête est très efficace, car elle interroge uniquement la table de l’historique :  
  
```  
SELECT * FROM Employee FOR SYSTEM_TIME    
    CONTAINED IN ('2014-01-01 00:00:00.0000000', '2015-01-01 00:00:00.0000000')   
        WHERE EmployeeID = 1000 ORDER BY ValidFrom;  
```  
  
 Enfin, dans certains scénarios d’audit, vous pouvez voir l’aspect que présentait une table entière à n’importe quel point passé dans le temps :  
  
```  
SELECT * FROM Employee FOR SYSTEM_TIME AS OF '2014-01-01 00:00:00.0000000' ;  
```  
  
 Les tables temporelles avec version système stockent des valeurs pour les colonnes de période dans le fuseau horaire UTC, même s’il est toujours plus pratique d’utiliser le fuseau horaire local à la fois pour le filtrage des données et l’affichage des résultats. L’exemple de code suivant montre comment appliquer la condition de filtrage qui est spécifiée à l’origine dans le fuseau horaire local puis convertie au format UTC à l’aide de l’instruction AT TIME ZONE introduite dans SQL Server 2016 :  
  
```  
/*Add offset of the local time zone to current time*/  
DECLARE @asOf DATETIMEOFFSET = GETDATE() AT TIME ZONE 'Pacific Standard Time'  
/*Convert AS OF filter to UTC*/  
SET @asOf = DATEADD (MONTH, -9, @asOf) AT TIME ZONE 'UTC';  
  
SELECT   
    EmployeeID  
    , Name  
    , Position  
    , Department  
    , [Address]  
    , [AnnualSalary]  
    , ValidFrom AT TIME ZONE 'Pacific Standard Time' AS ValidFromPT   
    , ValidTo AT TIME ZONE 'Pacific Standard Time' AS ValidToPT  
FROM Employee   
    FOR SYSTEM_TIME AS OF @asOf where EmployeeId = 1000  
  
```  
  
 L’instruction AT TIME ZONE est utile dans tous les autres scénarios faisant appel à des tables avec version système.  
  
> [!TIP]  
>  Les conditions de filtrage spécifiées dans des clauses temporelles avec FOR SYSTEM_TIME sont dites « SARG-able » ; en d’autres termes, SQL Server peut utiliser un index cluster sous-jacent pour effectuer une recherche au lieu d’une opération d’analyse.   
> Si vous interrogez directement la table de l’historique, vérifiez que votre condition de filtrage est également « SARG-able » en spécifiant des filtres sous la forme \<colonne de période>  {< | > | =, …} condition_date AT TIME ZONE ‘UTC’.  
> Si vous appliquez AT TIME ZONE à des colonnes de période, SQL Server effectue une analyse de table/index, qui peut être très coûteuse. Évitez ce type de condition dans vos requêtes :  
> \<colonne de période>  AT TIME ZONE '\<votre fuseau horaire>'  >  {< | > | =, …} condition_date.  
  
 Voir aussi [Interrogation des données dans une table temporelle avec versions gérées par le système](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md).  
  
## <a name="point-in-time-analysis-time-travel"></a>Analyses à un point dans le temps (voyage dans le temps)  
 À la différence de l’audit de données, qui se concentre essentiellement sur les modifications apportées à des enregistrements individuels, les scénarios de voyage dans le temps permettent aux utilisateurs de voir comment des jeux de données entiers changent au fil du temps. Parfois, le voyage dans le temps fait appel à plusieurs tables temporelles connexes, chacune évoluant à son propre rythme, dont vous pouvez analyser les éléments suivants :  
  
-   Tendances des indicateurs importants dans les données historiques et les données actuelles  
  
-   Instantané exact de la totalité des données « depuis » (AS OF) n’importe quel point passé dans le temps (hier, il y a un mois , etc.)  
  
-   Différences entre deux points dans le temps dignes d’intérêt (il y a un mois et il y a trois mois, par exemple)  
  
 Il existe plusieurs scénarios concrets qui exigent une analyse de voyage dans le temps. Pour illustrer ce scénario d’utilisation, nous allons examiner OLTP avec l’historique généré automatiquement.  
  
### <a name="oltp-with-auto-generated-data-history"></a>OLTP avec l’historique des données généré automatiquement  
 Dans les systèmes de traitement transactionnel, il n’est pas rare d’analyser l’évolution de métriques importantes dans le temps. Dans l’idéal, l’analyse de l’historique ne doit pas compromettre les performances de l’application OLTP, où l’accès à l’état des données le plus récent doit se produire avec une latence et un verrouillage des données minimaux.  Les tables avec version système temporelles sont conçues pour permettre aux utilisateurs de conserver en toute transparence l’historique complet des modifications en vue d’une analyse ultérieure, séparément des données actuelles, avec un impact minime sur la charge de travail OLTP principale.  
Pour les charges de travail de traitement transactionnel élevées, nous vous recommandons d’utiliser des [tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md); ainsi, vous pouvez stocker les données actuelles en mémoire et l’historique complet des modifications sur le disque à moindres coûts.  
  
 Pour la table de l’historique, nous vous recommandons d’utiliser un index columnstore cluster pour les raisons suivantes :  
  
-   L’analyse de tendances classique bénéficie des performances des requêtes que procure un index columnstore cluster.  
  
-   La tâche de vidage des données avec des tables optimisées en mémoire fonctionne mieux sous une lourde charge de travail OLTP quand la table de l’historique a un index columnstore cluster.  
  
-   Un index columnstore cluster fournit une excellente compression, surtout dans les scénarios où toutes les colonnes ne sont pas modifiées en même temps.  
  
 Utiliser des tables temporelles avec l’OLTP en mémoire réduit le besoin de conserver la totalité du jeu de données en mémoire et vous permet de différencier facilement les données à chaud des données à froid.  
Parmi les exemples de scénarios concrets qui rentrent dans cette catégorie, citons la gestion des stocks ou la négociation en devises.  
  
 Le diagramme suivant montre un modèle de données simplifié utilisé pour la gestion de stocks :  
  
 ![TemporalUsageInMemory](../../relational-databases/tables/media/temporalusageinmemory.png "TemporalUsageInMemory")  
  
 L’exemple de code suivant crée la table ProductInventory comme table temporelle avec version système en mémoire avec un index columnstore cluster sur la table de l’historique (qui remplace en fait l’index rowstore créé par défaut) :  
  
> [!NOTE]  
>  Vérifiez que votre base de données permet de créer des tables optimisées en mémoire. Consultez [Création d’une table mémoire optimisée et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
```  
USE TemporalProductInventory  
GO  
  
BEGIN  
    --If table is system-versioned, SYSTEM_VERSIONING must be set to OFF first   
    IF ((SELECT temporal_type FROM SYS.TABLES WHERE object_id = OBJECT_ID('dbo.ProductInventory', 'U')) = 2)  
    BEGIN  
        ALTER TABLE [dbo].[ProductInventory] SET (SYSTEM_VERSIONING = OFF)  
    END  
    DROP TABLE IF EXISTS [dbo].[ProductInventory]  
       DROP TABLE IF EXISTS [dbo].[ProductInventoryHistory]  
END  
GO  
  
CREATE TABLE [dbo].[ProductInventory]  
(  
    ProductId int NOT NULL,  
    LocationID INT NOT NULL,  
    Quantity int NOT NULL CHECK (Quantity >=0),  
  
    SysStartTime datetime2(0) GENERATED ALWAYS AS ROW START  NOT NULL ,  
    SysEndTime datetime2(0) GENERATED ALWAYS AS ROW END  NOT NULL ,  
    PERIOD FOR SYSTEM_TIME(SysStartTime,SysEndTime),  
  
    --Primary key definition  
    CONSTRAINT PK_ProductInventory PRIMARY KEY NONCLUSTERED (ProductId, LocationId)  
)  
WITH  
(  
    MEMORY_OPTIMIZED=ON,      
    SYSTEM_VERSIONING = ON   
    (          
        HISTORY_TABLE = [dbo].[ProductInventoryHistory],          
        DATA_CONSISTENCY_CHECK = ON  
    )  
)  
  
CREATE CLUSTERED COLUMNSTORE INDEX IX_ProductInventoryHistory ON [ProductInventoryHistory]  
WITH (DROP_EXISTING = ON);  
```  
  
 Pour le modèle ci-dessus, la procédure de gestion des stocks pourrait ressembler à ceci :  
  
```  
CREATE PROCEDURE [dbo].[spUpdateInventory]  
@productId int,  
@locationId int,  
@quantityIncrement int  
  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')  
    UPDATE dbo.ProductInventory  
        SET Quantity = Quantity + @quantityIncrement   
            WHERE ProductId = @productId AND LocationId = @locationId  
  
/*If zero rows were updated than this is insert of the new product for a given location*/  
    IF @@rowcount = 0  
        BEGIN  
            IF @quantityIncrement < 0  
                SET @quantityIncrement = 0  
            INSERT INTO [dbo].[ProductInventory]  
                (  
                    [ProductId]  
                    ,[LocationID]  
                    ,[Quantity]  
                )  
                VALUES  
                   (  
                        @productId  
                       ,@locationId  
                       ,@quantityIncrement  
        END  
END;  
```  
  
 La procédure stockée spUpdateInventory insère un nouveau produit dans l’inventaire ou met à jour la quantité du produit pour l’emplacement spécifique. La logique métier est très simple et consiste à maintenir le dernier état précis tout le temps en incrémentant/décrémentant le champ Quantity par le biais de la mise à jour de la table, tandis que les tables avec version système ajoutent en toute transparence une dimension d’historique aux données, comme l’illustre le diagramme ci-dessous.  
  
 ![TemporalUsageInMemory2b](../../relational-databases/tables/media/temporalusageinmemory2b.png "TemporalUsageInMemory2b")  
  
 À présent, l’interrogation du dernier état peut être effectuée efficacement dans le module compilé en mode natif :  
  
```  
CREATE PROCEDURE [dbo].[spQueryInventoryLatestState]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')  
    SELECT ProductId, LocationID, Quantity, SysStartTime  
      FROM dbo.ProductInventory  
    ORDER BY ProductId, LocationId  
END;  
GO  
EXEC [dbo].[spQueryInventoryLatestState];  
```  
  
 L’analyse des modifications des données au fil du temps devient très facile avec la clause FOR SYSTEM_TIME ALL, comme l’illustre l’exemple suivant :  
  
```  
DROP VIEW IF EXISTS vw_GetProductInventoryHistory;  
GO  
CREATE VIEW vw_GetProductInventoryHistory  
AS  
   SELECT ProductId, LocationId, Quantity, SysStartTime, SysEndTime   
   FROM [dbo].[ProductInventory]  
   FOR SYSTEM_TIME ALL;  
GO  
SELECT * FROM vw_GetProductInventoryHistory   
    WHERE ProductId = 2;  
```  
  
 Le diagramme suivant montre, pour un produit, l’historique des données, que vous pouvez afficher facilement en important la vue ci-dessus dans Power Query, Power BI ou un outil décisionnel similaire :  
  
 ![ProductHistoryOverTime](../../relational-databases/tables/media/producthistoryovertime.png "ProductHistoryOverTime")  
  
 Les tables temporelles peuvent être utilisées dans ce scénario pour effectuer d’autres types d’analyse de voyage dans le temps, tels que la reconstruction de l’état de l’inventaire à partir de n’importe quel point passé dans le temps (AS OF) ou la comparaison d’instantanés qui appartiennent à différents moments dans le temps.  
  
 Pour ce scénario d’utilisation, vous pouvez également convertir les tables Product et Location en tables temporelles, en vue d’analyser l’historique des modifications des données UnitPrice et NumberOfEmployee.  
  
```  
ALTER TABLE Product   
ADD   
    SysStartTime datetime2 (2) GENERATED ALWAYS AS ROW START HIDDEN    
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())  
    , SysEndTime datetime2 (2)  GENERATED ALWAYS AS ROW END HIDDEN     
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'  
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);   
  
ALTER TABLE Product    
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductHistory));  
  
ALTER TABLE [Location]  
ADD   
    SysStartTime datetime2 (2) GENERATED ALWAYS AS ROW START HIDDEN    
        constraint DFValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())  
    , SysEndTime datetime2 (2)  GENERATED ALWAYS AS ROW END HIDDEN     
        constraint DFValidTo DEFAULT '9999.12.31 23:59:59.99'  
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);  
  
ALTER TABLE [Location]    
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.LocationHistory));  
```  
  
 Étant donné que le modèle de données implique maintenant plusieurs tables temporelles, la meilleure pratique pour une analyse AS OF consiste à créer une vue qui extrait les données nécessaires des tables connexes et à appliquer FOR SYSTEM_TIME AS OF à la vue, ce qui permet de simplifier sensiblement la reconstruction de l’état du modèle de données entier :  
  
```  
DROP VIEW IF EXISTS vw_ProductInventoryDetails;  
GO  
  
CREATE VIEW vw_ProductInventoryDetails  
AS  
    SELECT PrInv.ProductId ,PrInv.LocationId, P.ProductName, L.LocationName, PrInv.Quantity  
    , P.UnitPrice, L.NumberOfEmployees  
    , P.SysStartTime AS ProductStartTime, P.SysEndTime AS ProductEndTime  
    , L.SysStartTime AS LocationStartTime, L.SysEndTime AS LocationEndTime  
    , PrInv.SysStartTime AS InventoryStartTime, PrInv.SysEndTime AS InventoryEndTime  
FROM dbo.ProductInventory as PrInv  
JOIN dbo.Product AS P ON PrInv.ProductId = P.ProductID  
JOIN dbo.Location AS L ON PrInv.LocationId = L.LocationID;  
GO  
SELECT * FROM vw_ProductInventoryDetails  
    FOR SYSTEM_TIME AS OF '2015.01.01';   
```  
  
 L’illustration suivante montre le plan d’exécution généré pour la requête SELECT. Comme vous pouvez le constater, le moteur SQL Server prend en charge toute la complexité de la gestion des relations temporelles :  
  
 ![ASOFExecutionPlan](../../relational-databases/tables/media/asofexecutionplan.png "ASOFExecutionPlan")  
  
 Le code suivant permet de comparer l’état du stock de produits entre deux points dans le temps (il y a un jour et il y a un mois) :  
  
```  
DECLARE @dayAgo datetime2 (0) = DATEADD (day, -1, SYSUTCDATETIME());  
DECLARE @monthAgo datetime2 (0) = DATEADD (month, -1, SYSUTCDATETIME());  
  
SELECT   
    inventoryDayAgo.ProductId  
    , inventoryDayAgo.ProductName  
    , inventoryDayAgo.LocationName  
    , inventoryDayAgo.Quantity AS QuantityDayAgo,inventoryMonthAgo.Quantity AS QuantityMonthAgo  
    , inventoryDayAgo.UnitPrice AS UnitPriceDayAgo, inventoryMonthAgo.UnitPrice AS UnitPriceMonthAgo  
FROM vw_ProductInventoryDetails  
FOR SYSTEM_TIME AS OF @dayAgo AS inventoryDayAgo  
JOIN vw_ProductInventoryDetails FOR SYSTEM_TIME AS OF @monthAgo AS inventoryMonthAgo  
    ON inventoryDayAgo.ProductId = inventoryMonthAgo.ProductId AND inventoryDayAgo.LocationId = inventoryMonthAgo.LocationID;  
```  
  
## <a name="anomaly-detection"></a>Détection d’anomalie  
 La détection d’anomalies (ou détection de valeurs hors norme) est l’identification d’éléments qui ne sont pas conformes à un modèle attendu ou à d’autres éléments dans un jeu de données.   
Vous pouvez utiliser des tables temporelles avec version système pour détecter les anomalies qui se produisent régulièrement ou irrégulièrement, et vous pouvez effectuer des interrogations temporelles pour localiser rapidement des modèles spécifiques.  
Le type de données que vous collectez et votre logique métier déterminent la nature des anomalies.  
  
 L’exemple suivant montre une logique simplifiée pour la détection des « pics » dans les chiffres de ventes. Supposons que vous travaillez avec une table temporelle qui collecte l’historique des produits achetés :  
  
```  
CREATE TABLE [dbo].[Product]  
                (  
            [ProdID] [int] NOT NULL PRIMARY KEY CLUSTERED  
        , [ProductName] [varchar](100) NOT NULL  
        , [DailySales] INT NOT NULL  
        , [ValidFrom] [datetime2](7) GENERATED ALWAYS AS ROW START NOT NULL  
        , [ValidTo] [datetime2](7) GENERATED ALWAYS AS ROW END NOT NULL  
        , PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])  
    )  
    WITH( SYSTEM_VERSIONING = ON (HISTORY_TABLE = [dbo].[ProductHistory]   
        , DATA_CONSISTENCY_CHECK = ON ))  
  
```  
  
 Le diagramme suivant montre les achats dans le temps :  
  
 ![TemporalAnomalyDetection](../../relational-databases/tables/media/temporalanomalydetection.png "TemporalAnomalyDetection")  
  
 En supposant que le nombre de produits achetés varie peut pendant les jours normaux, la requête suivante identifie les valeurs hors norme de singleton ; les échantillons présentent une différence significative (x2) par rapport à leurs voisins immédiats, tandis que les échantillons environnants ne diffèrent pas considérablement (moins de 20 %) :  
  
```  
WITH CTE (ProdId, PrevValue, CurrentValue, NextValue, ValidFrom, ValidTo)  
AS  
    (  
        SELECT   
            ProdId, LAG (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as PrevValue  
            , DailySales, LEAD (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as NextValue   
             , ValidFrom, ValidTo from Product  
        FOR SYSTEM_TIME ALL  
)  
  
SELECT   
    ProdId  
    , PrevValue  
    , CurrentValue  
    , NextValue  
    , ValidFrom  
    , ValidTo  
    , ABS (PrevValue - NextValue) / convert (float, (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END)) as PrevToNextDiff  
    , ABS (CurrentValue - PrevValue) / convert (float, (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END)) as CurrentToPrevDiff  
    , ABS (CurrentValue - NextValue) / convert (float, (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END)) as CurrentToNextDiff  
FROM CTE   
    WHERE   
        ABS (PrevValue - NextValue) / (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END) < 0.2  
            AND ABS (CurrentValue - PrevValue) / (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END) > 2  
            AND ABS (CurrentValue - NextValue) / (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END) > 2;  
```  
  
> [!NOTE]  
>  Cet exemple est intentionnellement simplifié. Dans les scénarios de production, vous utilisez généralement des méthodes statistiques avancées pour identifier les échantillons qui ne suivent pas le modèle commun.  
  
## <a name="slowly-changing-dimensions"></a>Dimensions à variation lente  
 En règle générale, les dimensions d’entreposage de données contiennent des données relativement statiques sur les entités telles que des produits, des clients ou des emplacements géographiques. Toutefois, dans certains scénarios, vous devez également tracer les modifications de données dans des tables de dimension. Étant donné que toute modification de dimensions se produit beaucoup moins fréquemment, de manière imprévisible et en dehors de la planification des mises à jour normales qui s’applique aux tables de faits, ces types de tables de dimension sont appelés dimensions à variation lente.  
  
 Il existe plusieurs catégories de dimensions à variation lente, selon la façon dont l’historique des modifications est conservé :  
  
-   Type 0 : l’historique n’est pas conservé. Les attributs de dimension reflètent les valeurs d’origine.  
  
-   Type 1 : les attributs de dimension reflètent les valeurs les plus récentes (les valeurs précédentes sont remplacées)  
  
-   Type 2 : chaque version de membre de dimension représentée par une ligne distincte dans la table, généralement avec des colonnes qui représentent la période de validité  
  
-   Type 3 : conservation d’un historique limité pour des attributs sélectionnés en utilisant des colonnes supplémentaires dans la même ligne  
  
-   Type 4 : conservation de l’historique dans la table distincte tandis que la table de dimension d’origine conserve les dernières versions des membres de dimension (actuelles)  
  
 Quand vous choisissez la stratégie de dimension à variation lente, il revient à la couche ETL (extraction, transformation et chargement) d’assurer l’exactitude des tables de dimension, ce qui exige généralement beaucoup de code et une maintenance complexe.  
  
 Grâce aux tables temporelles avec version système dans SQL Server 2016, vous pouvez réduire considérablement la complexité de votre code dans la mesure où l’historique des données est conservé automatiquement. La mise en œuvre reposant sur deux tables, les tables temporelles dans SQL Server 2016 sont plus proches de la dimension à variation lente de type 4. Toutefois, étant donné que les requêtes temporelles vous permettent de référencer la table actuelle uniquement, vous pouvez également envisager des tables temporelles dans les environnements où vous prévoyez d’utiliser la dimension à variation lente de type 2.  
  
 Pour convertir votre dimension normale en dimension à variation lente, il vous suffit de créer une dimension ou d’en convertir une en table temporelle avec version système. Si votre table de dimension existante contient des données historiques, créez une table distincte, déplacez-y les données historiques, puis conservez les versions de dimension actuelles (réelles) dans votre table de dimension d’origine. Ensuite, utilisez la syntaxe ALTER TABLE pour convertir votre table de dimension en table temporelle avec version système avec une table de l’historique prédéfinie.  
  
 L’exemple suivant illustre ce processus et suppose que la table de dimension DimLocation possède déjà ValidFrom et ValidTo en tant que colonnes datetime2 remplies par le processus ETL et n’acceptant pas de valeurs NULL :  
  
```  
/*Move “closed” row versions into newly created history table*/  
SELECT * INTO  DimLocationHistory  
    FROM DimLocation  
        WHERE ValidTo < '9999-12-31 23:59:59.99';  
GO  
/*Create clustered columnstore index which is a very good choice in DW scenarios*/  
CREATE CLUSTERED COLUMNSTORE INDEX IX_DimLocationHistory ON DimLocationHistory  
/*Delete previous versions from DimLocation which will become current table in temporal-system-versioning configuration*/  
DELETE FROM DimLocation  
    WHERE ValidTo < '9999-12-31 23:59:59.99';  
/*Add period definition*/  
ALTER TABLE DimLocation ADD PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);  
/*Enable system-versioning and bind histiory table to the DimLocation*/  
ALTER TABLE DimLocation SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DimLocationHistory));  
```  
  
 Notez qu’aucun code supplémentaire n’est nécessaire pour gérer la dimension à variation lente pendant le processus de chargement de l’entrepôt de données, une fois la création effectuée.  
  
 L’illustration suivante montre comment vous pouvez utiliser des tables temporelles dans un scénario simple impliquant 2 dimensions à variation lente (DimLocation et DimProduct) et une table de faits.  
  
 ![TemporalSCD](../../relational-databases/tables/media/temporalscd.png "TemporalSCD")  
  
 Pour utiliser les dimensions à variation lente ci-dessus dans les rapports, vous devez paramétrer l’interrogation de manière efficace. Par exemple, vous souhaiterez calculer le montant total des ventes et le nombre moyen de produits vendus par habitant au cours des six derniers mois.  Notez que les deux métriques impliquent la corrélation de données à partir de la table de faits et des dimensions dont les attributs importants pour l’analyse ont pu évoluer (DimLocation.NumOfCustomers, DimProduct.UnitPrice).  La requête suivante calcule correctement les métriques requises :  
  
```  
DECLARE @now datetime2 = SYSUTCDATETIME()  
DECLARE @sixMonthsAgo datetime2 SET   
    @sixMonthsAgo = DATEADD (month, -12, SYSUTCDATETIME())   
  
SELECT DimProduct_History.ProductId  
   , DimLocation_History.LocationId  
    , SUM(F.Quantity * DimProduct_History.UnitPrice) AS TotalAmount  
    , AVG (F.Quantity/DimLocation_History.NumOfCustomers) AS AverageProductsPerCapita   
FROM FactProductSales F   
/* find corresponding record in SCD history in last 6 months, based on matching fact */  
JOIN DimLocation FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimLocation_History   
    ON DimLocation_History.LocationId = F.LocationId   
        AND F.FactDate BETWEEN DimLocation_History.ValidFrom AND DimLocation_History.ValidTo   
/* find corresponding record in SCD history in last 6 months, based on matching fact */  
JOIN DimProduct FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimProduct_History   
    ON DimProduct_History.ProductId = F.ProductId  
        AND F.FactDate BETWEEN DimProduct_History.ValidFrom AND DimProduct_History.ValidTo   
    WHERE F.FactDate BETWEEN @sixMonthsAgo AND @now   
GROUP BY DimProduct_History.ProductId, DimLocation_History.LocationId ;  
```  
  
 **Éléments à prendre en considération :**  
  
-   L’utilisation de tables temporelles avec version système pour la dimension à variation lente est acceptable si la période de validité calculée selon l’heure de transaction de base de données est appropriée pour votre logique métier. Si vous chargez des données avec un délai important, l’heure de transaction peut ne pas être acceptable.  
  
-   Par défaut, les tables temporelles avec version système n’autorisent pas la modification des données historiques après le chargement (vous pouvez modifier l’historique après avoir défini SYSTEM_VERSIONING sur OFF). Cela peut constituer une limitation dans les cas où les données historiques sont régulièrement modifiées.  
  
-   Les tables temporelles avec version système génèrent une version de ligne à chaque modification de la colonne. Si vous souhaitez supprimer les nouvelles versions à l’occasion d’une modification de colonne spécifique, vous devez intégrer cette limitation à la logique ETL.  
  
-   Si vous prévoyez un nombre important de lignes d’historique dans les tables de dimension à variation lente, envisagez d’utiliser un index columnstore cluster comme option de stockage principal pour la table de l’historique. Ainsi, vous réduisez l’encombrement de la table de l’historique et accélérez vos requêtes analytiques.  
  
## <a name="repairing-row-level-data-corruption"></a>Réparation d’une altération des données au niveau des lignes  
 Vous pouvez vous baser sur les données historiques des tables temporelles avec version système pour rétablir rapidement des lignes individuelles dans tout état précédemment capturé. Cette propriété des tables temporelles est très utile quand vous pouvez localiser les lignes affectées et/ou que vous connaissez l’heure de la modification de données non souhaitée ; vous pouvez ainsi effectuer la réparation très efficacement sans faire appel à des sauvegardes.  
  
 Cette approche présente plusieurs avantages :  
  
-   Vous pouvez contrôler très précisément l’étendue de la réparation. Les enregistrements qui ne sont pas affectés doivent demeurer au dernier état, condition souvent essentielle.  
  
-   L’opération est très efficace et la base de données reste en ligne pour toutes les charges de travail utilisant des données.  
  
-   L’opération de réparation elle-même fait l’objet d’une gestion des versions. Comme vous disposez d’une piste d’audit pour l’opération de réparation elle-même, vous pouvez analyser ce qu’il s’est passé ultérieurement si nécessaire.  
  
 L’action de réparation peut être automatisée relativement facilement. Voici un exemple de code de la procédure stockée qui effectue la réparation des données de la table Employee utilisée dans le scénario d’audit de données.  
  
```  
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecord;  
GO  
  
CREATE PROCEDURE sp_RepairEmployeeRecord   
    @EmployeeID INT,  
    @versionNumber INT = 1  
AS  
  
;WITH History  
AS  
(  
        /* Order historical rows by tehir age in DESC order*/  
        SELECT ROW_NUMBER () OVER (PARTITION BY EmployeeID ORDER BY [ValidTo] DESC) AS RN, *  
        FROM Employee FOR SYSTEM_TIME ALL WHERE YEAR (ValidTo) < 9999 AND Employee.EmployeeID = @EmployeeID  
)  
  
/*Update current row by using N-th row version from history (default is 1 - i.e. last version)*/  
UPDATE Employee   
    SET [Position] = H.[Position], [Department] = H.Department, [Address] = H.[Address], AnnualSalary = H.AnnualSalary  
    FROM Employee E JOIN History H ON E.EmployeeID = H.EmployeeID AND RN = @versionNumber  
    WHERE E.EmployeeID = @EmployeeID  
  
```  
  
 Cette procédure stockée accepte @EmployeeID et @versionNumber en tant que paramètres d’entrée. Cette procédure restaure par défaut l’état de la ligne à la dernière version de l’historique (@versionNumber = 1).  
  
 L’illustration suivante montre l’état de la ligne avant et après l’appel de la procédure. Le rectangle rouge indique la version de ligne actuelle qui est incorrecte, tandis que le rectangle vert indique la version correcte de l’historique.  
  
 ![TemporalUsageRepair1](../../relational-databases/tables/media/temporalusagerepair1.png "TemporalUsageRepair1")  
  
```  
EXEC sp_RepairEmployeeRecord @EmployeeID = 1, @versionNumber = 1  
```  
  
 ![TemporalUsageRepair2](../../relational-databases/tables/media/temporalusagerepair2.png "TemporalUsageRepair2")  
  
 Cette procédure stockée de réparation peut être définie pour accepter un horodatage exact au lieu de la version de ligne. Dans ce cas, elle restaure la ligne à n’importe quelle version qui était active pour le point dans le temps fourni (autrement dit, AS OF point dans le temps).  
  
```  
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecordAsOf;  
GO  
  
CREATE PROCEDURE sp_RepairEmployeeRecordAsOf   
    @EmployeeID INT,  
    @asOf datetime2  
AS  
  
/*Update current row to the state that was actual AS OF provided date*/  
UPDATE Employee   
    SET [Position] = History.[Position], [Department] = History.Department, [Address] = History.[Address], AnnualSalary = History.AnnualSalary  
    FROM Employee AS E JOIN Employee FOR SYSTEM_TIME AS OF @asOf AS History  ON E.EmployeeID = History.EmployeeID  
    WHERE E.EmployeeID = @EmployeeID  
  
```  
  
 Pour le même exemple de données, l’image suivante illustre le scénario de réparation avec une condition de temps. Sont mis en valeur le paramètre @asOf, une ligne sélectionnée dans l’historique qui était réelle au point dans le temps fourni et la nouvelle version de ligne dans la table actuelle après l’opération de réparation :  
  
 ![TemporalUsageRepair3](../../relational-databases/tables/media/temporalusagerepair3.png "TemporalUsageRepair3")  
  
 La correction des données peut être intégrée au processus de chargement automatisé des données dans les systèmes d’entreposage de données et de rapports.  
Si une valeur qui vient d’être mise à jour n’est pas correcte, dans de nombreux scénarios, restaurer la version précédente à partir de l’historique peut suffire. Le diagramme suivant montre comment ce processus peut être automatisé :  
  
 ![TemporalUsageRepair4](../../relational-databases/tables/media/temporalusagerepair4.png "TemporalUsageRepair4")  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)   
 [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md)   
 [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)   
 [Sécurité de la table temporelle](../../relational-databases/tables/temporal-table-security.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)  
  
  
