---
title: Procédure pas à pas de données de bout en bout science pour R et SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d3cba2c8deeec356b4d169960c76d65f2c6a02df
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="end-to-end-data-science-walkthrough-for-r-and-sql-server"></a>Procédure pas à pas de données de bout en bout science pour R et SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans cette procédure pas à pas, vous développez une solution de bout en bout pour la modélisation prédictive basée sur Microsoft R avec SQL Server 2016 ou SQL Server 2017.

Cette procédure pas à pas est basée sur le jeu de données des taxis de New York, un jeu de données publiques souvent utilisé. Vous utilisez une combinaison de code R, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données et des fonctions SQL personnalisées pour générer un modèle de classification qui indique la probabilité que le pilote peut obtenir un Conseil sur un voyage taxi particulier. Vous également déployez votre modèle R à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et utiliser les données du serveur pour générer des scores en fonction du modèle.

Cet exemple peut être étendu à tous les types de réels problèmes, tels que prédire les réponses des clients pour les campagnes de vente, ou de prévoir les dépenses ou la participation à des événements. Étant donné que le modèle peut être appelé à partir d’une procédure stockée, vous pouvez facilement l’incorporer dans une application.

Étant donné que la procédure pas à pas est conçu pour présenter aux développeurs de R [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], R est utilisé chaque fois que possible. Toutefois, cela ne signifie pas que R est nécessairement le meilleur outil pour chaque tâche. Dans de nombreux cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut offrir de meilleures performances, en particulier pour des tâches telles que l’agrégation de données et l’ingénierie de caractéristiques.  Ces tâches peuvent notamment profiter de nouvelles fonctionnalités dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], telles que les index columnstore optimisés en mémoire. Nous essayons de souligner les optimisations possibles tout au long du processus.

> [!NOTE]
> La procédure pas à pas a initialement été développée et testée pour SQL Server 2016. Toutefois, des captures d’écran et les procédures ont été mis à jour pour utiliser la dernière version de SQL Server Management Studio, qui fonctionne avec SQL Server 2017.

## <a name="overview"></a>Vue d'ensemble

Estimée temps n’incluent pas le programme d’installation. Pour plus d’informations, consultez [conditions préalables requises pour la procédure pas à pas](../tutorials/walkthrough-prerequisites-for-data-science-walkthroughs.md).

|Liste de rubriques|Durée estimée|
|-|------------------------------|
|[Préparer les données de la procédure pas à pas R](../tutorials/walkthrough-prepare-the-data.md) <br /><br />Obtenez les données utilisées pour la création d’un modèle. Téléchargez un jeu de données publiques et chargez-le dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|30 minutes|
|[Explorer les données à l’aide de SQL](../tutorials/walkthrough-view-and-explore-the-data.md) <br /><br />Comprendre vos données à l’aide des résumés et des outils SQL.|10 minutes|
|[Totaliser les données à l’aide de R](../tutorials/walkthrough-view-and-summarize-data-using-r.md) <br /><br />Utiliser R pour Explorer les données et générer des synthèses.|10 minutes|
|[Créer des graphiques à l’aide de R dans SQL Server](../tutorials/walkthrough-create-graphs-and-plots-using-r.md) <br /><br />Créer des graphiques dans les contextes de calcul locaux et distants par le mélange de R et SQL.|10 minutes|
|[Créer des fonctionnalités de données à l’aide de R et T-SQL)](../tutorials/walkthrough-create-data-features.md) <br /><br />Effectuez l’ingénierie des caractéristiques à l’aide de fonctions personnalisées dans R et [!INCLUDE[tsql](../../includes/tsql-md.md)]. Comparez les performances de R et de T-SQL pour les tâches de personnalisation des fonctions. |10 minutes|
|[Générer un modèle R et l’enregistrer dans SQL Server](../tutorials/walkthrough-build-and-save-the-model.md) <br /><br />Formez un modèle de prédiction et ajustez-le. Évaluez les performances d’un modèle. Cette procédure pas à pas permet de créer un modèle de classification. Tracez la précision du modèle à l’aide de R.|15 minutes|
|[Déployer le modèle R à l’aide de SQL Server](../tutorials/walkthrough-deploy-and-use-the-model.md) <br /><br />Déployez le modèle en production en l’enregistrant dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Appelez le modèle à partir d’une procédure stockée pour générer des prédictions.|10 minutes|

### <a name="intended-audience"></a>Public visé

Cette procédure pas à pas est destinée aux développeurs R ou SQL. Elle présente de quelle façon R peut être intégré dans les workflows d’entreprise à l’aide de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].  Vous devez être familiarisé avec les opérations de base de données, telles que la création de bases de données et de tables, l’importation de données et l’exécution des requêtes.

+ Tous les scripts SQL et R sont inclus.
+ Vous devrez peut-être modifier les chaînes dans les scripts à exécuter dans votre environnement. Cela avec n’importe quel éditeur de code, tel que [Visual Studio Code](https://code.visualstudio.com/Download).

### <a name="prerequisites"></a>Configuration requise

+ Vous devez avoir accès à une instance de SQL Server 2016, ou une version d’évaluation de SQL Server 2017.
+ [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] doit être installé sur au moins une instance sur l’ordinateur SQL Server.
+ Si vous souhaitez exécuter des commandes R à partir d’un ordinateur distant, par exemple un ordinateur portable ou un autre ordinateur en réseau, vous devez installer les bibliothèques Microsoft R Open. Vous pouvez installer Microsoft R Client ou Microsoft R Server. L’ordinateur distant doit être en mesure de se connecter à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.
+ Si vous devez mettre le client et le serveur sur le même ordinateur, veillez à installer un ensemble distinct de bibliothèques Microsoft R pour une utilisation lors de l’envoi du script R à partir d’un client « distant ». N’utilisez pas les bibliothèques R qui sont installés pour une utilisation par l’instance de SQL Server à cet effet.

Pour plus d’informations sur la façon de configurer ces environnements serveur et client, consultez [conditions préalables requises pour R et SQL Server procédure pas à pas de données scientifiques](../tutorials/walkthrough-prerequisites-for-data-science-walkthroughs.md).

## <a name="next-lesson"></a>Leçon suivante

[Préparer les données de la procédure pas à pas R](../tutorials/walkthrough-prepare-the-data.md)
