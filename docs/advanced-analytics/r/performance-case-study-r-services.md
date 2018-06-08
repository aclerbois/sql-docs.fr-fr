---
title: Performances de SQL Server R Services - résultats et les ressources | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ce5fb99b3808b9da0d32bee48ff31f6e0b2dae95
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="performance-for-r-services-results-and-resources"></a>Performances pour R Services : résultats et des ressources
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article est le quatrième et dernière d’une série qui décrit l’optimisation des performances pour R Services. Cet article résume les méthodes, des résultats et des conclusions de deux études de cas qui testé différentes méthodes d’optimisation.

Les études de cas deux a différents objectifs :

+ La première étude de cas, par l’équipe de développement R Services, recherché mesurer l’impact des techniques d’optimisation spécifiques
+ La deuxième étude de cas, par une équipe de spécialistes des données, Transposez plusieurs méthodes pour déterminer les meilleures optimisations pour un scénario de calcul de score de haut volume spécifique.

Cette rubrique répertorie les résultats détaillés de la première étude de cas. Pour le deuxième étude de cas, un résumé décrit les conclusions générales. À la fin de cette rubrique sont des liens vers tous les scripts et les exemples de données et les ressources utilisées par les auteurs d’origine.

## <a name="performance-case-study-airline-dataset"></a>Étude de cas de performances : jeu de données de billet d’avion

Cette étude de cas par l’équipe de développement SQL Server R Services testé les effets de différentes optimisations. Un modèle unique rxLogit a été créé et calcul de score effectuées sur le jeu de données compagnie aérienne. Optimisations ont été appliquées au cours de la formation et le score des processus afin d’évaluer l’impact individuels.

- Github : [exemples de données et les scripts](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PerfTuning) pour l’étude des optimisations de SQL Server

### <a name="test-methods"></a>Méthodes de test

1. Le jeu de données compagnie aérienne compose d’une seule table de lignes de 10M. Il a été téléchargé et en bloc chargées dans SQL Server.
2. Six copies de la table ont été apportées.
3. Diverses modifications ont été appliquées aux copies de la table, pour tester les fonctionnalités de SQL Server comme la compression de page, la compression de ligne, l’indexation, le magasin de données en colonnes, etc.
4. Performances a été mesuré avant et après que chaque optimisation a été appliquée.

| Nom de la table|  Description|
|------|------|
| *airline* | Données converties à partir du fichier xdf d’origine à l’aide de `rxDataStep`|                          |
| *airlineWithIntCol*   | Chaîne *DayOfWeek* convertie en entier. Colonne *rowNum* également ajoutée.|
| *airlineWithIndex*    | Mêmes données que dans la table *airlineWithIntCol*, mais avec un seul index cluster utilisant la colonne *rowNum*.|
| *airlineWithPageComp* | Mêmes données que dans la table *airlineWithIndex*, mais avec la compression de page activée. Deux colonnes également ajoutées, *CRSDepHour* et *Late*, qui sont calculées à partir de *CRSDepTime* et *ArrDelay*. |
| *airlineWithRowComp*  | Mêmes données que dans la table *airlineWithIndex*, mais avec la compression de ligne activée. Deux colonnes également ajoutées, *CRSDepHour* et *Late*, qui sont calculées à partir de *CRSDepTime* et *ArrDelay*. |
| *airlineColumnar*     | Stockage en colonnes avec un seul index cluster. Table remplie avec les données d’un fichier csv nettoyé.|

Chaque test est composé de ces étapes :

1. Une opération garbage collection R a été induite avant chaque test.
2. Un modèle de régression logistique a été créé en fonction des données de la table. La valeur de *rowsPerRead* a été définie sur 500 000 pour chaque test.
3. Scores générés à l’aide du modèle formé
4. Chaque test a été exécuté six fois. L’heure de la première exécution (la « exécuter à froid ») a été supprimé. Pour permettre la valeur hors norme occasionnelle, le **maximale** temps entre les exécutions de cinq restantes a été supprimé également. Le temps d’exécution écoulé moyen de chaque test a été calculé à partir de la moyenne des quatre exécutions restantes.
5. Les tests ont été exécutés à l’aide de la *reportProgress* paramètre avec la valeur 3 (= le minutage de l’état et la progression). Chaque fichier de sortie contient des informations concernant le temps consacré aux e/s, temps de transition et temps de calcul. Ces informations sont utiles pour le diagnostic et la résolution des problèmes.
6. La sortie de console a été également dirigée vers un fichier dans le répertoire de sortie.
7. Les scripts de test a traité les heures dans ces fichiers afin de calculer la durée moyenne des exécutions.

Par exemple, les résultats suivants sont aux fois à partir d’un seul test. Les valeurs les plus intéressantes sont **Total read time** (le temps d’E/S) et **Transition time** (le temps passé à créer les processus de calcul).

**Exemple de minutage**

```
Running IntCol Test. Using airlineWithIntCol table.
run 1 took 3.66 seconds
run 2 took 3.44 seconds
run 3 took 3.44 seconds
run 4 took 3.44 seconds
run 5 took 3.45 seconds
run 6 took 3.75 seconds
  
Average Time: 3.4425
metric time pct
1 Total time 3.4425 100.00
2 Overall compute time 2.8512 82.82
3 Total read time 2.5378 73.72
4 Transition time 0.5913 17.18
5 Total non IO time 0.3134 9.10
```

Nous vous recommandons de télécharger et de modifier les scripts de test pour vous aider à résoudre les problèmes avec R Services ou avec les fonctions RevoScaleR.

### <a name="test-results-all"></a>Tester les résultats (tous)

Cette section compare avant et après les résultats pour chacun des tests.

#### <a name="data-size-with-compression-and-a-columnar-table-store"></a>Taille des données avec la compression et une banque de colonnes de table

Le premier test par rapport à l’utilisation de la compression et une table en colonnes pour réduire la taille des données.

| Nom de la table            | Lignes     | Réservé   | data       | index_size | Inutilisé  | % gagné (réservé) |
|-----------------------|----------|------------|------------|------------|---------|---------------------|
| *airlineWithIndex*    | 10 000 000 | 2 978 816 Ko | 2 972 160 Ko | 6 128 Ko    | 528 Ko  | 0                   |
| *airlineWithPageComp* | 10 000 000 | 625 784 Ko  | 623 744 Ko  | 1 352 Ko    | 688 Ko  | 79 %                 |
| *airlineWithRowComp*  | 10 000 000 | 1 262 520 KB | 1 258 880 Ko | 2 552 Ko    | 1 088 Ko | 58 %                 |
| *airlineColumnar*     | 9 999 999  | 201 992 Ko  | 201 624 Ko  | n/a        | 368 Ko  | 93 %                 |

**Conclusions**

La plus grande réduction de taille des données a été obtenue en appliquant un index columnstore, suivi de la compression de page.

#### <a name="effects-of-compression"></a>Effets de la compression

Ce test de comparer les avantages de la compression de ligne, la compression de page et aucune compression. Un modèle a été formé à l’aide de `rxLinMod` sur les données de trois tables de données différents. La même formule et requête a été utilisée pour toutes les tables.

| Nom de la table            | Nom du test       | numTasks | Temps moyen |
|-----------------------|-----------------|----------|--------------|
| *airlineWithIndex*    | NoCompression   | 1        | 5,6775       |
|                       | NoCompression - parallèle| 4        | 5,1775       |
| *airlineWithPageComp* | PageCompression | 1        | 6,7875       |
|                       | PageCompression - parallèle | 4        | 5,3225       |
| *airlineWithRowComp*  | RowCompression  | 1        | 6,1325       |
|                       | RowCompression - parallèle  | 4        | 5,2375       |

**Conclusions**

Seule la compression ne semble pas à l’aide. Dans cet exemple, l’augmentation de l’UC pour gérer la compression permet de compenser la diminution de la durée d’e/s.

Toutefois, quand le test est exécuté en parallèle en définissant *numTasks* sur 4, le temps moyen diminue.

L’effet de la compression peut être plus significatif pour les jeux de données volumineux. La compression varie selon le jeu de données et les valeurs. Vous avez donc intérêt à faire plusieurs tests pour évaluer l’effet de la compression sur votre jeu de données.

### <a name="effect-of-windows-power-plan-options"></a>Effet des options de plan d’alimentation de Windows

Dans cette expérience, `rxLinMod` a été utilisé avec la table *airlineWithIntCol*. La gestion de l’alimentation de Windows a été définie avec la valeur **équilibré** ou **hautes performances**. Pour tous les tests, *numTasks* a été défini sur 1. Le test a été exécuté six fois et a été exécutée deux fois dans les deux options d’alimentation pour étudier la variabilité des résultats.

**Hautes performances** option d’alimentation :

| Nom du test | Exécutez \# | Temps écoulé | Temps moyen |
|-----------|--------|--------------|--------------|
| IntCol    | 1      | 3,57 secondes |              |
|           | 2      | 3,45 secondes |              |
|           | 3      | 3,45 secondes |              |
|           | 4      | 3,55 secondes |              |
|           | 5      | 3,55 secondes |              |
|           | 6      | 3,45 secondes |              |
|           |        |              | 3,475        |
|           | 1      | 3,45 secondes |              |
|           | 2      | 3,53 secondes |              |
|           | 3      | 3,63 secondes |              |
|           | 4      | 3,49 secondes |              |
|           | 5      | 3,54 secondes |              |
|           | 6      | 3,47 secondes |              |
|           |        |              | 3,5075       |

Option de gestion de l’alimentation **Équilibré** :

| Nom du test | Exécutez \# | Temps écoulé | Temps moyen |
|-----------|--------|--------------|--------------|
| IntCol    | 1      | 3,89 secondes |              |
|           | 2      | 4,15 secondes |              |
|           | 3      | 3,77 secondes |              |
|           | 4      | 5 secondes    |              |
|           | 5      | 3,92 secondes |              |
|           | 6      | 3,8 secondes  |              |
|           |        |              | 3,91         |
|           | 1      | 3,82 secondes |              |
|           | 2      | 3,84 secondes |              |
|           | 3      | 3,86 secondes |              |
|           | 4      | 4,07 secondes |              |
|           | 5      | 4,86 secondes |              |
|           | 6      | 3,75 secondes |              |
|           |        |              | 3,88         |

**Conclusions**

La durée d’exécution est plus cohérent et plus rapides lors de l’utilisation de Windows **hautes performances** de l’alimentation.

#### <a name="using-integer-vs-strings-in-formulas"></a>À l’aide des entiers et des chaînes dans les formules

Ce test évaluer l’impact de la modification du code R pour éviter un problème courant avec les facteurs de chaîne. Plus précisément, un modèle a été formé à l’aide de `rxLinMod` à l’aide de deux tables : dans la première, les facteurs sont stockés sous forme de chaînes ; dans la deuxième table, les facteurs sont stockées en tant qu’entiers.

+ Pour le *compagnie aérienne* table, la colonne [DayOfWeek] contient des chaînes. Le _colInfo_ paramètre a été utilisé pour spécifier les niveaux de facteur (lundi, mardi,...)

+  Pour le *airlineWithIndex* [DayOfWeek] est un entier. Le _colInfo_ paramètre n’a pas été spécifié.

+ Dans les deux cas, la même formule a été utilisée : `ArrDelay ~ CRSDepTime + DayOfWeek`.

| Nom de la table          | Nom du test   | Temps moyen |
|---------------------|-------------|--------------|
| *Billet d’avion*           | *FactorCol* | 10,72        |
| *airlineWithIntCol* | *IntCol*    | 3,4475       |

**Conclusions**

Il existe un avantage évident lors de l’utilisation de nombres entiers plutôt que des chaînes de facteurs.

### <a name="avoiding-transformation-functions"></a>Éviter les fonctions de transformation

Dans ce test, un modèle a été formé à l’aide de `rxLinMod`, mais le code a été modifié entre les deux séries :

+ Dans la première exécution, une fonction de transformation a été appliquée dans le cadre de la génération du modèle. 
+ Dans la seconde exécution, les valeurs de fonctionnalités ont été précalculées et disponibles, afin qu’aucune fonction de transformation a été requise.

| Nom du test             | Temps moyen |
|-----------------------|--------------|
| WithTransformation    | 5,1675       |
| WithoutTransformation | 4,7          |

**Conclusions**

Le temps d’apprentissage est plus court lorsque **pas** à l’aide d’une fonction de transformation. En d’autres termes, le modèle a été formé plus rapidement lors de l’utilisation des colonnes qui sont précalculés et conservées dans la table.

Les économies doit être supérieure si a été de nombreuses transformations plus et le jeu de données ont été supérieure (\> 100 M).

### <a name="using-columnar-store"></a>À l’aide de la banque de colonnes

Ce test évaluer les avantages de l’utilisation d’un magasin de données en colonnes et les index. Le modèle même a été formé à l’aide de `rxLinMod` et aucune transformation de données.

+ Dans la première exécution, la table de données utilisé un magasin de ligne standard.
+ Dans la seconde exécution, une banque des colonnes a été utilisée.

| Nom de la table         | Nom du test | Temps moyen |
|--------------------|-----------|--------------|
| *airlineWithIndex* | RowStore  | 4,67         |
| *airlineColumnar*  | ColStore  | 4,555        |

**Conclusions**

Les performances sont meilleures avec le magasin de colonnes que la banque de ligne standard. Une différence significative dans les performances peut se produire sur les jeux de données volumineux (\> 100 M).

### <a name="effect-of-using-the-cube-parameter"></a>Effet de l’utilisation du paramètre de cube

L’objectif de ce test consistait à déterminer si l’option de RevoScaleR pour à l’aide de la commande précalculées **cube** paramètre peut améliorer les performances. Un modèle a été formé à l’aide de `rxLinMod`, à l’aide de cette formule :

```R
ArrDelay ~ Origin:DayOfWeek + Month + DayofMonth + CRSDepTime
```

Dans la table, les facteurs *DayOfWeek* est stocké sous forme de chaîne.

| Nom du test     | Paramètre cube | numTasks | Temps moyen | Ligne unique prédire (ArrDelay_Pred) |
|---------------|----------------|----------|--------------|---------------------------------|
| CubeArgEffect | `cube = F`     | 1        | 91,0725      | 9,959204                        |
|               |                | 4        | 44,09        | 9,959204                        |
|               | `cube = T`     | 1        | 21,1125      | 9,959204                        |
|               |                | 4        | 8,08         | 9,959204                        |

**Conclusions**

L’utilisation de l’argument du paramètre cube clairement améliore les performances.

### <a name="effect-of-changing-maxdepth-for-rxdtree-models"></a>Effet de la modification maxDepth pour les modèles rxDTree

Dans cette expérience, les `rxDTree` algorithme a été utilisé pour créer un modèle sur le *airlineColumnar* table. Pour ce test, *numTasks* a été défini sur 4. Plusieurs valeurs différentes pour *maxDepth* ont été utilisés pour illustrer comment la modification de profondeur d’arborescence affecte moment de l’exécution.

| Nom du test       | maxDepth | Temps moyen |
|-----------------|----------|--------------|
| TreeDepthEffect | 1        | 10,1975      |
|                 | 2        | 13,2575      |
|                 | 4        | 19,27        |
|                 | 8        | 45,5775      |
|                 | 16       | 339,54       |

**Conclusions**

La profondeur de l’arborescence de l’augmentation du nombre total de nœuds augmente de façon exponentielle. Le temps écoulé pour la création du modèle est également augmenté de manière significative.

### <a name="prediction-on-a-stored-model"></a>PRÉDICTION sur un modèle stocké

L’objectif de ce test consistait à déterminer l’impact sur les performances de score lorsque le modèle formé est enregistré dans une table SQL Server plutôt que générées en tant que partie du code en cours d’exécution. Pour calculer les scores, le modèle stocké est chargé à partir de la base de données et des prédictions sont créées à l’aide d’une trame de données d’une ligne dans la mémoire (contexte de calcul local).

Les résultats des tests affichent l’heure pour enregistrer le modèle et le temps nécessaire pour charger le modèle et à prévoir.

| Nom de la table | Nom du test | Temps moyen d’apprentissage du modèle | Temps pour enregistrer/charger le modèle|
|------------|------------|------------|------------|
| airline    | SaveModel| 21,59| 2,08|
| airline    | LoadModelAndPredict | | 2,09 (inclut le temps de prédiction) |

**Conclusions**

Chargement d’un modèle formé à partir d’une table est une méthode plus rapide pour effectuer la prédiction. Nous vous déconseillons de création du modèle et l’exécution dans le même script de calcul de score.

## <a name="case-study-optimization-for-the-resume-matching-task"></a>Étude de cas : l’optimisation de la tâche de mise en correspondance de reprise

Le modèle de mise en correspondance de reprise a été développé par le chercheur de données Microsoft Ke Huang pour tester les performances du code R dans SQL Server et en effectuant des chercheurs créer évolutives, donc les données aide solutions au niveau de l’entreprise.

### <a name="methods"></a>Méthodes

Les packages MicrosoftML et RevoScaleR ont été utilisés pour l’apprentissage d’un modèle de prévision dans une solution R complexe impliquant de volumineux datasets. Requêtes SQL et le code R sont identiques dans tous les tests. Les tests ont été effectués sur une seule machine virtuelle de Azure avec SQL Server est installé. Ensuite, l’auteur comparé heures de calcul de score avec et sans les optimisations suivantes fournies par SQL Server :

- Tables en mémoire
- Soft-NUMA
- Resource Governor

Pour évaluer l’effet de soft-NUMA sur l’exécution du script R, l’équipe de science des données testé la solution sur une machine virtuelle Azure avec 20 cœurs physiques. Sur ces cœurs physiques, les quatre nœuds soft-NUMA ont été créées automatiquement, telle que chaque nœud contenus cinq cœurs.

Affinage de l’UC a été appliqué dans le scénario de mise en correspondance de reprise, afin d’évaluer l’impact sur les travaux R. Quatre **pools de ressources SQL** et quatre **pools de ressources externes** ont été créés, et l’affinité du processeur a été spécifiée pour vous assurer que le même jeu de processeurs est utilisable dans chaque nœud.

Chacun des pools de ressources a été attribué à un groupe de charges de travail différent, pour optimiser l’utilisation du matériel. La raison est que Soft-NUMA et l’affinité de processeur ne peut pas diviser la mémoire physique dans les nœuds NUMA physiques ; Par conséquent, par définition, tous les nœuds NUMA logicielles qui sont basés sur le même nœud NUMA physique doivent utiliser mémoire dans le même bloc de mémoire du système d’exploitation. En d’autres termes, il n’existe aucune affinité du processeur de mémoire.

Le processus suivant a été utilisé pour créer cette configuration :

1. Réduire la quantité de mémoire allouée par défaut pour SQL Server.

2. Créez quatre nouveaux pools pour exécuter les travaux R en parallèle.

3. Créez quatre groupes de charges de travail de sorte que chaque groupe de charge de travail est associé à un pool de ressources.

4. Redémarrez le gouverneur de ressources avec les nouveaux groupes de charges de travail et les affectations.

5. Créer une fonction classifieur définie par l’utilisateur (UDF) permettent d’attribuer différentes tâches sur les groupes de charges de travail différent.

6. Mettre à jour la configuration du gouverneur de ressources pour utiliser la fonction pour les groupes de charges de travail approprié.

### <a name="results"></a>Résultats

La configuration ayant les meilleures performances dans la correspondance de reprise étudier était le suivant :

-   Quatre pools de ressources internes (pour SQL Server)

-   Quatre pools de ressources externes (pour les travaux de script externe)

-   Chaque pool de ressources est associé à un groupe de charges de travail spécifique

-   Chaque pool de ressources est affecté à différentes UC

-   Utilisation maximale de mémoire interne (pour SQL Server) = 30 %

-   Mémoire maximale utilisée par les sessions R = 70 %

Pour le modèle de mise en correspondance de reprise, script externe a été importante, et aucune autre base de données n’est en cours d’exécution des services de moteur. Par conséquent, les ressources allouées aux scripts externes ont été augmentées à 70 %, ce qui a été identifié comme la meilleure configuration pour les performances de script.

Cette configuration a été arrivée en expérimentant des modèles avec des valeurs différentes. Si vous utilisez un matériel différent ou une autre solution, la configuration optimale peut être différente. Toujours faire des essais pour trouver la meilleure configuration pour votre cas !

Dans la solution optimisée, 1.1 millions de lignes (avec les fonctionnalités de 100) ont été notées dans sous 8,5 secondes sur un ordinateur de 20 cœurs. Les optimisations d’améliorer considérablement les performances en termes de temps de calcul de score.

Les résultats suggéré également que le **nombre de fonctionnalités** eu un impact significatif sur le temps de calcul de score. L’amélioration a été encore plus importante lorsque davantage de fonctionnalités ont été utilisés dans le modèle de prévision.

Nous vous recommandons de lire cet article de blog et le didacticiel d’accompagnement pour une présentation détaillée.

-   [Conseils d’optimisation et pour l’apprentissage dans SQL Server](https://azure.microsoft.com/blog/optimization-tips-and-tricks-on-azure-sql-server-for-machine-learning-services/)

De nombreux utilisateurs ont de noter qu’il existe une petite pause en tant que le runtime R (ou Python) est chargé pour la première fois. Pour cette raison, comme décrit dans ces tests, l’heure de la première exécution est souvent mesurée mais rejeté par la suite. Mise en cache ultérieure peut entraîner des différences de performances notables entre la première et deuxième s’exécute. Il existe également une charge de traitement lorsque les données sont déplacées entre SQL Server et le runtime externe, en particulier si les données sont transmises sur le réseau au lieu d’être chargée directement à partir de SQL Server.

Pour toutes ces raisons, il n’existe aucune solution pour atténuer ce temps de chargement initial, comme l’impact sur les performances varie considérablement selon la tâche. Par exemple, la mise en cache est effectuée pour une seule ligne de calcul de score par lots ; Par conséquent, les opérations de calcul de score successives sont beaucoup plus rapides et le modèle, ni le runtime R est rechargé. Vous pouvez également utiliser [score native](../sql-native-scoring.md) pour éviter de charger le runtime R entièrement.

Apprentissage des modèles de grande taille ou de score par lots de grande taille, la surcharge peut être minimale comparée les gains en évitant le déplacement des données ou à partir de la diffusion en continu et le traitement parallèle. Consultez ces dernières blogs et des exemples pour obtenir des conseils supplémentaires sur les performances :

+ [Classification de prêt à l’aide de SQL Server 2016 R Services](https://blogs.msdn.microsoft.com/microsoftrservertigerteam/2016/09/27/loan-classification-using-sql-server-2016-r-services/)
+ [Client anticipée des expériences avec R Services](https://blogs.msdn.microsoft.com/sqlcat/2016/06/16/early-customer-experiences-with-sql-server-r-services/)
+ [À l’aide de R à détecter une fraude à 1 million de transactions par seconde](http://blog.revolutionanalytics.com/2016/09/fraud-detection.html/)

## <a name="resources"></a>Ressources

Voici des liens vers plus d’informations, les outils et les scripts utilisés dans le développement de ces tests.

+ Test des scripts et des liens vers les données de performance : [exemples de données et des scripts pour l’étude des optimisations de SQL Server](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PerfTuning)

+ Article décrivant la solution de mise en correspondance de reprise : [Conseil d’optimisation et des astuces pour SQL Server R Services](https://azure.microsoft.com/blog/optimization-tips-and-tricks-on-azure-sql-server-for-machine-learning-services/)

+ Scripts utilisés dans l’optimisation de SQL pour la solution de mise en correspondance de reprise : [référentiel GitHub](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/SQLOptimizationTips)

### <a name="learn-about-windows-server-management"></a>En savoir plus sur la gestion de Windows server

+ [Comment faire pour déterminer la taille de fichier d’échange appropriée pour les versions 64 bits de Windows](https://support.microsoft.com/kb/2860880)

+ [Présentation de NUMA](https://technet.microsoft.com/library/ms178144.aspx)

+ [Comment SQL Server prend en charge NUMA](https://technet.microsoft.com/library/ms180954.aspx)

+ [Configuration NUMA logicielle](https://docs.microsoft.com/sql/database-engine/configure-windows/soft-numa-sql-server)

### <a name="learn-about-sql-server-optimizations"></a>En savoir plus sur les optimisations de SQL Server

+ [Réorganiser et reconstruire des index](../../relational-databases\indexes\reorganize-and-rebuild-indexes.md)

+ [Introduction aux tables mémoire optimisées](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables)

+ [Démonstration : Optimisation de performances de l’OLTP en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp)

+ [Compression de données](../../relational-databases/data-compression/data-compression.md)

+ [Activer la compression sur une table ou un index](../../relational-databases/data-compression/enable-compression-on-a-table-or-index.md)

+ [Désactiver la compression sur une table ou un index](../../relational-databases/data-compression/disable-compression-on-a-table-or-index.md)

### <a name="learn-about-managing-sql-server"></a>En savoir plus sur la gestion de SQL Server

+ [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)

+ [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)

+ [Présentation du gouverneur de ressources](https://technet.microsoft.com/library/bb895232.aspx)

+ [Gouvernance des ressources pour R Services](resource-governance-for-r-services.md)

+ [Comment créer un pool de ressources pour R](how-to-create-a-resource-pool-for-r.md)

+ [Un exemple de configuration du gouverneur de ressources](https://blog.sqlauthority.com/2012/06/04/sql-server-simple-example-to-configure-resource-governor-introduction-to-resource-governor/)

### <a name="tools"></a>Outils

+ [Générateur de charge de stockage/outil de test de performances DISKSPD](https://github.com/microsoft/diskspd)

+ [Informations de référence sur l’utilitaire FSUtil](https://technet.microsoft.com/library/cc753059.aspx)


## <a name="other-articles-in-this-series"></a>Autres articles de cette série

[Performances du paramétrage des R – introduction](sql-server-r-services-performance-tuning.md)

[Réglage des performances pour R - configuration de SQL Server](sql-server-configuration-r-services.md)

[Réglage des performances pour R - R optimisation de code et les données](r-and-data-optimization-r-services.md)

[Réglage des performances - résultats de l’étude de cas](performance-case-study-r-services.md)
