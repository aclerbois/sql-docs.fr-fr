---
title: Tables et index partitionnés | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: partitions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-partition
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- partitioned tables [SQL Server], about partitioned tables
- partitioned indexes [SQL Server], architecture
- partitioned tables [SQL Server], architecture
- partitioned indexes [SQL Server], about partitioned indexes
ms.assetid: cc5bf181-18a0-44d5-8bd7-8060d227c927
caps.latest.revision: 48
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: bd4402ca30b3f99e8470282aea9fa46a4667cb05
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708447"
---
# <a name="partitioned-tables-and-indexes"></a>Partitioned Tables and Indexes
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le partitionnement des tables et des index. Les données des tables et des index partitionnés sont divisées en unités qui peuvent être réparties sur plusieurs groupes de fichiers d'une base de données. Les données sont partitionnées horizontalement, de sorte que les groupes de lignes sont mappés à des partitions individuelles. Toutes les partitions d'un index ou d'une table unique doivent résider dans la même base de données. La table ou l'index est traité en tant qu'entité logique unique lorsque des requêtes ou des mises à jour sont effectuées sur les données. Dans les versions antérieures à [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)]SP1, les tables et les index partitionnés n’étaient pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] prend en charge jusqu'à 15 000 partitions par défaut. Dans les versions antérieures à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le nombre de partitions était limité à 1 000 par défaut. Sur les systèmes x86, la création d'une table ou d'un index contenant plus de 1 000 partitions est possible, mais n'est pas prise en charge.  
  
## <a name="benefits-of-partitioning"></a>Avantages du partitionnement  
 Le partitionnement des tables ou des index peut offrir les avantages suivants en matière de gestion et de performances.  
  
-   Vous pouvez créer des sous-ensembles de données et y accéder facilement et efficacement, tout en conservant l'intégrité d'une collection de données. Par exemple, une opération telle que le chargement des données d'un système OLTP vers un système OLAP ne prend que quelques secondes au lieu des minutes et des heures qu'elle exige lorsque les données ne sont pas partitionnées.  
  
-   Vous pouvez effectuer des opérations de maintenance sur une ou plusieurs partitions plus rapidement. Les opérations sont plus efficaces car elles ne ciblent que ces sous-ensembles de données, au lieu de la totalité de la table. Par exemple, vous pouvez choisir de compresser les données dans une ou plusieurs partitions ou de reconstruire une ou plusieurs partitions d'un index.  
  
-   Suivant les types de requêtes fréquemment exécutées et la configuration matérielle, vous pouvez améliorer les performances des requêtes. Par exemple, l'optimiseur de requête peut traiter les requêtes d'équijointure entre plusieurs tables partitionnées plus rapidement lorsque les colonnes de partitionnement dans les tables sont identiques, car les partitions elles-mêmes peuvent être jointes.  
  
     Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trie des données pour des opérations d'entrée/sortie, il trie d'abord les données par partition. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accède à un lecteur à la fois, ce qui peut réduire les performances. Pour améliorer les performances de tri des données, distribuez les fichiers de données des partitions sur plusieurs disques en définissant un volume RAID. Ainsi, bien que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trie toujours les données par partition, il peut accéder à tous les lecteurs de chaque partition au même moment.  
  
     De plus, il est possible d'améliorer les performances en activant l'escalade de verrous au niveau de la partition plutôt qu'au niveau de la table entière. Cela peut réduire les conflits de verrouillage de la table.  
  
## <a name="components-and-concepts"></a>Composants et concepts  
 Les termes suivants s'appliquent aux partitionnement de table et d'index.  
  
 Fonction de partition  
 Objet de base de données qui définit comment les lignes d'une table ou d'un index sont mappées à un ensemble de partitions en fonction des valeurs de certaines colonnes, appelées « colonnes de partitionnement ». Autrement dit, la fonction de partition définit le nombre de partitions qu'aura la table, ainsi que la façon dont les limites des partitions sont définies. Prenons l’exemple d’une table qui contient des données de commande client ; vous pouvez partitionner la table en douze partitions (mensuellement) en fonction d’une colonne **datetime** , telle qu’une date de vente.  
  
 Schéma de partition  
 Objet de base de données qui mappe les partitions d'une fonction de partition à un ensemble de groupes de fichiers. Le principal motif de placement des partitions sur des groupes de fichiers distincts est la possibilité de réaliser des opérations de sauvegarde indépendantes sur les partitions. En effet, vous pouvez réaliser des sauvegardes sur des groupes de fichiers spécifiques.  
  
 Colonne de partitionnement  
 Colonne d'une table ou d'un index utilisée par une fonction de partition pour partitionner la table ou l'index. Les colonnes calculées qui font partie d'une fonction de partition doivent présenter l'attribut PERSISTED. Tous les types de données autorisés dans les colonnes d'index peuvent être utilisés dans la colonne de partitionnement, sauf **timestamp**. Les types de données **ntext**, **text**, **image**, **xml**, **varchar(max)**, **nvarchar(max)** ou **varbinary(max)** ne peuvent pas être spécifiés. Le type défini par l’utilisateur CLR (Common Langage Runtime) Microsoft .NET Framework et les colonnes de type de données alias ne peuvent pas être non plus spécifiés.  
  
 Index aligné  
 Index créé sur le même schéma de partition que la table qui lui correspond. Lorsqu'une table et ses index sont alignés, SQL Server peut commuter rapidement et efficacement les partitions tout en préservant leur structure aussi bien dans la table que dans les index. Un index n'a pas besoin de participer à la même fonction de partition nommée pour être aligné avec sa table de base. Toutefois, la fonction de partition de l'index et celle de la table de base doivent être identique de trois points de vue : 1) les arguments des fonctions de partition ont le même type de données, 2) elles définissent le même nombre de partitions et 3) elles définissent les mêmes valeurs limites pour les partitions.  
  
 Index non aligné  
 Index partitionné indépendamment de la table correspondante. Autrement dit, l'index a un schéma de partition différent ou il est placé dans un groupe de fichiers différent de la table de base. La conception d'un index partitionné non aligné peut être utile dans les cas suivants :  
  
-   la table de base n'a pas été partitionnée ;  
  
-   la clé d'index est unique et elle ne doit pas contenir la colonne de partitionnement de la table ;  
  
-   vous souhaitez que la table de base soit impliquée dans des jointures communes à plusieurs tables en utilisant différentes colonnes de jointure.  

 Élimination de partition Processus par lequel l’optimiseur de requête accède uniquement aux partitions pertinentes pour remplir les critères de la requête.  
  
## <a name="performance-guidelines"></a>Recommandations relatives aux performances  
 La nouvelle limite plus élevée de 15 000 partitions affecte la mémoire, les opérations d'index partitionnés, les commandes DBCC et les requêtes. Cette section décrit les implications en matière de performances de l'augmentation du nombre de partitions au-delà de 1000 et fournit des solutions de contournement si nécessaire. La quantité maximale de partitions étant passée à 15 000, vous pouvez stocker des données pendant plus longtemps. Toutefois, vous devez conserver les données uniquement pendant la durée nécessaire et obtenir un compromis entre les performances et le nombre de partitions.  
  
### <a name="processor-cores-and-number-of-partitions-guidelines"></a>Recommandations concernant les cœurs de processeur et le nombre de partitions  
 Pour optimiser les performances liées aux opérations parallèles, nous vous recommandons d’utiliser le même nombre de partitions que de cœurs de processeur, à hauteur de 64 (cette valeur correspondant au nombre maximal de processeurs parallèles que peut utiliser SQL Server).  
  
### <a name="memory-usage-and-guidelines"></a>Utilisation de la mémoire et recommandations  
 Nous vous recommandons d'utiliser au moins 16 Go de RAM si un grand nombre de partitions sont en cours d'utilisation. Si le système n'a pas assez de mémoire, les instructions DML (Data Manipulation Language), les instructions DDL (Data Definition Language) et d'autres opérations peuvent échouer en raison d'une insuffisance de mémoire. Les systèmes avec 16 Go de RAM qui exécutent un grand nombre de processus nécessitant beaucoup de mémoire risque de ne pas disposer de suffisamment de mémoire lors des opérations qui s'exécutent sur un grand nombre de partitions. Par conséquent, plus vous disposez de mémoire au-delà de 16 Go, moins vous risquez de rencontrer des problèmes de performances et de mémoire.  
  
 Les limitations de mémoire peuvent affecter les performances de SQL Server ou sa capacité à créer un index partitionné. C'est le cas notamment lorsque l'index n'est pas aligné avec sa table de base ou son index cluster, si un index cluster a été appliqué à la table.  
  
### <a name="partitioned-index-operations"></a>Opérations d'index partitionné  
 Les limitations de mémoire peuvent affecter les performances de SQL Server ou sa capacité à créer un index partitionné. C'est notamment le cas avec des index non alignés. La création et la reconstruction des index non alignés sur une table contenant plus de 1 000 partitions sont possibles, mais ne sont pas prises en charge. Ces opérations peuvent entraîner une dégradation des performances ou une consommation de mémoire excessive.  
  
 La création et la reconstruction d'index alignés peuvent exiger davantage de temps à mesure que le nombre de partitions augmente. Nous vous recommandons de ne pas exécuter simultanément plusieurs commandes de création et de reconstruction d'index, car vous risquez de rencontrer des problèmes de performances et de mémoire.  
  
 Lorsque SQL Server effectue un tri pour créer des index partitionnés, il commence par créer une table de tri pour chaque partition. Ensuite, il produit les tables de tri soit dans le groupe de fichiers de chaque partition, soit dans **tempdb**, si l’option d’index SORT_IN_TEMPDB est spécifiée. La création de chaque table de tri nécessite une quantité minimale de mémoire. Lorsque vous créez un index partitionné qui est aligné avec sa table de base, les tables de tri sont créées une par une, ce qui utilise moins de mémoire. Toutefois, lorsque vous créez un index partitionné non aligné, les tables de tri sont produites en même temps. De ce fait, il doit y avoir assez de mémoire pour gérer ces tri simultanés. Plus il y a de partitions, plus il faut de mémoire. La taille minimale pour chaque table de tri, pour chaque partition, est de 40 pages, à raison de 8 kilo-octets par page. Par exemple, un index partitionné non aligné avec 100 partitions nécessite une quantité de mémoire suffisante pour trier en série 4 000 (40 * 100) pages à la fois. Si cette mémoire est disponible, l'opération de création réussit, mais les performances risquent d'en pâtir. Sinon, la création échoue. À l'inverse, un index partitionné aligné avec 100 partitions n'a besoin que de la mémoire suffisante pour trier 40 pages, parce que les tris ne sont pas effectués en même temps.  
  
 Pour les deux types d'index, alignés et non alignés, la mémoire requise peut être beaucoup plus importante si SQL Server applique divers degrés de parallélisme à l'opération de création sur un ordinateur multiprocesseur. En effet, plus il y a de degrés de parallélisme, plus il faut de mémoire. Par exemple, si SQL Server affecte aux degrés de parallélisme la valeur 4, un index partitionné non aligné avec 100 partitions a besoin d'une quantité de mémoire suffisante pour que quatre processeurs puissent trier 4 000 pages à la fois, soit 16 000 pages. Si l'index partitionné est aligné, la mémoire requise est moins importante puisqu'il en faut pour quatre processeurs triant 40 pages ou 160 (4 * 40) pages. Vous pouvez utiliser l'option d'index MAXDOP pour réduire manuellement les degrés de parallélisme.  
  
### <a name="dbcc-commands"></a>Commandes DBCC  
 Avec un plus grand nombre de partitions, l'exécution des commandes DBCC peut exiger davantage de temps à mesure que le nombre de partitions augmente.  
  
### <a name="queries"></a>Requêtes  
 Les requêtes qui utilisent l'élimination de partition peuvent présenter des performances comparables ou meilleures avec un plus grand nombre de partitions. Les requêtes qui n'utilisent pas l'élimination de partition peuvent être plus longues à mesure que le nombre de partitions augmente.  
  
 Par exemple, supposons qu'une table a 100 millions de lignes et de colonnes `A`, `B`et `C`. Dans le scénario 1, la table est divisée en 1 000 partitions sur la colonne `A`. Dans le scénario 2, la table est divisée en 10 000 partitions sur la colonne `A`. Une requête sur la table qui contient une clause WHERE filtrant sur la colonne `A` effectuera une élimination de partition et analysera une partition. Il se peut que cette même requête s'exécute plus rapidement dans le scénario 2 car il y a moins de lignes à analyser dans une partition. Une requête qui contient une clause WHERE filtrant sur la colonne B analyse toutes les partitions. Il se peut que cette requête s'exécute plus rapidement dans le scénario 1 que dans le scénario 2 car il y a moins de partitions à analyser.  
  
 Les requêtes qui utilisent des opérateurs tels que TOP ou MAX/MIN sur des colonnes autres que la colonne de partitionnement peuvent enregistrer une baisse des performances lors du partitionnement, du fait que toutes les partitions doivent être évaluées.  
  
## <a name="behavior-changes-in-statistics-computation-during-partitioned-index-operations"></a>Changements de comportement dans le calcul des statistiques pour les opérations d'index partitionnés  
 À partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les statistiques ne sont pas créées en analysant toutes les lignes de la table lorsqu'un index partitionné est créé ou reconstruit. Au lieu de cela, l'optimiseur de requête utilise l'algorithme d'échantillonnage par défaut pour générer des statistiques. Après la mise à niveau d'une base de données avec des index partitionnés, vous pouvez remarquer une différence dans les données d'histogramme pour ces index. Cette modification du comportement peut ne pas affecter les performances des requêtes. Pour obtenir des statistiques sur les index partitionnés en analysant toutes les lignes de la table, utilisez CREATE STATISTICS ou UPDATE STATISTICS avec la clause FULLSCAN.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|**Tâches**|**Rubrique**|  
|Décrit comment créer des fonctions de partition et des schémas de partition et les appliquer ensuite à une table ou à un index.|[Créer des tables et des index partitionnés](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)|  
|||  
  
## <a name="related-content"></a>Contenu associé  
 Les livres blancs suivants relatifs aux stratégies et implémentations de tables et index partitionnés pourront se révéler utiles.  
  
-   [Stratégies de tables et d’index partitionnés avec SQL Server 2008](http://msdn.microsoft.com/library/dd578580\(SQL.100\).aspx)  
  
-   [Comment implémenter une fenêtre glissante automatique](http://msdn.microsoft.com/library/aa964122\(SQL.90\).aspx)  
  
-   [Chargement en masse dans une table partitionnée](http://msdn.microsoft.com/library/cc966380.aspx)  
  
-   [Projet REAL : Cycle de vie de données -- Partitionnement](https://technet.microsoft.com/library/cc966424.aspx)  
  
-   [Améliorations du traitement des requêtes sur les tables et les index partitionnés](http://msdn.microsoft.com/library/ms345599.aspx)  
  
-   [10 meilleures pratiques pour générer un entrepôt de données relationnelles à grande échelle](http://sqlcat.com/top10lists/archive/2008/02/06/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse.aspx)  
  
  
