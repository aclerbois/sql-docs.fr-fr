---
title: Requêtes PolyBase | Microsoft Docs
ms.custom: ''
ms.date: 12/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: polybase
ms.reviewer: ''
ms.suite: sql
ms.technology: polybase
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
caps.latest.revision: 18
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 21c4f31e5467096ae37e4e3b2634c65065236e1a
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="polybase-queries"></a>Requêtes PolyBase
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cet article donne des exemples de requêtes qui utilisent la fonctionnalité [PolyBase](../../relational-databases/polybase/polybase-guide.md) de SQL Server ( à partir de la version 2016). Avant d’utiliser ces exemples, vous devez également comprendre les instructions T-SQL nécessaires pour installer PolyBase (voir [Objets T-SQL PolyBase](../../relational-databases/polybase/polybase-t-sql-objects.md)).
  
## <a name="queries"></a>Requêtes  
 Exécutez des instructions Transact-SQL sur les tables externes ou utilisez des outils d’aide à la décision pour interroger des tables externes.
  
## <a name="select-from-external-table"></a>Requête SELECT sur une table externe  
 Requête simple qui retourne des données à partir d’une table externe définie.  
  
```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```
  
 Requête simple qui inclut un prédicat.

```
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>Jointure de tables externes à des tables locales avec JOIN

```
SELECT InsuranceCustomers.FirstName,   
                           InsuranceCustomers.LastName,   
                           SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  
  
## <a name="pushdown-computation-to-hadoop"></a>Calcul de poussée vers le bas dans Hadoop

Des variations de poussée vers le bas sont affichées ici.

### <a name="pushdown-for-selecting-a-subset-of-rows"></a>Poussée vers le bas pour sélectionner un sous-ensemble de lignes

Utilisez une poussée vers le bas de prédicat pour améliorer les performances d’une requête qui sélectionne un sous-ensemble de lignes d’une table externe.

Dans cet exemple, SQL Server 2016 lance un travail MapReduce pour récupérer les lignes qui correspondent au prédicat `customer.account_balance < 200000` sur Hadoop. Comme la requête peut s’effectuer correctement sans analyser toutes les lignes de la table, seules les celles qui répondent aux critères du prédicat sont copiées sur SQL Server. Cette opération permet de gagner un temps considérable et nécessite moins d’espace de stockage temporaire quand le nombre de soldes clients inférieur à < 200000 est faible par rapport au nombre de clients ayant des soldes de compte supérieurs à 200000.

```
SELECT * FROM customer WHERE customer.account_balance < 200000
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="pushdown-for-selecting-a-subset-of-columns"></a>Poussée vers le bas pour sélectionner un sous-ensemble de colonnes

Utilisez une poussée vers le bas de prédicat pour améliorer les performances d’une requête qui sélectionne un sous-ensemble de colonnes d’une table externe.

Dans cette requête, SQL Server lance une tâche Map/Reduce pour prétraiter le fichier texte délimité Hadoop afin que seules les données pour les deux colonnes, customer.name et customer.zip_code, soient copiées dans SQL Server PDW.

```
SELECT customer.name, customer.zip_code FROM customer WHERE customer.account_balance < 200000
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>Poussée vers le bas pour les opérateurs et expressions de base

SQL Server autorise les opérateurs et expressions de base suivants pour une poussée vers le bas de prédicat.

+ Opérateurs de comparaison binaire (\<, >, =, !=, <>, >=, <=) pour les valeurs numériques, d’heure et de date.

+ Opérateurs arithmétiques (+, -, *, /, %).

+ Opérateurs logiques (AND, OR).

+ Opérateurs unaires (NOT, IS NULL, IS NOT NULL).

Les opérateurs BETWEEN, NOT, IN et LIKE peuvent être poussés vers le bas. Le comportement réel dépend de la façon dont l’optimiseur de requête réécrit les expressions des opérateurs sous la forme d’une série d’instructions qui utilisent des opérateurs relationnels de base.

La requête de cet exemple comporte plusieurs prédicats pouvant être refoulés vers Hadoop. SQL Server peut placer des travaux MapReduce dans Hadoop pour exécuter le prédicat `customer.account_balance <= 200000`. L’expression `BETWEEN 92656 and 92677` est également constituée d’opérations binaires et logiques qui peuvent être empilées vers Hadoop. Le **AND** logique dans `customer.account_balance and customer.zipcode` est une expression finale.

Étant donnée cette combinaison de prédicats, les travaux MapReduce peuvent exécuter l’ensemble de la clause WHERE. Seules les données qui répondent aux critères SELECT seront recopiées dans SQL Server PDW.

```
SELECT * FROM customer WHERE customer.account_balance <= 200000 AND customer.zipcode BETWEEN 92656 AND 92677
```

### <a name="force-pushdown"></a>Forcer la poussée vers le bas

```
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (FORCE EXTERNALPUSHDOWN);
```

### <a name="disable-pushdown"></a>Désactiver la poussée vers le bas

```
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (DISABLE EXTERNALPUSHDOWN);
```

## <a name="import-data"></a>Importer des données

Importez des données de Hadoop ou d’Azure Storage dans SQL Server à des fins de stockage permanent. Utilisez SELECT INTO pour importer des données référencées par une table externe, en vue de leur stockage permanent dans SQL Server. Créez une table relationnelle à la volée, puis créez un index columnstore en haut de la table.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```

## <a name="export-data"></a>Exporter des données

Exportez des données de SQL Server vers Hadoop ou le Stockage Azure. 

Tout d’abord, activez la fonctionnalité d’exportation en définissant la valeur `sp_configure` de l’option « autoriser l’exportation polybase » sur 1. Créez ensuite une table externe pointant vers le répertoire de destination. L’instruction CREATE EXTERNAL TABLE crée le répertoire de destination, s’il n’existe pas encore. Utilisez ensuite l’instruction INSERT INTO pour exporter les données d’une table SQL Server locale dans la source de données externe. 

Les résultats de l’instruction SELECT sont exportés dans un fichier au format et à l’emplacement spécifiés. Les fichiers externes sont nommés *QueryID_date_time_ID.format*, où *ID* est un identificateur incrémentiel et *format* est le format des données exportées. Exemple de nom de fichier : QID776_20160130_182739_0.orc.


> [!NOTE]
> En cas d’exportation de données vers Hadoop ou le Stockage Blob Azure via PolyBase, seules les données sont exportées, et non les noms de colonnes (métadonnées), comme le définit la commande CREATE EXTERNAL TABLE.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Nouveaux affichages catalogue

Les nouveaux affichages catalogue suivants montrent des ressources externes.
  
```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```
  
 Déterminer si une table est une table externe à l’aide de `is_external`  
  
```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  
  
## <a name="next-steps"></a>Étapes suivantes  

Pour en savoir plus sur la résolution des problèmes, consultez [Résolution des problèmes de Polybase](../../relational-databases/polybase/polybase-troubleshooting.md).
