---
title: Événements étendus pour prédire les instructions d’analyse | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 9f56544416d88cc56fed13833667bf689f395693
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="extended-events-for-monitoring-predict-statements"></a>Événements étendus pour prédire les instructions d’analyse

Cet article décrit les événements étendus fournis dans SQL Server que vous pouvez utiliser pour surveiller et analyser les travaux qui utilisent [PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) pour effectuer le calcul de score en temps réel dans SQL Server.

Calculer les scores en temps réel génère les scores à partir d’un modèle qui a été stocké dans SQL Server d’apprentissage. La fonction PREDICT ne nécessite pas d’une exécution externe tels que R ou Python, seul un modèle qui a été créé à l’aide d’un format binaire spécifique. Pour plus d’informations, consultez [en temps réel de calcul de score](https://docs.microsoft.com/sql/advanced-analytics/real-time-scoring).

## <a name="prerequisites"></a>Configuration requise

Pour obtenir des informations générales sur les événements étendus (parfois appelés événements étendus) et comment assurer le suivi des événements dans une session, consultez les articles suivants :

+ [Architecture et des concepts des événements étendus](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events)
+ [Configurer la capture d’événements dans SSMS](https://docs.microsoft.com/sql/relational-databases/extended-events/quick-start-extended-events-in-sql-server)
+ [Gérer des sessions d’événements dans l’Explorateur d’objets](https://docs.microsoft.com/sql/relational-databases/extended-events/manage-event-sessions-in-the-object-explorer)

## <a name="table-of-extended-events"></a>Tableau des événements étendus

Les événements étendus suivants sont disponibles dans toutes les versions de SQL Server qui prennent en charge la [T-SQL prédire](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) instruction, y compris SQL Server sur Linux et la base de données SQL Azure. 

L’instruction T-SQL prédire a été introduite dans SQL Server 2017. 

|name |object_type|description| 
|----|----|----|
|predict_function_completed |événement  |Répartition du temps d’exécution de Builtin|
|predict_model_cache_hit |événement|Se produit lorsqu’un modèle est récupéré du cache du modèle de fonction PREDICT. Utilisez cet événement avec d’autres événements predict_model_cache_ * pour résoudre les problèmes causés par le cache de modèle de fonction PREDICT.|
|predict_model_cache_insert |événement  |   Se produit lorsqu’un modèle est inséré dans le cache de modèle de fonction PREDICT. Utilisez cet événement avec d’autres événements predict_model_cache_ * pour résoudre les problèmes causés par le cache de modèle de fonction PREDICT.    |
|predict_model_cache_miss   |événement|Se produit lorsqu’un modèle est introuvable dans le cache de modèle de fonction PREDICT. De fréquentes occurrences de cet événement peut indiquer que SQL Server a besoin de davantage de mémoire. Utilisez cet événement avec d’autres événements predict_model_cache_ * pour résoudre les problèmes causés par le cache de modèle de fonction PREDICT.|
|predict_model_cache_remove |événement| Se produit lorsqu’un modèle est supprimé du cache de modèle pour la fonction de prédiction. Utilisez cet événement avec d’autres événements predict_model_cache_ * pour résoudre les problèmes causés par le cache de modèle de fonction PREDICT.|

## <a name="query-for-related-events"></a>Rechercher des événements connexes

Pour afficher une liste de toutes les colonnes retournées pour ces événements, exécutez la requête suivante dans SQL Server Management Studio :

```sql
SELECT * FROM sys.dm_xe_object_columns WHERE object_name LIKE `predict%'
```

## <a name="examples"></a>Exemples

Capture d’informations sur les performances d’une session de score à l’aide de la prédiction :

1. Créer une nouvelle étendue de session d’événements, à l’aide de Management Studio ou un autre pris en charge [outil](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events-tools).
2. Ajoutez les événements `predict_function_completed` et `predict_model_cache_hit` à la session.
3. Démarrer la session d’événements étendus.
4. Exécutez la requête qui utilise la prédiction.

Dans les résultats, passez en revue ces colonnes :

+ La valeur de `predict_function_completed` affiche le temps que la requête consacrée à charger le modèle et le score.
+ La valeur booléenne pour `predict_model_cache_hit` indique si la requête a utilisé un modèle mis en cache ou non. 

### <a name="native-scoring-model-cache"></a>Cache de modèle de score natif

Outre les événements spécifiques à prédire, vous pouvez utiliser les requêtes suivantes pour obtenir plus d’informations sur le modèle mis en cache et l’utilisation du cache :

Afficher le cache de modèle de score natif :

```sql
SELECT *
FROM sys.dm_os_memory_clerks
WHERE type = 'CACHESTORE_NATIVESCORING';
```

Afficher les objets dans le cache des modèles :

```sql
SELECT *
FROM sys.dm_os_memory_objects
WHERE TYPE = 'MEMOBJ_NATIVESCORING';
```

