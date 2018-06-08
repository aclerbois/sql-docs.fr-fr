---
title: Performances de SQL Server R Services - optimisation des données | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 5a30ff30651bacde42c60a1e0b265105e3c932e3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34563757"
---
# <a name="performance-for-r-services---data-optimization"></a>Performances pour R Services - optimisation des données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article est le troisième d’une série qui décrit l’optimisation des performances pour R Services basé sur deux études de cas. Cet article traite des optimisations de performances pour R ou Python scripts qui s’exécutent dans SQL Server. Elle décrit également les méthodes que vous pouvez utiliser pour mettre à jour votre code R, pour optimiser les performances et à éviter les problèmes connus.

## <a name="choosing-a-compute-context"></a>Choix d’un contexte de calcul

Dans SQL Server 2016 et 2017, vous pouvez utiliser la **local** ou **SQL** contexte de calcul lors de l’exécution du script R ou Python.

Lorsque vous utilisez la **local** contexte de calcul, analyse est effectuée sur votre ordinateur et non sur le serveur. Par conséquent, si vous obtenez des données à partir de SQL Server à utiliser dans votre code, les données doivent être extraites sur le réseau. L’impact de ce transfert réseau sur les performances est plus ou moins important selon le volume de données transférées, la vitesse du réseau et les autres transferts réseau simultanés.

Lorsque vous utilisez la **contexte de calcul de SQL Server**, le code est exécuté sur le serveur. Si vous obtenez des données à partir de SQL Server, les données doivent être locales sur le serveur exécutant l’analyse, et par conséquent aucune surcharge réseau n’est introduite. Si vous avez besoin importer des données provenant d’autres sources, envisagez d’organiser ETL au préalable.

Si vous manipulez des jeux de données volumineux, utilisez toujours le contexte de calcul SQL.

## <a name="factors"></a>Facteurs

Le langage R propose le concept de « facteurs », une variable spéciale pour les données par catégorie. Les chercheurs de données souvent utiliser des variables de facteur dans leur formule, étant donné que la gestion des variables catégorielles comme facteurs garantit que les données est traitée correctement par les fonctions d’apprentissage machine. Pour plus d’informations, consultez [R pour les nuls : facteur de Variables](http://www.dummies.com/programming/r/how-to-look-at-the-structure-of-a-factor-in-r/).

Par sa conception, les variables de facteur peuvent être convertis à partir de chaînes à des entiers et arrière à nouveau pour le stockage ou le traitement. R `data.frame` fonction gère toutes les chaînes en tant que variables de facteur, sauf si l’argument *stringsAsFactors* a la valeur **False**. Cela signifie est que les chaînes sont automatiquement converti en un entier pour le traitement, puis mappé à la chaîne d’origine.

Si les données sources à des facteurs sont stockées en tant qu’entier, performances peuvent être affectées, parce que R convertit les entiers facteur en chaînes en cours d’exécution, puis effectue sa propre conversion de chaîne en entier interne.

Pour éviter ces conversions d’exécution, envisagez de stocker les valeurs en tant qu’entiers dans la table SQL Server et à l’aide de la _colInfo_ argument pour spécifier les niveaux de la colonne utilisée comme facteur. La plupart des objets de source de données dans RevoScaleR acceptent le paramètre _colInfo_. Ce paramètre vous permet de nommer les variables utilisées par la source de données, spécifiez leur type et définir les niveaux de variables ou des transformations sur les valeurs de colonne.

Par exemple, l’appel de fonction R suivante obtient les entiers de 1, 2 et 3 à partir d’une table, mais il mappe les valeurs à un facteur de niveaux « apple », « orange » et « banana ».

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

Lors de la colonne source contient des chaînes, il est toujours plus efficace de spécifier les niveaux avant l’heure à l’aide de la _colInfo_ paramètre. Par exemple, le code R suivant traite les chaînes en tant que facteurs lors de leur lecture.

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

S’il n’existe aucune différence de sémantique de la génération du modèle, cette dernière approche peut entraîner une amélioration des performances.

## <a name="data-transformations"></a>Transformations de données

Les « scientifiques des données » utilisent souvent des fonctions de transformation écrites en langage R pour leurs analyses. La fonction de transformation est appliquée à chaque ligne extraite de la table. Dans SQL Server, ces transformations sont appliquées à toutes les lignes sont récupérées dans un lot, ce qui requiert une communication entre l’interpréteur R et le moteur analytique. Les données sont transférées de SQL vers le moteur analytique, puis vers le processus de l’interpréteur R, avant d’être renvoyées.

Pour cette raison, l’à l’aide des transformations dans le cadre de votre code R peut avoir un effet négatif significatif sur les performances de l’algorithme, selon la quantité de données impliquées.

Il est plus efficace de toutes les colonnes nécessaires dans la table ou vue avant d’effectuer l’analyse et éviter les transformations au cours du calcul. Si vous ne pouvez pas ajouter de colonnes aux tables existantes, créez une autre table ou vue contenant les colonnes transformées, puis utilisez une requête appropriée pour récupérer les données.

## <a name="batch-row-reads"></a>Lit les lignes du lot

Si vous utilisez une source de données SQL Server (`RxSqlServerData`) dans votre code, nous vous recommandons d’essayer d’utiliser le paramètre _rowsPerRead_ pour spécifier la taille de lot. Ce paramètre définit le nombre de lignes qui sont interrogées et puis envoyé au script externe pour le traitement. Au moment de l’exécution, l’algorithme voit uniquement le nombre spécifié de lignes dans chaque lot.

La possibilité de contrôler la quantité de données sont traitées à la fois peut vous aider à résoudre ou éviter les problèmes. Par exemple, si votre jeu de données d’entrée est très grande (possède plusieurs colonnes), ou si le dataset contient quelques colonnes volumineuses (par exemple, texte libre), vous pouvez réduire la taille de lot pour éviter la pagination des données en dehors de la mémoire.

Par défaut, la valeur de ce paramètre est définie à 50 000, pour garantir des performances même sur des ordinateurs avec peu de mémoire acceptable. Si le serveur dispose de suffisamment de mémoire disponible, l’augmentation de cette valeur à 500 000 ou même un million peut générer de meilleures performances, en particulier pour les tables volumineuses.

Les avantages de l’augmentation de taille de lot deviennent évidents sur un jeu de données volumineux et une tâche qui peut s’exécuter sur plusieurs processus. Toutefois, l’augmentation de cette valeur ne produit pas toujours les meilleurs résultats. Nous vous recommandons d’expérimenter avec vos données et l’algorithme permettant de déterminer la valeur optimale.

## <a name="parallel-processing"></a>Traitement parallèle

Pour améliorer les performances de **rx** fonctions analytiques, vous pouvez exploiter la capacité de SQL Server pour exécuter des tâches en parallèle à l’aide de cœurs disponibles sur l’ordinateur serveur.

Il existe deux manières d’effectuer la parallélisation de R dans SQL Server :

-   **Utilisez \@parallèle.** Si vous utilisez la procédure stockée `sp_execute_external_script` pour exécuter un script R, définissez le paramètre `@parallel` à `1`. Ceci est la meilleure méthode si votre script R ne **pas** utiliser les fonctions RevoScaleR, qui ont d’autres mécanismes pour le traitement. Si votre script utilise les fonctions RevoScaleR (généralement précédées de « rx »), le traitement parallèle est effectué automatiquement et vous n’avez pas besoin de définir explicitement `@parallel` à `1`.

    Si le script R peut être parallélisé, et si la requête SQL peut être parallélisée, le moteur de base de données crée plusieurs traitements parallèles. Le nombre maximal de processus qui peuvent être créés est égal à la **degré maximal de parallélisme** paramètre (MAXDOP) pour l’instance. Tous les processus puis exécutez le même script, mais uniquement une partie des données de réception.
    
    Par conséquent, cette méthode n’est pas utile pour les scripts qui doivent voir toutes les données, par exemple quand un modèle d’apprentissage. Au contraire, cette méthode est utile pour effectuer certaines tâches, comme la prédiction par lot en parallèle. Pour plus d’informations sur l’utilisation de parallélisme avec `sp_execute_external_script`, consultez la **avancé conseils : le traitement parallèle** section de [à l’aide de Code R dans Transact-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md).

-   **Utilisez numTasks = 1.** Lorsque vous utilisez **rx** fonctions dans un contexte de calcul de SQL Server, définissez la valeur de la _numTasks_ paramètre pour le nombre de processus que vous souhaitez créer. Le nombre de processus créé ne peut jamais être plus de **MAXDOP**; Toutefois, le nombre réel de processus créé est déterminé par le moteur de base de données et peut être inférieure à vous a demandé.

    Si le script R peut être parallélisé, et si la requête SQL peut être parallélisée, SQL Server crée plusieurs traitements parallèles, lors de l’exécution des fonctions rx. Le nombre réel de processus qui sont créés dépend de divers facteurs tels que la gouvernance des ressources, en cours d’utilisation des ressources, les autres sessions et le plan d’exécution de requête pour la requête utilisée avec le script R.

## <a name="query-parallelization"></a>Parallélisation de la requête

Dans Microsoft R, vous pouvez travailler avec des sources de données SQL Server en définissant de vos données comme un objet de source de données RxSqlServerData.

Crée une source de données basée sur une table entière ou une vue :

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

Crée une source de données basée sur une requête SQL :

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> Si une table est spécifiée dans la source de données au lieu d’une requête, R Services utilise l’heuristique interne pour déterminer les colonnes nécessaires pour récupérer à partir de la table ; Toutefois, cette approche est peu susceptible d’engendrer l’exécution en parallèle.

Pour vous assurer que les données peuvent être analysées en parallèle, la requête utilisée pour récupérer les données doit être encadrée de telle sorte que le moteur de base de données peut créer un plan de requête parallèle. Si le code ou l’algorithme utilise des volumes importants de données, assurez-vous que la requête donnée à `RxSqlServerData` est optimisé pour une exécution parallèle. Une requête qui ne retourne pas de plan d’exécution parallèle peut générer un processus unique pour le calcul.

Si vous avez besoin travailler avec les jeux de données volumineux, utilisez Management Studio ou un autre analyseur de requêtes SQL avant d’exécuter votre code R, pour analyser le plan d’exécution. Effectuer toutes les étapes recommandées pour améliorer les performances de la requête. Par exemple, l’absence d’un index sur une table peut faire augmenter le temps d’exécution d’une requête. Pour plus d’informations, consultez [surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md).

Une autre erreur courante qui peut affecter les performances est une requête extrait des colonnes supplémentaires que nécessaire. Par exemple, si une formule est basée sur seulement trois colonnes, mais il se peut que votre table source a 30 colonnes, vous déplacez données inutilement.

 + Évitez d’utiliser `SELECT *`!
 + Prendre un certain temps pour vérifier les colonnes dans le jeu de données et identifier uniquement ceux qui sont nécessaires pour l’analyse
 + Supprimer à partir de vos requêtes, toutes les colonnes qui contiennent des types de données qui ne sont pas compatibles avec le code R, tels que les GUID et ROWGUID
 + Vérification des formats non pris en charge de date et heure
 + Plutôt que de charger une table, créez une vue qui sélectionne certaines valeurs ou convertit les colonnes pour éviter les erreurs de conversion

## <a name="optimizing-the-machine-learning-algorithm"></a>Optimisation de l’algorithme d’apprentissage automatique

Cette section fournit des conseils et divers ressources qui sont spécifiques à RevoScaleR et d’autres options dans Microsoft R.

> [!TIP]
> Une présentation générale de l’optimisation de R est hors de l’étendue de cet article. Toutefois, si vous avez besoin rendre votre code plus rapide, nous recommandons l’article populaire, [la R Inferno](http://www.burns-stat.com/pages/Tutor/R_inferno.pdf). Elle couvre les constructions de programmation dans R et des pièges courants dans le langage vive et de détails et fournit de nombreux exemples spécifiques de techniques de programmation de R.

### <a name="optimizations-for-revoscaler"></a>Optimisations de RevoScaleR

De nombreux algorithmes de RevoScaleR prend en charge les paramètres pour contrôler la façon dont le modèle formé est généré. La précision et l’exactitude du modèle est important, les performances de l’algorithme peuvent être tout aussi important. Pour obtenir un bon équilibre entre la précision et le temps d’apprentissage, vous pouvez modifier les paramètres pour augmenter la vitesse de calcul, dans de nombreux cas, d’améliorer les performances sans réduire la précision ou l’exactitude.

+ [rxDTree](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdtree)

    `rxDTree` prend en charge la `maxDepth` paramètre qui contrôle la profondeur de l’arbre de décision. En tant que `maxDepth` est augmentée, performances peuvent se dégrader, il est donc important d’analyser les avantages de l’augmentation de la profondeur et réduit les performances.

    Vous pouvez également contrôler l’équilibre entre la précision de la complexité et la prédiction de temps en ajustant des paramètres tels que `maxNumBins`, `maxDepth`, `maxComplete`, et `maxSurrogate`. L’augmentation de la profondeur au-delà de 10 ou 15 peut rendre le calcul très coûteux.

+ [rxLinMod](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxlinmod)

    Essayez d’utiliser le `cube` argument si la première variable dépendante dans la formule est une variable de facteur.
    
    Lorsque `cube` a la valeur `TRUE`, la régression est effectuée à l’aide d’un inverse partitionnée, ce qui peut être plus rapides et utilisent moins de mémoire que le calcul de régression standard. Si la formule contient un grand nombre de variables, le gain de performance peut être significatif.

+ [rxLogit](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxlogit)

    Utilisez le `cube` argument si la première variable dépendante est un facteur.
    
    Lorsque `cube` a la valeur `TRUE`, l’algorithme utilise un inverse partitionnée, ce qui peut être plus rapide et utilise moins de mémoire. Si la formule contient un grand nombre de variables, le gain de performance peut être significatif.

Pour plus d’informations sur l’optimisation des RevoScaleR, voir les articles suivants :

+ Article du support technique : [options de rxDForest et rxDTree de réglage des performances](https://support.microsoft.com/kb/3104235)

+ Les méthodes de modèle de contrôle peut être contenue dans un modèle d’arbre augmenté : [estimation des modèles à l’aide de stochastique le Gradient Boosting](https://docs.microsoft.com/r-server/r/how-to-revoscaler-boosting)

+ Vue d’ensemble du mode de RevoScaleR déplace et traite les données : [écrire des algorithmes de segmentation personnalisés dans ScaleR](https://docs.microsoft.com/r-server/r/how-to-developer-write-chunking-algorithms)

+ Modèle de programmation pour RevoScaleR : [la gestion des threads dans RevoScaleR](https://docs.microsoft.com/r-server/r/how-to-developer-manage-threads)

+ Référence de fonction [rxDForest](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdforest)

+ Référence de fonction [rxBTrees](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxbtrees)

### <a name="use-microsoftml"></a>Utilisez MicrosoftML

Nous vous recommandons également de que vous recherchez dans le nouvel **MicrosoftML** package, qui fournit des algorithmes d’apprentissage automatique évolutive que vous peuvent utiliser les contextes de calcul et les transformations fournies par RevoScaleR.

+ [Prise en main MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)

+ [Comment choisir un algorithme MicrosoftML](https://docs.microsoft.com/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

### <a name="operationalize-a-solution-using-microsoft-r-server"></a>Tiens une solution à l’aide de Microsoft R Server

Si votre scénario implique la prédiction rapide à l’aide d’un modèle stocké ou l’intégration d’apprentissage automatique dans une application, vous pouvez utiliser la [à l’Opérationnalisation](https://docs.microsoft.com/r-server/what-is-operationalization) fonctionnalités de Microsoft R Server (anciennement DeployR).

+ Comme un **chercheur de données**, utilisez le [mrsdeploy package](https://docs.microsoft.com/r-server/r-reference/mrsdeploy/mrsdeploy-package) à partager du code R avec d’autres ordinateurs et à intégrer analytique R dans des applications web, bureau, mobile et du tableau de bord : [comment publier et gérer les services web de R dans R Server](https://docs.microsoft.com/r-server/operationalize/how-to-deploy-web-service-publish-manage-in-r)

+ Comme un **administrateur**, découvrez comment gérer les packages, surveiller des nœuds de site web et les nœuds de calcul et contrôler la sécurité sur les travaux R : [comment manipuler et utiliser des services web dans R](https://docs.microsoft.com/r-server/operationalize/how-to-consume-web-service-interact-in-r)

## <a name="articles-in-this-series"></a>Articles de cette série

[Performances du paramétrage des R – introduction](sql-server-r-services-performance-tuning.md)

[Réglage des performances pour R - configuration de SQL Server](sql-server-configuration-r-services.md)

[Réglage des performances pour R - R optimisation de code et les données](r-and-data-optimization-r-services.md)

[Réglage des performances - résultats de l’étude de cas](performance-case-study-r-services.md)
