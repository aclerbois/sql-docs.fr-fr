---
title: Afficher et Explorer les données à l’aide de SQL (procédure pas à pas) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: b933337c1234f09f0a8963c1979a86a9ab61db53
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="view-and-explore-the-data-using-sql-walkthrough"></a>Afficher et Explorer les données à l’aide de SQL (procédure pas à pas)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’exploration de données est une partie importante de la modélisation des données et implique l’examen de synthèses d’objets de données à utiliser dans les analyses, ainsi que la visualisation des données. Dans cette leçon, vous avez exploré les objets de données et générer des graphiques, à l’aide de deux [!INCLUDE[tsql](../../includes/tsql-md.md)] et des fonctions R incluses dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].

Ensuite, vous générez des graphiques pour visualiser les données, à l’aide de nouvelles fonctions fournies par les packages installés avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].

> [!TIP]
> Vous êtes déjà un utilisateur expérimenté de R ?
>   
> Maintenant que vous avez téléchargé toutes les données et préparé l’environnement, vous êtes invité à exécuter le script R complet dans RStudio ou tout autre environnement, et à explorer les fonctionnalités par vous-même. Ouvrez simplement le fichier RSQL_Walkthrough.R, et sélectionnez et exécutez des lignes individuelles, ou exécutez la totalité du script en guise de démonstration.
>   
> Pour obtenir des explications supplémentaires sur les fonctions RevoScaleR et des conseils sur l’utilisation des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans R, continuez le didacticiel. Il utilise exactement le même script.

## <a name="verify-downloaded-data-using-sql-server"></a>Vérifiez que les données téléchargées à l’aide de SQL Server

Tout d’abord, prenez le temps de vérifier que vos données ont été chargées correctement.

1. Se connecter à votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance à l’aide de votre outil de gestion préféré de la base de données, tels que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’Explorateur de serveurs dans Visual Studio ou Visual Studio Code.

2. Sélectionnez la base de données que vous avez créé et que vous développez pour afficher la nouvelle base de données, les tables et les fonctions.
  
    ![rsql_e2e_ssms_newobjects](media/rsql-e2e-ssms-newobjects.PNG)
  
3.  Pour vérifier que les données chargées correctement, avec le bouton droit de la table et sélectionnez **sélectionnez 1000 lignes du haut**. L’option de menu exécute cette requête :

    ```SQL
    SELECT TOP 1000 * FROM [dbo].[nyctaxi_sample]
    ```
    Si la table ne contient aucune donnée, consultez la section [Dépannage](walkthrough-prepare-the-data.md) de la rubrique précédente.

4. Pour optimiser cette table de données pour les calculs basés sur les jeux, un [index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) a été ajouté. Exécutez cette instruction pour générer un résumé de la table.

    ```SQL
    SELECT DISTINCT [passenger_count]
        , ROUND (SUM ([fare_amount]),0) as TotalFares
        , ROUND (AVG ([fare_amount]),0) as AvgFares
    FROM [dbo].[nyctaxi_sample]
    GROUP BY [passenger_count]
    ORDER BY  AvgFares DESC
    ````
    Dans la leçon suivante, vous générerez des résumés plus complexes à l’aide de R.

## <a name="next-lesson"></a>Leçon suivante

[Résumer les données à l’aide de R](walkthrough-view-and-summarize-data-using-r.md)

## <a name="previous-lesson"></a>Leçon précédente

[Préparer les données à l’aide de PowerShell](walkthrough-prepare-the-data.md)
