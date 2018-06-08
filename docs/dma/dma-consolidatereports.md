---
title: Consolider les rapports d’évaluation (données Assistant Migration SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 04/16/2018
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 7192c055b4b9bfb6155f0963c598f9dc4a760c0a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="consolidate-assessment-reports-data-migration-assistant"></a>Consolider les rapports d’évaluation (Assistant Migration de données)

Vous pouvez utiliser la ligne de commande pour effectuer des évaluations de la migration en mode sans assistance, en commençant par l’Assistant Migration de données v2.1. Cette fonctionnalité vous aide à exécuter les évaluations à l’échelle. L’évaluation des résultats sous la forme d’un fichier JSON ou CSV.

Vous pouvez évaluer plusieurs bases de données dans une seule instanciation de l’utilitaire de ligne de commande de l’Assistant Migration de données et exporter tous les résultats des évaluations dans un seul fichier JSON. Ou bien, vous pouvez évaluer une base de données au moment et ultérieurement consolider les résultats de ces plusieurs fichiers JSON dans une base de données SQL.

Pour plus d’informations sur la façon d’exécuter l’Assistant Migration de données à partir de la ligne de commande, consultez [exécuter données Assistant de Migration à partir de la ligne de commande](../dma/dma-commandline.md). 


## <a name="import-assessment-results-into-a-sql-server-database"></a>Importer les résultats d’évaluation dans une base de données SQL Server

Utilisez le script PowerShell disponible dans ce [référentiel Github](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/data-migration-assistant) pour importer les résultats d’évaluation à partir de fichiers JSON dans une base de données SQL Server.

> [!NOTE]
> PowerShell v5 ou ultérieure est requis.

Lorsque vous exécutez le script, vous devez fournir les informations suivantes : 

- **nom_serveur**: nom de l’instance SQL Server que vous souhaitez importer l’évaluation des résultats à partir de fichiers au format JSON.

- **databaseName**: le nom de base de données importés pour les résultats.

- **jsonDirectory**: le dossier dans lequel les résultats d’évaluation enregistrées, un ou plusieurs fichiers au format JSON.

- **processTo**: SQL Server

Ajoutez les valeurs précédentes dans la section « Exécuter les fonctions », comme suit.

```
dmaProcessor -serverName localhost \`\
-databaseName DMAReporting \`\
-jsonDirectory "C:\\temp\\DMACmd\\output\\" \`\
-processTo SQLServer
```

Le script PowerShell crée les objets suivants dans l’instance SQL que vous avez spécifié, si les objets n’existent pas déjà :

- **Base de données** – le nom fourni dans les paramètres PowerShell

  - Référentiel principal

- **Table** – rapports

  - Données création de rapports

- **Table** -BreakingChangeWeighting

  - Table de référence pour toutes les modifications avec rupture. Ici, vous pouvez définir vos propres valeurs de pondération afin d’influencer un classement de réussite de mise à niveau de pourcentage (%) plus précis.

- **Vue** – UpgradeSuccessRanking\_locale

  - Affichant un facteur de réussite pour chaque base de données être migrés localement.

- **Vue** – UpgradeSuccessRanking\_Azure

  - Affichant un facteur de réussite pour chaque base de données être migrés localement.

- **Procédure stockée** – JSONResults\_Insert

  - Utilisé pour importer des données à partir d’un fichier JSON dans SQL Server.

- **Procédure stockée** – AzureFeatureParityResults\_Insert

  - Utilisé pour importer les résultats de parité de fonctionnalités Azure à partir d’un fichier JSON dans SQL Server.

- **Type de table** – JSONResults

  - Utilisé pour stocker les résultats JSON pour les évaluations de localement et passer à la JSONResults\_procédure stockée Insert

- **Type de table** – AzureFeatureParityResults

  - Utilisé pour stocker des résultats de parité pour les évaluations azure de la fonctionnalité Azure et passer à la AzureFeatureParityResults\_procédure stockée Insert

Le script PowerShell crée un **traités** répertoire dans le répertoire que vous avez fournies qui contient les fichiers JSON qui doivent être traitées.

Une fois le script terminé, les résultats sont importés dans la table, les rapports.

### <a name="viewing-the-results-in-sql-server"></a>Affichage des résultats dans SQL Server

Une fois que les données ont été chargées, connectez-vous à votre instance de SQL Server. Votre écran doit apparaître comme indiqué dans l’illustration suivante :

![Rapports consolidés dans la base de données SQL Server](../dma/media/DMAReportingDatabase.png)

Le dbo. Rapports tableau contient le contenu du fichier JSON dans sa forme brute.

## <a name="on-premises-upgrade-success-ranking"></a>Local mise à niveau de classement de réussite

Pour afficher une liste de bases de données et leur rang de réussite de pourcentage (%), sélectionnez le dbo. Vue de UpgradeSuccessRanking_OnPrem :

![Affichage des données dans UpgradeSuccessRaning_OnPrem](../dma/media/UpgradeSuccessRankingView.png)

Vous trouverez ici pour une base de données quelle est la probabilité de réussite de mise à niveau pour les niveaux de compatibilité. Ainsi, par exemple, la base de données des ressources humaines a été évaluée par rapport aux niveaux de compatibilité 100, 110, 120 et 130. Cette évaluation vous permet de voir la quantité de travail est impliqué dans la migration vers une version ultérieure de SQL Server à partir de la version actuelle qui se trouve actuellement la base de données.

Généralement les mesures que vous intéressent sont le nombre de modifications avec rupture il pour une base de données. Dans l’exemple précédent, vous pouvez voir que la base de données des ressources humaines a un facteur de réussite de mise à niveau de 50 % pour les niveaux de compatibilité 100, 110, 120 et 130.

Cette mesure peut être influencée en modifiant les valeurs de pondération dans la table dbo. Table BreakingChangeWeighting.

Dans l’exemple suivant, les efforts nécessaires pour résoudre le problème de syntaxe dans la base de données des ressources humaines sont considéré comme élevé donc la valeur 3 est affectée à **Effort**. Car il n’aurait aucun longue pour résoudre le problème de syntaxe, la valeur 1 est affectée à **FixTime**. Car il serait un coût impliqués dans la modification, une valeur de 2 est assignée à **coût**. 2 à l’aide de cette valeur remplace la Changerank combinée.

> [!NOTE]
> Le score est sur une échelle de 1 à 5.  1 est faible et 5 soit élevée. En outre, le ChangeRank est une colonne calculée.

![Effort, FixTime et le coût des valeurs pour le problème de syntaxe](../dma/media/SyntaxIssueEffort.png)

Désormais, dans cet exemple, lorsque vous interrogez le dbo. Vue de UpgradeSuccessRanking_OnPrem, le facteur de réussite de mise à niveau de la base de données de ressources humaines pour les modifications avec rupture a supprimé.

![Facteur de réussite de mise à niveau de base de données des ressources humaines](../dma/media/UpgradeSuccessFactor_HR.png)

## <a name="azure-upgrade-success-ranking"></a>Classement de la réussite de mise à niveau Azure

Pour afficher la liste des bases de données à migrer vers la base de données SQL Azure et leur rang de réussite de pourcentage, sélectionnez le dbo. Vue de UpgradeSuccessRanking_Azure.

![Affichage des données dans UpgradeSuccessRanking_Azure](../dma/media/UpgradeSuccessRankingView_Azure.png)

Ici, vous êtes intéressé par la valeur MigrationBlocker. 100,00 signifie qu’il y a un rang de réussite de 100 % pour le déplacement d’une base de données à la base de données SQL Azure v12.

La différence avec cette vue est qu’il n’existe actuellement aucune substitution pour la modification de la pondération de règles de migration le bloqueur de fenêtres.

Pour plus d’informations sur la création de rapports sur ces données à l’aide de Power BI, consultez [créer des rapports sur vos évaluations consolidées avec Power BI](../dma/dma-powerbiassesreport.md).
