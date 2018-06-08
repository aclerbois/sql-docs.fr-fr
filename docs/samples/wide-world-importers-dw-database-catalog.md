---
title: Catalogue de base de données WideWorldImporters OLAP - SQL | Documents Microsoft
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7b4533ec0667bc59317ba213578db7301c15b241
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wideworldimportersdw-database-catalog"></a>Catalogue de base de données WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Explications des schémas, tables et procédures stockées dans la base de données WideWorldImportersDW. 

La base de données WideWorldImportersDW est utilisé pour l’entreposage des données et de traitement analytique. Les données transactionnelles sur les ventes et les achats sont générées dans la base de données WideWorldImporters et chargées dans la base de données WideWorldImportersDW à l’aide un **processus ETL quotidien**.

Les données de WideWorldImportersDW reflète ainsi les données de WideWorldImporters, mais les tables sont organisées différemment. Alors que WideWorldImporters a un schéma normalisé traditionnel, WideWorldImportersDW utilise le [schéma en étoile](https://wikipedia.org/wiki/Star_schema) approche de sa conception de la table. Outre les tables de faits et de dimension, la base de données inclut un nombre de tables intermédiaires qui sont utilisés dans le processus ETL.

## <a name="schemas"></a>Schémas

Les différents types de tables sont organisées dans trois schémas.

|Schéma| Description|
|-----------------------------|---------------------|
|Dimension|Tables de dimension.|
|Fait|Tables de faits.|  
|Intégration|Les tables intermédiaires et autres objets nécessaires pour ETL.|  

## <a name="tables"></a>Tables

Les tables de dimension et de faits sont répertoriées ci-dessous. Les tables dans le schéma d’intégration sont utilisés uniquement pour le processus ETL et ne sont pas répertoriées.

### <a name="dimension-tables"></a>Tables de dimension

WideWorldImportersDW a des tables de dimension suivantes. La description inclut la relation avec les tables source dans la base de données WideWorldImporters.

|Table|Tables sources|
|-----------------------------|---------------------|
|Ville|`Application.Cities`, `Application.StateProvinces`, `Application.Countries`.|
|Customer|`Sales.Customers`, `Sales.BuyingGroups`, `Sales.CustomerCategories`.|
|Date|Nouvelle table avec des informations sur les dates, y compris les exercice (selon le 1er novembre de démarrage pour l’exercice).|
|Employee|`Application.People`.|
|StockItem|`Warehouse.StockItems`, `Warehouse.Colors`, `Warehouse.PackageType`.|
|Fournisseur|`Purchasing.Suppliers`, `Purchasing.SupplierCategories`.|
|PaymentMethod|`Application.PaymentMethods`.|
|TransactionType|`Application.TransactionTypes`.|

### <a name="fact-tables"></a>Tables de faits

WideWorldImportersDW contient les tables de faits suivants. La description inclut la relation avec les tables source dans la base de données WideWorldImporters, ainsi que les classes de chaque table de faits est généralement utilisé avec les requêtes/reporting analytique.

|Table|Tables sources|Exemple Analytique|
|-----------------------------|---------------------|---------------------|
|JSON|`Sales.Orders`et`Sales.OrderLines`|Ventes personnes, la productivité de sélecteur/compression et sur le temps pour prélever des commandes. En outre, faible stocks situations permettant de commandes en différé.|
|Vente|`Sales.Invoices`et`Sales.InvoiceLines`|Dates de ventes, les dates de livraison, rentabilité au fil du temps, rentabilité par vendeur.|
|Achat|`Purchasing.PurchaseOrderLines`|Délais réel vs attendu|
|Transaction|`Sales.CustomerTransactions`et`Purchasing.SupplierTransactions`|Dates de finalisation vs problème dates et les montants de mesure.|
|Déplacement des|`Warehouse.StockTransactions`|Mouvements au fil du temps.|
|Actions|`Warehouse.StockItemHoldings`|Les niveaux de stock disponible et la valeur.|

## <a name="stored-procedures"></a>Procédures stockées

Les procédures stockées sont utilisées principalement pour le processus ETL et de fins de configuration.

Toutes les extensions de l’exemple sont encouragées à utiliser le `Reports` schéma pour les rapports Reporting Services et le `PowerBI` schéma pour l’accès à Power BI.

### <a name="application-schema"></a>Schéma d’application

Ces procédures sont utilisées pour configurer l’exemple. Ils sont utilisés pour appliquer les fonctionnalités d’édition entreprise vers la version de l’édition standard de l’exemple, ajoutez PolyBase et reseed ETL.

|Procédure|Fonction|
|-----------------------------|---------------------|
|Configuration_ApplyPartitionedColumnstoreIndexing|S’applique à la fois le partitionnement et columnstore index pour les tables de faits.|
|Configuration_ConfigureForEnterpriseEdition|Applique le partitionnement, columnstore d’indexation et en mémoire.|
|Configuration_EnableInMemory|Remplace les tables intermédiaires de l’intégration avec les tables mémoire optimisées SCHEMA_ONLY afin d’améliorer les performances ETL.|
|Configuration_ApplyPolybase|Configure une source de données externe, le format de fichier et la table.|
|Configuration_PopulateLargeSaleTable|Applique les modifications d’édition entreprise, puis remplit une plus grande quantité de données pour l’année 2012 comme historique supplémentaire.|
|Configuration_ReseedETL|Supprime les données existantes et redémarre les semences ETL. Cela permet de remplir à nouveau la base de données OLAP pour faire correspondre les lignes mises à jour dans la base de données OLTP.|

### <a name="integration-schema"></a>Schéma de l’intégration

Les procédures utilisées dans le processus ETL se répartissent dans ces catégories :
- Procédures d’assistance pour le package ETL - Get * de toutes les procédures.
- Les procédures utilisées par le package ETL pour la migration des données intermédiaires dans les tables d’entrepôt de données - toutes les procédures de migration *.
- `PopulateDateDimensionForYear` -Prend une année et garantit que toutes les dates de cette année sont renseignées dans la `Dimension.Date` table.

### <a name="sequences-schema"></a>Schéma de séquences

Procédures pour configurer les séquences dans la base de données.

|Procédure|Fonction|
|-----------------------------|---------------------|
|ReseedAllSequences|Appelle la procédure `ReseedSequenceBeyondTableValue` pour toutes les séquences.|
|ReseedSequenceBeyondTableValue|Utilisée pour repositionner la valeur de la séquence suivante au-delà de la valeur dans une table qui utilise la même séquence. (Comme un `DBCC CHECKIDENT` pour équivalent de colonnes d’identité pour les séquences, mais potentiellement plusieurs tables.)|
