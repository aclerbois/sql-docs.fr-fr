---
title: WideWorldImportersDW - flux de travail ETL | Documents Microsoft
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
ms.openlocfilehash: 36638c4cc2bda58ac277822d5c4a4ce5421ab8b4
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="wideworldimportersdw-etl-workflow"></a>Flux de travail WideWorldImportersDW ETL
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Utilisez le *WWI_Integration* package ETL pour migrer des données à partir de la base de données WideWorldImporters à la base de données WideWorldImportersDW en tant que les modifications de données. Le package est exécuté périodiquement (généralement quotidienne).

Le package garantit hautes performances à l’aide de SQL Server Integration Services pour orchestrer les opérations en bloc T-SQL (au lieu de transformations distinctes dans Integration Services).

Les dimensions sont chargées en premier, puis les tables de faits sont chargés. Vous pouvez réexécuter le package à tout moment après une défaillance.

Le flux de travail ressemble à ceci :

 ![Flux de travail WideWorldImporters ETL](media/wide-world-importers/wideworldimporters-etl-workflow.png)

Le flux de travail démarre avec une tâche d’expression qui détermine le moment opportun de coupure. L’heure limite est l’heure actuelle moins de quelques minutes. (Cette approche est plus fiable que la demande des données directement à l’heure actuelle). Les millisecondes sont tronqués à partir de l’heure.

Le traitement principal commence par le remplissage de la table de dimension de Date. Le traitement permet de s’assurer que toutes les dates de l’année en cours ont été renseignées dans la table.

Ensuite, une série de tâches de flux de données charge chaque dimension. Ensuite, leur chargement chaque fait.

## <a name="prerequisites"></a>Configuration requise

- SQL Server 2016 (ou version ultérieure), avec les bases de données WideWorldImporters et WideWorldImportersDW (dans le même ou dans différentes instances de SQL Server)
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - Vérifiez que vous créez un catalogue Integration Services. Pour créer un catalogue Integration Services, dans l’Explorateur d’objets SQL Server Management Studio, cliquez sur **Integration Services**, puis sélectionnez **ajouter un catalogue**. Laissez les options par défaut. Vous êtes invité à activer SQLCLR et fournir un mot de passe.


## <a name="download"></a>Télécharger

Pour obtenir la dernière version de l’exemple, consultez [wide world importers version](http://go.microsoft.com/fwlink/?LinkID=800630). Téléchargez le *ETL.ispac quotidienne* fichier de package Integration Services.

Pour le code source recréer la base de données, consultez [sein wide world importers](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-integration-etl).

## <a name="install"></a>Installation

1. Déployer le package Integration Services :
   1. Dans l’Explorateur Windows, ouvrez le *ETL.ispac quotidienne* package. Cette opération lance l’Assistant de déploiement de SQL Server Integration Services.
   2. Sous **sélectionner une Source**, suivez les valeurs par défaut pour le déploiement de projet, avec le chemin d’accès pointant vers le *ETL.ispac quotidienne* package.
   3. Sous **sélectionner la Destination**, entrez le nom du serveur qui héberge le catalogue Integration Services.
   4. Sélectionnez un chemin d’accès sous le catalogue Integration Services, par exemple, dans un dossier nommé *WideWorldImporters*.
   5. Sélectionnez **déployer** pour terminer l’Assistant.

2. Créer un travail de l’Agent SQL Server pour le processus ETL :
   1. Dans Management Studio, cliquez sur **l’Agent SQL Server**, puis sélectionnez **nouveau** > **travail**.
   2. Entrez un nom, par exemple, *WideWorldImporters ETL*.
   3. Ajouter un **étape de travail** du type **Package SQL Server Integration Services**.
   4. Sélectionnez le serveur qui possède le catalogue Integration Services, puis le *ETL quotidienne* package.
   5. Sous **Configuration** > **gestionnaires de connexions**, assurez-vous que les connexions à la source et cible sont configurées correctement. La valeur par défaut consiste à connecter à l’instance locale.
   6. Sélectionnez **OK** pour créer le travail.

3. Exécuter ou planifier la tâche.
