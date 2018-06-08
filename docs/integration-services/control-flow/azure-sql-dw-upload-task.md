---
title: Chargement Azure SQL Data Warehouse, tâche | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPDWUPTASK.F1
- sql14.dts.designer.afpdwuptask.f1
ms.assetid: eef82c89-228a-4dc7-9bd0-ea00f57692f5
caps.latest.revision: 5
author: Lingxi-Li
ms.author: lingxl
manager: craigg
ms.openlocfilehash: 8da49de795c0a0fbc8d6da2cf28374a07d7f8778
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="azure-sql-dw-upload-task"></a>Tâche de chargement Azure SQL Data Warehouse
La **tâche de chargement Azure SQL Data Warehouse** permet à un package SSIS de charger des données locales sur une table dans Azure SQL Data Warehouse. Le format de fichier de données source pris en charge actuellement est le texte délimité dans l’encodage UTF-8. Le processus de chargement suit l’approche PolyBase efficace comme décrit dans l’article [Azure SQL Data Warehouse Loading Patterns and Strategies](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). Plus précisément, les données sont tout d’abord chargées sur le stockage Blob Azure, puis sur Azure SQL Data Warehouse. Par conséquent, un compte de stockage Blob Azure est nécessaire pour utiliser cette tâche.

La **tâche de chargement Azure SQL Data Warehouse** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

Pour ajouter une **tâche de chargement Azure SQL Data Warehouse**, faites-la glisser de la boîte à outils SSIS vers le canevas du concepteur, double-cliquez dessus ou cliquez dessus avec le bouton droit, puis cliquez sur **Modifier** pour afficher la boîte de dialogue de l’éditeur de tâche.

Dans la page **Général** , configurez les propriétés suivantes.

Champ|Description
-----|-----------
LocalDirectory|Spécifie le répertoire local qui contient les fichiers de données à charger.
Recursively|Spécifie s’il convient d’effectuer des recherches de façon récursive dans les sous-répertoires.
FileName|Indique un filtre de nom pour sélectionner des fichiers dont le nom répond à certains critères. Par ex. MaFeuille*.xsl\* inclut les fichiers MaFeuille001.xsl et MaFeuilleABC.xslx.
RowDelimiter|Spécifie le ou les caractères qui marquent la fin de chaque ligne.
ColumnDelimiter|Spécifie un ou plusieurs caractères qui marquent la fin de chaque colonne. Par ex. &#124; (barre verticale), \t (tabulation), ’ (apostrophe), “ (guillemets doubles) et 0x5c (barre oblique inverse).
IsFirstRowHeader|Spécifie si la première ligne de chaque fichier de données contient les noms de colonne au lieu des données réelles.
AzureStorageConnection|Spécifie un gestionnaire de connexions de stockage Azure.
BlobContainer|Spécifie le nom du conteneur d’objets blob sur lequel les données locales seront chargées et relayées vers Azure Data Warehouse via PolyBase. Un conteneur sera créé s’il n’existe pas.
BlobDirectory|Spécifie le répertoire d’objets blob (structure hiérarchique virtuelle) sur lequel les données locales seront chargées et relayées vers Azure Data Warehouse via PolyBase.
RetainFiles|Spécifie s’il convient de conserver les fichiers chargés sur le stockage Azure.
CompressionType|Spécifie le format de compression à utiliser lors du chargement de fichiers sur le stockage Azure. La source locale n’est pas affectée.
CompressionLevel|Spécifie le niveau de compression à utiliser pour le format de compression.
AzureDwConnection|Spécifie un gestionnaire de connexions ADO.NET pour Azure SQL Data Warehouse.
TableName|Spécifie le nom de la table de destination. Choisissez un nom de table existant ou créez-en un en choisissant **\<Nouvelle table ...>**.
TableDistribution|Spécifie la méthode de distribution pour la nouvelle table. S’applique si un nouveau nom de table est spécifié pour **TableName**.
HashColumnName|Spécifie la colonne utilisée pour la distribution de table de hachage. S’applique si la valeur **HASH** est spécifiée pour **TableDistribution**.

La page **Mappages** sera différente selon si vous procédez au chargement vers une nouvelle table ou une table existante. Dans le premier cas, configurez les colonnes sources à mapper et les noms correspondants dans la table de destination à créer. Dans le second cas, configurez les relations de mappage entre les colonnes sources et de destination.

Dans la page **Colonnes** , configurez les propriétés de type de données pour chaque colonne source.

La page **T-SQL** affiche le T-SQL utilisé pour charger des données depuis le stockage Blob Azure sur Azure SQL Data Warehouse. Le T-SQL est généré automatiquement à partir des configurations sur les autres pages et sera exécuté dans le cadre de l’exécution de la tâche. Vous pouvez choisir de modifier manuellement le T-SQL généré pour répondre à vos besoins spécifiques en cliquant sur le bouton **Modifier** . Vous pouvez revenir à celui qui a été généré automatiquement par la suite en cliquant sur le bouton **Réinitialiser** .

