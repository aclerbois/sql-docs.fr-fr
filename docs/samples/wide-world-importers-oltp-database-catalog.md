---
title: Catalogue de base de données WideWorldImporters OLTP - SQL | Documents Microsoft
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
ms.openlocfilehash: 35228d6773e576b2d8b062c94aa8797d07f00809
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wideworldimporters-database-catalog"></a>Catalogue de base de données WideWorldImporters
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
La base de données WideWorldImporters contient toutes les les informations sur les transactions et données quotidiennes pour les achats et, ainsi que des données de capteur des véhicules et chambres froides.

## <a name="schemas"></a>Schémas

WideWorldImporters utilise les schémas à différentes fins, telles que le stockage des données, en définissant comment les utilisateurs peuvent accéder aux données et en fournissant des objets pour l’intégration et de développement de l’entrepôt de données.

### <a name="data-schemas"></a>Schémas de données

Ces schémas contiennent les données. Un nombre de tables requises par tous les autres schémas et est situé dans le schéma de l’Application.

|Schéma| Description|
|-----------------------------|---------------------|
|Application|Les utilisateurs de l’application, des contacts et des paramètres. Il contient également des tables de référence avec des données qui sont utilisées par plusieurs schémas|
|Purchasing|Élément du stock achats à partir des fournisseurs et des détails sur les fournisseurs.|  
|Ventes|Ventes d’articles pour les particuliers et les détails des clients et aux ventes de personnes de stock. |  
|Warehouse|Inventaire de l’élément du stock et les transactions.|  

### <a name="secure-access-schemas"></a>Schémas d’accès sécurisé

Ces schémas sont utilisés pour les applications externes qui ne sont pas autorisées à accéder directement aux tables de données. Ils contiennent des vues et procédures stockées utilisées par des applications externes.

|Schéma| Description|
|-----------------------------|---------------------|
|Site Web|Tous les accès à la base de données à partir du site Web d’entreprise sont via ce schéma.|
|Rapports|Tous les accès à la base de données à partir de rapports de Reporting Services sont à ce schéma.|
|Power BI|Tous les accès à la base de données dans les tableaux de bord Power BI via la passerelle d’entreprise sont via ce schéma.|

Notez que les rapports Power BI et les schémas ne sont pas utilisés dans la version initiale de la base de données. Toutefois, les exemples de toutes les Reporting Services et Power BI reposant sur cette base de données sont encouragés à utiliser ces schémas.

### <a name="development-schemas"></a>Schémas de développement

Schémas spécial

|Schéma| Description|
|-----------------------------|---------------------|
|Intégration|Objets et procédures requises pour l’intégration de l’entrepôt de données (autrement dit, migrer les données à la base de données WideWorldImportersDW).|
|Séquences|Contient des séquences utilisées par toutes les tables dans l’application.|

## <a name="tables"></a>Tables

Toutes les tables dans la base de données sont dans les schémas de données.

### <a name="application-schema"></a>Schéma d’application

Détails des paramètres et des personnes (utilisateurs et contacts), ainsi que des tables de référence communes (communes à plusieurs autres schémas).

|Table| Description|
|-----------------------------|---------------------|
|SystemParameters|Contient les paramètres configurables à l’échelle du système.|
|Personnes|Contient les noms d’utilisateur, informations de contact, pour tous ceux qui utilisent l’application et pour les personnes qui Wide World Importers porte sur les organisations de client. Cela inclut le personnel, les clients, fournisseurs et autres contacts. Pour les personnes qui ont reçu l’autorisation d’utiliser le système ou un site Web, les informations incluent les détails de connexion.|
|Villes|Il existe plusieurs adresses stockées dans le système, pour les personnes, adresses de livraison client organisation collecte des adresses à des fournisseurs, etc. Chaque fois qu’une adresse est stockée, il existe une référence à une ville dans cette table. Il existe également un emplacement spatial de chaque ville.|
|StateProvinces|Villes font partie des États ou provinces. Cette table comporte les détails d’eux, y compris les données spatiales décrivant les limites de chaque pays ou région.|
|Pays|Les États ou Provinces font partie du pays. Cette table comporte les détails d’eux, y compris les données spatiales décrivant les limites de chaque pays.|
|DeliveryMethods|Options pour la remise des articles en stock (par exemple, camion/van, post, collecte, expédition par courrier, etc.)|
|PaymentMethods|Options de paiement (par exemple, espèces, chèque, EFT, etc.)|
|TransactionTypes|Types de client, fournisseur ou les transactions de stock (par exemple, facture, note de crédit, etc.)|

### <a name="purchasing-schema"></a>Schéma d’achat

Détails des fournisseurs et des achats de stock.

|Table| Description|
|-----------------------------|---------------------|
|Suppliers|Table de l’entité principale pour les fournisseurs (organisations)|
|SupplierCategories|Catégories de fournisseurs (par exemple, attrapes, toys, habillement, emballage, etc.).|
|SupplierTransactions|Toutes les transactions financières qui sont liés au fournisseur (factures, paiements)|
|PurchaseOrders|Détails des bons de commande fournisseur|
|PurchaseOrderLines|Lignes de détails de fournisseur bons de commande|

 
### <a name="sales-schema"></a>Schéma de vente

Détails des clients, les commerciaux et des ventes de l’élément du stock.

|Table| Description|
|-----------------------------|---------------------|
|Clients (Customers)|Tables d’entité principale pour les clients (organisations ou aux individus)|
|CustomerCategories|Catégories pour les clients (c.-à-d. nouveauté magasins, grande distribution, etc.)|
|BuyingGroups|Organisations clientes peuvent faire partie de groupes qui exercer une plus grande puissance d’achat|
|CustomerTransactions|Toutes les transactions financières qui sont liés au client (factures, paiements)|
|SpecialDeals|Une tarification spéciale. Cela peut inclure le prix fixes, remise en dollars ou le pourcentage de remise.|
|Orders|Détail des commandes client|
|OrderLines|Lignes de détails de commandes client|
|Factures|Détails des factures client|
|InvoiceLines|Lignes de détails de factures client|

### <a name="warehouse-schema"></a>Schéma de l’entrepôt

Détails de stock, leurs exploitations et aux transactions.

|Table| Description|
|-----------------------------|---------------------|
|StockItems|Table d’entité principale pour les éléments de stock|
|StockItemHoldings|Colonnes non temporelle pour les articles de stock. Ce sont les colonnes fréquemment mise à jour.|
|StockGroups|Groupes pour le classement des éléments de stock (par exemple, attrapes, toys, attrapes alimentaires, etc.)|
|StockItemStockGroups|Les éléments de stock sont dans lequel stock groupes (plusieurs à plusieurs)|
|Couleurs|Articles en stock peuvent avoir (éventuellement) des couleurs|
|PackageTypes|Méthodes des articles en stock peut être empaqueté (par exemple, zone, carton, palettes, kg, etc.|
|StockItemTransactions|Transactions couvrant tous les mouvements de tous les éléments de stock (réception, la vente, d’annulation)|
|VehicleTemperatures|Températures régulièrement enregistrées ménagers du véhicule|
|ColdRoomTemperatures|Enregistrée régulièrement les températures ménagers de salle à froid|


## <a name="design-considerations"></a>Considérations de conception

Conception de base de données est subjective et il n’existe aucun moyen de concevoir une base de données ou non. Les schémas et les tables dans cette base de données affichent d’informations sur la façon dont vous pouvez concevoir votre propre base de données.

### <a name="schema-design"></a>Conception de schéma

WideWorldImporters utilise un petit nombre de schémas afin qu’il soit facile à comprendre le système de base de données et de décrire les principes de base de données.  

Dans la mesure du possible, la base de données collocates des tables qui sont fréquemment interrogés dans le même schéma afin de réduire la complexité de la jointure.

Le schéma de base de données a été généré par le code basé sur une série de tables de métadonnées dans une autre base de données WWI_Preparation. Ainsi, WideWorldImporters un niveau très élevé de cohérence de la conception, la cohérence d’affectation de noms et d’exhaustivité. Pour plus d’informations sur la façon dont le schéma a été généré, consultez le code source : [wide world-importateurs/wwi-base de données-scripts](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

### <a name="table-design"></a>Création de table

- Toutes les tables ont des clés primaires de colonne unique par souci de simplicité de jointure.
- Tous les schémas, tables, colonnes, index et contraintes de validation ont une Description de propriété qui peut être utilisée pour identifier l’objectif de l’objet ou de la colonne étendue. Tables optimisées en mémoire sont une exception à cela, car ils ne prennent actuellement en charge les propriétés étendues.
- Toutes les clés étrangères sont automatiquement indexés sauf s’il existe un autre index non cluster qui a le même composant de gauche.
- Numérotation automatique dans les tables est basée sur des séquences. Ces séquences sont plus faciles de travailler avec des serveurs liés et des environnements similaires à des colonnes d’identité. Tables optimisées en mémoire utilisent des colonnes d’identité, car ils ne prennent pas en charge dans SQL Server 2016.
- Une seule séquence (TransactionID) est utilisée pour ces tables : CustomerTransactions, SupplierTransactions et StockItemTransactions. Cela montre comment un ensemble de tables peut avoir une seule séquence.
- Certaines colonnes ont des valeurs par défaut appropriées.

### <a name="security-schemas"></a>Schémas de sécurité

Pour la sécurité, WideWorldImporters n’autorise pas les applications externes d’accéder directement à des schémas de données. Pour isoler l’accès, WideWorldImporters utilise les schémas d’accès de sécurité qui ne contiennent pas de données, mais qui contiennent des vues et procédures stockées. Des applications externes permet de récupérer les données qu’ils sont autorisés à afficher les schémas de sécurité.  De cette manière, les utilisateurs peuvent uniquement exécuter les procédures stockées et vues dans les schémas d’accès sécurisé

Par exemple, cet exemple inclut des tableaux de bord Power BI. Une application externe accède à ces tableaux de bord Power BI à partir de la passerelle Power BI en tant qu’utilisateur qui a l’autorisation en lecture seule sur le schéma de Power BI.  Pour une autorisation en lecture seule, l’utilisateur doit uniquement l’autorisation SELECT et EXECUTE sur le schéma de Power BI. Un administrateur de base de données au WWI affecte ces autorisations en fonction des besoins.

## <a name="stored-procedures"></a>Procédures stockées

Les procédures stockées sont organisées dans des schémas. La plupart des schémas est utilisée pour la configuration ou à des fins d’exemple.

Le `Website` schéma contient les procédures stockées qui peuvent être utilisées par un serveur Web frontal.

Le `Reports` et `PowerBI` schémas sont conçus pour reporting services et à des fins de Power BI. Toutes les extensions de l’exemple sont encouragées à utiliser ces schémas à des fins de création de rapports.

### <a name="website-schema"></a>Schéma de site Web

Voici les procédures utilisées par une application cliente, par exemple un serveur Web frontal.

|Procédure|Fonction|
|-----------------------------|---------------------|
|ActivateWebsiteLogon|Permet à une personne (à partir de `Application.People`) d’accéder au site Web.|
|ChangePassword|Modifie un mot de passe (pour les utilisateurs qui ne sont pas à l’aide des mécanismes d’authentification externe).|
|InsertCustomerOrders|Permet l’insertion d’une ou plusieurs commandes client (y compris les lignes de commande).|
|InvoiceCustomerOrders|Accepte une liste de commandes à facturer et traite les factures.|
|RecordColdRoomTemperatures|Utilise une liste de données de capteur, comme un paramètre table (TVP) et applique les données à le `Warehouse.ColdRoomTemperatures` table temporelle.|
|RecordVehicleTemperature|Prend un tableau JSON et l’utilise pour mettre à jour `Warehouse.VehicleTemperatures`.|
|SearchForCustomers|Recherche pour les clients par nom ou une partie du nom (le nom de la personne ou le nom de société).|
|SearchForPeople|Recherches de personnes par nom ou une partie du nom.|
|SearchForStockItems|Recherche des éléments de stock par nom ou une partie du nom ou de commentaires de marketing.|
|SearchForStockItemsByTags|Recherche des éléments de stock par des balises.|
|SearchForSuppliers|Recherche de fournisseurs par nom ou une partie du nom (le nom de la personne ou le nom de société).|

### <a name="integration-schema"></a>Schéma de l’intégration

Les procédures stockées dans ce schéma sont utilisés par le processus ETL. Ils obtiennent les données nécessaires à partir des tables différentes pour l’intervalle de temps requis par le [package ETL](wide-world-importers-perform-etl.md).

### <a name="dataloadsimulation-schema"></a>Schéma de DataLoadSimulation

Simule une charge de travail qui insère des ventes et les achats. La procédure stockée principale est `PopulateDataToCurrentDate`, qui est utilisé pour insérer des exemples de données jusqu'à la date actuelle.

|Procédure|Fonction|
|-----------------------------|---------------------|
|Configuration_ApplyDataLoadSimulationProcedures|Recrée les procédures nécessaires pour les données de simulation de charge. Cela est nécessaire pour mettre les données jusqu'à la date actuelle.|
|Configuration_RemoveDataLoadSimulationProcedures|Cette opération supprime les procédures à nouveau une fois la simulation de données est terminée.|
|DeactiveTemporalTablesBeforeDataLoad|Supprime la nature temporelle de toutes les tables temporelles et le cas échéant, applique un déclencheur afin que les modifications peuvent être effectuées comme si elles ont été appliquées à une date antérieure, pas dans les tables temporelles de sys.|
|PopulateDataToCurrentDate|Utilisé pour afficher les données jusqu'à la date actuelle. Doit être exécutée avant d’autres options de configuration après la restauration de la base de données à partir d’une sauvegarde initiale.|
|ReactivateTemporalTablesAfterDataLoad|Rétablit les tables temporelles, y compris la vérification de la cohérence des données. (Supprime les déclencheurs associés).|


### <a name="application-schema"></a>Schéma d’application

Ces procédures sont utilisées pour configurer l’exemple. Ils sont utilisés pour appliquer les fonctionnalités d’édition entreprise pour la version d’édition standard de l’échantillon et également d’ajouter un audit et l’indexation de texte intégral.

|Procédure|Fonction|
|-----------------------------|---------------------|
|AddRoleMemberIfNonexistant|Ajoute un membre à un rôle si le membre n’est pas déjà dans le rôle|
|Configuration_ApplyAuditing|Ajoute l’audit. L’audit de serveur est appliqué pour les bases de données standard edition ; l’audit de base de données supplémentaire est ajouté pour l’édition enterprise.|
|Configuration_ApplyColumnstoreIndexing|S’applique columnstore l’indexation à `Sales.OrderLines` et `Sales.InvoiceLines` et répertorie à nouveau en conséquence.|
|Configuration_ApplyFullTextIndexing|S’applique à des index de texte intégral `Application.People`, `Sales.Customers`, `Purchasing.Suppliers`, et `Warehouse.StockItems`. Remplace `Website.SearchForPeople`, `Website.SearchForSuppliers`, `Website.SearchForCustomers`, `Website.SearchForStockItems`, `Website.SearchForStockItemsByTags` avec les procédures de remplacement qui utilisent l’indexation de texte intégral.|
|Configuration_ApplyPartitioning|Applique le partitionnement de table pour `Sales.CustomerTransactions and `'Purchasing.SupplierTransactions et réorganise les index pour satisfaire.|
|Configuration_ApplyRowLevelSecurity|Applique la sécurité au niveau des lignes de filtrer les clients par montant des ventes territoire les rôles associés.|
|Configuration_ConfigureForEnterpriseEdition|S’applique columnstore l’indexation de recherche en texte intégral, en mémoire, polybase et le partitionnement.|
|Configuration_EnableInMemory|Ajoute un groupe de fichiers mémoire optimisé (lorsque ne fonctionne ne pas dans Azure), remplace `Warehouse.ColdRoomTemperatures`, `Warehouse.VehicleTemperatures` avec les équivalents en mémoire et la migration des données, recrée le `Website.OrderIDList`, `Website.OrderList`, `Website.OrderLineList`, `Website.SensorDataList` types de tables optimisées en mémoire correspondants, supprime et recrée les procédures `Website.InvoiceCustomerOrders`, `Website.InsertCustomerOrders`, et `Website.RecordColdRoomTemperatures` qui utilise ces types de table.|
|Configuration_RemoveAuditing|Supprime la configuration de l’audit.|
|Configuration_RemoveRowLevelSecurity|Supprime la configuration de sécurité de niveau ligne (cela est nécessaire pour les modifications apportées aux tables associées).|
|CreateRoleIfNonExistant|Crée un rôle de base de données s’il n’existe pas.|


### <a name="sequences-schema"></a>Schéma de séquences

Procédures pour configurer les séquences dans la base de données.

|Procédure|Fonction|
|-----------------------------|---------------------|
|ReseedAllSequences|Appelle la procédure ReseedSequenceBeyondTableValue pour toutes les séquences.|
|ReseedSequenceBeyondTableValue|Utilisée pour repositionner la valeur de la séquence suivante au-delà de la valeur dans une table qui utilise la même séquence. (Par exemple, d’un DBCC CHECKIDENT pour équivalent de colonnes d’identité pour les séquences, mais potentiellement plusieurs tables).|
