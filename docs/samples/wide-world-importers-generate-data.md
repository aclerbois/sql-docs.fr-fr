---
title: Générer des données - base de données exemple SQL WideWorldImporters | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6ace1f771ef3a77a6f7db0072442affe181d7872
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="wideworldimporters-data-generation"></a>Génération de données WideWorldImporters
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Les versions des bases de données WideWorldImporters et WideWorldImportersDW ont des données à partir du 1er janvier 2013, jusqu'à la journée à laquelle les bases de données ont été générées.

Lorsque vous utilisez ces bases de données d’exemple, vous souhaiterez incluent des exemples de données les plus récentes.

## <a name="data-generation-in-wideworldimporters"></a>Génération de données dans WideWorldImporters

Pour générer les exemples de données jusqu'à la date actuelle :

1. Si vous n’avez pas encore fait, installez une nouvelle version de la base de données WideWorldImporters. Pour obtenir des instructions d’installation, consultez [Installation et la configuration](wide-world-importers-oltp-install-configure.md).
2. Dans la base de données, exécutez l’instruction suivante :

    ```
        EXECUTE DataLoadSimulation.PopulateDataToCurrentDate
            @AverageNumberOfCustomerOrdersPerDay = 60,
            @SaturdayPercentageOfNormalWorkDay = 50,
            @SundayPercentageOfNormalWorkDay = 0,
            @IsSilentMode = 1,
            @AreDatesPrinted = 1;
    ```

    Cette instruction ajoute des ventes de l’exemple et les données d’achat à la base de données, jusqu'à la date actuelle. Il affiche la progression de la génération de données par jour. Génération de données peut prendre environ 10 minutes pour chaque année qui a besoin de données. En raison d’un facteur aléatoire dans la génération de données, il existe des différences dans les données qui sont générées entre chaque exécution.

    Pour augmenter ou diminuer la quantité de données générées pour les commandes par jour, modifiez la valeur pour le paramètre `@AverageNumberOfCustomerOrdersPerDay`. Utilisez les paramètres `@SaturdayPercentageOfNormalWorkDay` et `@SundayPercentageOfNormalWorkDay` pour déterminer le volume de commandes pour les jours de week-end.

## <a name="import-generated-data-in-wideworldimportersdw"></a>Données d’importation générée dans WideWorldImportersDW

Pour importer les exemples de données jusqu'à la date actuelle dans la base de données WideWorldImportersDW OLAP :

1. Exécuter la logique de génération de données dans la base de données WideWorldImporters OLTP à l’aide de la procédure dans la section précédente.
2. Si vous n’avez pas encore fait, installez une nouvelle version de la base de données WideWorldImportersDW. Pour obtenir des instructions d’installation, consultez [Installation et la configuration](wide-world-importers-oltp-install-configure.md).
3. Réamorcer la base de données OLAP en exécutant l’instruction suivante dans la base de données :

    ```sql
    EXECUTE [Application].Configuration_ReseedETL
    ```

4. Exécutez le *ETL.ispac quotidienne* package SQL Server Integration Services pour importer les données dans la base de données OLAP. Pour savoir comment exécuter la tâche ETL, voir [flux de travail WideWorldImporters ETL](wide-world-importers-perform-etl.md).

## <a name="generate-data-in-wideworldimportersdw-for-performance-testing"></a>Générer des données dans WideWorldImportersDW au test des performances

WideWorldImportersDW arbitrairement peut augmenter la taille des données pour les tests de performances. Par exemple, taille des données à utiliser avec l’indexation de columnstore en cluster peut augmenter.

L’une des difficultés est de conserver la taille du téléchargement assez petit pour télécharger facilement, mais suffisamment pour illustrer les fonctionnalités de performances de SQL Server. Par exemple, des avantages significatifs pour les index columnstore sont obtenus lorsque vous travaillez avec un plus grand nombre de lignes. 

Vous pouvez utiliser la `Application.Configuration_PopulateLargeSaleTable` procédure pour augmenter le nombre de lignes dans la `Fact.Sale` table. Les lignes sont insérées dans l’année 2012 pour éviter toute collision avec des données existantes de quatre coins du monde qui commence le 1er janvier 2013.

### <a name="procedure-details"></a>Détails de la procédure

#### <a name="name"></a>Nom

    Application.Configuration_PopulateLargeSaleTable

#### <a name="parameters"></a>Paramètres

  `@EstimatedRowsFor2012` **bigint** (valeur par défaut 12000000)

#### <a name="result"></a>Résultat

Approximativement le nombre de lignes requis est inséré dans le `Fact.Sale` table dans l’année 2012. La procédure limite artificiellement le nombre de lignes à 50 000 par jour. Vous pouvez modifier cette limitation, mais la limitation vous permet d’éviter overinflations accidentelles de la table.

La procédure s’applique également à columnstore en cluster, l’indexation s’il n’a pas déjà été appliqué.
