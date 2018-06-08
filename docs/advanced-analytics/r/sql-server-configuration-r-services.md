---
title: Configuration de SQL Server (R Services) | Microsoft Docs
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 382b39a7209480f7f02b0cee5d91eb6cbd662cd9
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-configuration-for-use-with-r"></a>Configuration de SQL Server pour une utilisation avec R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article est le deuxième d’une série qui décrit l’optimisation des performances pour R Services basé sur deux études de cas.  Cet article fournit des conseils sur la configuration matérielle et réseau de l’ordinateur qui est utilisé pour exécuter SQL Server R Services. Il contient également des informations sur les façons de configurer l’instance SQL Server, base de données ou les tables utilisées dans une solution. Étant donné que l’utilisation de NUMA dans SQL Server flou sur la ligne entre les optimisations de matériel et de la base de données, une troisième section traite de processeur affinage et gouvernance des ressources en détail.

> [!TIP]
> Si vous ne connaissez pas SQL Server, nous vous recommandons également consulter le guide du paramétrage des performances SQL Server : [surveillance et le réglage de performance](https://docs.microsoft.com/sql/relational-databases/performance/monitor-and-tune-for-performance).

## <a name="hardware-optimization"></a>Optimisation du matériel

L’optimisation de l’ordinateur du serveur est importante pour s’assurer que vous disposez de ressources pour exécuter des scripts externes. Lorsque les ressources sont limitées, vous pouvez rencontrer des problèmes telles que celles-ci :

- Exécution d’une tâche est différée ou annulée pour hiérarchiser d’autres opérations de base de données
- Erreur « quota dépassé » à l’origine du script R se terminer sans fin
- Données chargées en mémoire de R tronqué pour des résultats incomplets

### <a name="memory"></a>Mémoire

La quantité de mémoire disponible sur l’ordinateur peut avoir un impact important sur les performances des algorithmes d’analyse avancés. Mémoire insuffisante peut affecter le degré de parallélisme lors de l’utilisation du contexte de calcul SQL. Cela peut aussi affecter la taille de segment (lignes par opération de lecture) qui peut être traitée, ainsi que le nombre de sessions simultanées qui peuvent être prises en charge.

Un minimum de 32 Go est fortement recommandé. Si vous avez plus de 32 Go de disponible, vous pouvez configurer la source de données SQL pour utiliser plus de lignes dans chaque opération de lecture pour améliorer les performances.

Vous pouvez également gérer la mémoire utilisée par l’instance. Par défaut, SQL Server est prioritaire sur les processus de script externe lors de l’allocation de mémoire. Dans une installation par défaut de R Services, seulement 20 % de mémoire est allouée à R.

En général, cela n’est pas suffisant pour les tâches courantes relatives aux données, mais ni voulez-vous priver de temps processeur SQL server de la mémoire. Vous devez tester et ajuster l’allocation de mémoire entre le moteur de base de données, les services associés et des scripts externes, étant entendu que la configuration optimale varie selon le cas par cas.

Pour le modèle de mise en correspondance de reprise, utilisation du script externe s’est importante et il n’était aucune autre base de données services de moteur en cours d’exécution ; Par conséquent, les ressources allouées à des scripts externes ont été augmentées à 70 %, qui était la meilleure configuration pour les performances de script.

### <a name="power-options"></a>Options d’alimentation

Sur le système d’exploitation Windows, le **hautes performances** power option doit être utilisée. À l’aide d’un paramètre d’alimentation différentes résulte des performances réduites ou incohérents lors de l’utilisation de SQL Server.

### <a name="disk-io"></a>E/S disque

Apprentissage et la prédiction de travaux à l’aide de R Services sont, par nature, e/s liée et dépendent de la vitesse des disques que la base de données est stocké sur. Disques plus rapides, telles que les disques SSD () peuvent vous aider.

Les E/S disque sont aussi affectées par les autres applications qui accèdent au disque. Tel est le cas, par exemple, des opérations de lecture dans une base de données qui sont le fait d’autres clients. Les performances d’E/S disque peuvent aussi être affectées par les paramètres du système de fichiers utilisé, notamment la taille de bloc utilisée par le système de fichiers.

Si plusieurs lecteurs sont disponibles, le magasin de bases de données sur un lecteur autre que SQL Server afin que les demandes pour le moteur de base de données ne sont pas atteinte le même disque que demande pour les données stockées dans la base de données.

Par ailleurs, les E/S disque peuvent avoir un impact important sur les performances quand les fonctions d’analyse RevoScaleR en cours d’exécution utilisent plusieurs itérations en phase d’apprentissage. Par exemple, `rxLogit`, `rxDTree`, `rxDForest`, et `rxBTrees` tous utiliser plusieurs itérations. Lorsque la source de données est SQL Server, ces algorithmes utilisent des fichiers temporaires qui sont optimisés pour capturer les données. Ces fichiers sont automatiquement nettoyés en fin de session. Avoir un disque hautes performances pour les opérations de lecture/écriture peut améliorer considérablement la durée globale de ces algorithmes.

> [!NOTE]
> Les versions antérieures de R Services requis prise en charge de nom de fichier au format 8.3 sur les systèmes d’exploitation Windows. Cette restriction a été levée après le Service Pack 1. Toutefois, vous pouvez utiliser fsutil.exe pour déterminer si un lecteur prend en charge les noms de 8.3 fichiers ou pour activer la prise en charge si elle n’est pas le cas.

### <a name="paging-file"></a>Fichier d’échange

Le système d’exploitation Windows utilise un fichier de pagination pour gérer les vidages sur incident et stocker les pages de mémoire virtuelle. Si vous constatez une pagination excessive, envisagez d’augmenter la capacité de la mémoire physique de l’ordinateur. Même si une capacité de mémoire physique supérieure n’est pas propre à éliminer la pagination, elle en réduit la nécessité.

La rapidité du disque sur lequel est stocké le fichier d’échange peut aussi affecter les performances. En stockant le fichier d’échange sur un disque SSD ou en utilisant plusieurs fichiers d’échange sur plusieurs disques SSD, il est possible d’améliorer les performances.

Pour plus d’informations sur le fichier d’échange de dimensionnement, consultez [comment déterminer la taille de fichier de page appropriée pour les versions 64 bits de Windows](https://support.microsoft.com/kb/2860880).

## <a name="optimizations-at-instance-or-database-level"></a>Optimisations au niveau instance ou base de données

Optimisation de l’instance de SQL Server est la clé pour une exécution efficace des scripts externes.

> [!NOTE]
> Les paramètres optimaux diffèrent selon la taille et le type de données, le nombre de colonnes que vous utilisez pour calculer les scores ou d’apprentissage d’un modèle.
> 
> Vous pouvez examiner les résultats des optimisations spécifiques dans le dernier article : [réglage des performances - résultats de l’étude de cas](../../advanced-analytics/r/performance-case-study-r-services.md)
> 
> Pour des exemples de scripts, consultez particulières [référentiel GitHub](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PerfTuning).

### <a name="table-compression"></a>Compression de table

Performances d’e/s peuvent souvent être améliorées en utilisant la compression ou un magasin de données en colonnes. En règle générale, données sont souvent répétées dans plusieurs colonnes dans une table, donc à l’aide d’un index columnstore exploite ces répétitions lors de la compression des données.

Un index columnstore peuvent ne pas être aussi efficace s’il existe de nombreuses insertions dans la table, mais constitue un bon choix si les données sont statiques ou uniquement changent peu fréquemment. Si l’utilisation d’une banque de colonnes n’est pas appropriée, l’activation de la compression sur une table principale de lignes permet d’améliorer les E/S.

Pour plus d’informations, consultez les documents suivants :

+ [Compression de données](../../relational-databases/data-compression/data-compression.md)

+ [Activer la compression sur une table ou un index](../../relational-databases/data-compression/enable-compression-on-a-table-or-index.md)

+ [Guide des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)

### <a name="memory-optimized-tables"></a>Tables optimisées en mémoire

Aujourd'hui, la mémoire n’est plus un problème pour les ordinateurs modernes. À mesure que les spécifications matérielles améliorer, il est relativement facile à obtenir de RAM sur les valeurs appropriées. Toutefois, en même temps, données sont produites plus rapidement qu’auparavant, et les données doivent être traitées avec une faible latence.

Tables mémoire optimisées représentent une solution, dans la mesure où ils tirent parti de la mémoire de grande taille disponible sur les ordinateurs avancées de résoudre le problème de données volumineuses. Tables optimisées en mémoire se trouvent principalement en mémoire, afin que les données sont lues et écrites dans la mémoire. Pour une durabilité, une deuxième copie de la table est conservée sur le disque et les données sont uniquement lues à partir du disque lors de la récupération de la base de données.

Si vous avez besoin lire et écrire dans des tables fréquemment, les tables optimisées en mémoire peuvent aider à haute évolutivité et une faible latence.  Dans le scénario de mise en correspondance de reprise, utilisez des tables mémoire optimisées, nous a permis à lire toutes les fonctionnalités de reprise à partir de la base de données et de les stocker dans la mémoire principale, met en correspondance avec les nouvelles ouvertures de travail. Cela réduit considérablement e/s disque.

Les gains de performances supplémentaires ont été obtenues à l’aide de la table optimisée en mémoire en cours de réécriture des prédictions sur la base de données à partir de plusieurs lots simultanés. L’utilisation de tables mémoire optimisées sur SQL Server activé une latence faible sur les écritures et lectures de la table.

L’expérience a été également transparente au cours du développement. Les tables durables optimisées en mémoire ont été créés en même temps que la base de données a été créé. Par conséquent, le développement utilisé le même flux de travail quel que soit l’emplacement de données.

### <a name="processor"></a>Processeur

SQL Server peut effectuer des tâches en parallèle à l’aide de cœurs disponibles sur l’ordinateur ; davantage de cœurs est disponibles, meilleures sont les performances. Tandis qu’augmenter le nombre de cœurs peut aider à pas pour les opérations d’e/s liée, le processeur de manière intensive avantage des algorithmes de processeurs plus rapides avec un nombre de cœurs.

Étant donné que le serveur est normalement utilisé par plusieurs utilisateurs simultanément, l’administrateur de base de données doit déterminer le nombre idéal de cœurs sont nécessaires pour prendre en charge les calculs de charge de travail maximale.

### <a name="resource-governance"></a>Gouvernance des ressources

Dans les éditions qui prennent en charge le gouverneur de ressources, vous pouvez utiliser des pools de ressources pour spécifier que certaines charges de travail sont allouées à un nombre d’unités centrales. Vous pouvez également gérer la quantité de mémoire allouée aux charges de travail spécifiques.

La gouvernance des ressources dans SQL Server vous permet de centraliser l’analyse et le contrôle des ressources différents utilisés par SQL Server et par R. Par exemple, vous pourrez allouer de la moitié de la mémoire disponible pour le moteur de base de données, pour vous assurer que core services peuvent s’exécuter toujours en dépit de charges de travail plus importante transitoires.

La valeur par défaut pour la consommation de mémoire par les scripts externes est limitée à 20 % de la mémoire totale disponible pour SQL Server lui-même. Cette limite est appliquée par défaut pour vous assurer que toutes les tâches qui s’appuient sur le serveur de base de données ne sont pas sérieusement affectés par les travaux R en cours d’exécution longue. Cependant, l’administrateur de base de données peut modifier ces limites. Dans de nombreux cas, la limite de 20 % n’est pas suffisante pour prendre en charge les charges de travail d’apprentissage grave.

Les options de configuration prises en charge sont **MAX_CPU_PERCENT**, **MAX_MEMORY_PERCENT**, et **MAX_PROCESSES**. Pour afficher les paramètres actuels, utilisez cette instruction : `SELECT * FROM sys.resource_governor_external_resource_pools`

-  Si le serveur est principalement utilisé pour R Services, il peut être utile augmenter MAX_CPU_PERCENT à 40 % ou 60 %.

-  Si le nombre de sessions R doit utiliser le même serveur en même temps, ces trois paramètres doivent être augmentés.

Pour modifier les valeurs de ressource, utilisez les instructions T-SQL.

+ Instruction définit l’utilisation de la mémoire à 40 %. `ALTER EXTERNAL RESOURCE POOL [default] WITH (MAX_MEMORY_PERCENT = 40)`

+ Cette instruction définit les trois valeurs configurables : `ALTER EXTERNAL RESOURCE POOL [default] WITH (MAX_CPU_PERCENT = 40, MAX_MEMORY_PERCENT = 50, MAX_PROCESSES = 20)`

+ Si vous modifiez une mémoire, processeur ou paramètre de processus maximale et que vous souhaitez appliquer les paramètres immédiatement, exécutez cette instruction : `ALTER RESOURCE GOVERNOR RECONFIGURE`

## <a name="soft-numa-hardware-numa-and-cpu-affinity"></a>Affinité Soft-NUMA, le matériel NUMA et du processeur

Lorsque vous utilisez SQL Server comme contexte de calcul, vous pouvez parfois obtenir de meilleures performances en réglant les paramètres liés à l’affinité du processeur et de NUMA. 

Systèmes avec _matériel NUMA_ ont plusieurs bus système, chacun au service d’un petit ensemble de processeurs. Chaque processeur peut accéder à la mémoire associée aux autres groupes de façon cohérente. Chaque groupe est appelé nœud NUMA. Si vous possédez un matériel NUMA, il peut être configuré de façon à utiliser la mémoire entrelacée au lieu de l'accès NUMA. Dans ce cas, Windows et par conséquent, SQL Server seront reconnaît pas comme NUMA. 

Vous pouvez exécuter la requête suivante pour rechercher le nombre de nœuds de mémoire disponibles pour SQL Server :

```SQL
SELECT DISTINCT memory_node_id
FROM sys.dm_os_memory_clerks
```

Si la requête retourne un seul nœud mémoire (nœud 0), vous ne disposez pas de matériel NUMA, soit le matériel est configuré comme entrelacé (non-NUMA). SQL Server ignore également les matériels NUMA lorsqu’il n’y des quatre ou moins de processeurs, ou si au moins un nœud n’a qu’un seul processeur.

Si votre ordinateur dispose de plusieurs processeurs, mais n’a pas de matériel NUMA, vous pouvez également utiliser [Soft-NUMA](https://docs.microsoft.com/sql/database-engine/configure-windows/soft-numa-sql-server) de subdiviser les processeurs en groupes plus petits.  Dans SQL Server 2016 et SQL Server 2017, la fonctionnalité Soft-NUMA est automatiquement activée lors du démarrage du service SQL Server.

Lorsque Soft-NUMA est activée, SQL Server gère automatiquement les nœuds ; Toutefois, pour optimiser les charges de travail spécifiques, vous pouvez désactiver _affinité logicielle_ et configurer manuellement l’affinité du processeur pour les nœuds NUMA logicielles. Cela peut vous donner plus de contrôle sur lequel les charges de travail sont attribués à quels nœuds, en particulier si vous utilisez une édition de SQL Server qui prend en charge la gouvernance des ressources. En spécifiant l’affinité du processeur et alignement des pools de ressources avec des groupes de processeurs, vous pouvez réduire la latence et assurez-vous que les processus connexes sont exécutées dans le même nœud NUMA.

Le processus global de la configuration soft-NUMA et l’affinité du processeur pour prendre en charge les charges de travail R est la suivante :

1. Permettre soft-NUMA, si disponible
2. Définir l’affinité du processeur
3. Créer des pools de ressources pour les processus externes, à l’aide de [du gouverneur de ressources](../r/resource-governance-for-r-services.md)
4. Affecter le [groupes de charges de travail](../../relational-databases/resource-governor/resource-governor-workload-group.md) à des groupes d’affinités spécifiques

Pour plus d’informations, y compris les exemples de code, consultez ce didacticiel : [optimisation conseils et astuces SQL (Ke Huang)](https://gallery.cortanaintelligence.com/Tutorial/SQL-Server-Optimization-Tips-and-Tricks-for-Analytics-Services)

**Autres ressources :**

+ [Soft-NUMA dans SQL Server](https://docs.microsoft.com/sql/database-engine/configure-windows/soft-numa-sql-server)
    
    Comment mapper des nœuds soft-NUMA aux UC

+ [Soft-NUMA automatique : il exécute plus rapide (Bob Ward)](https://blogs.msdn.microsoft.com/bobsql/2016/06/03/sql-2016-it-just-runs-faster-automatic-soft-numa/)

   Décrit l’historique, ainsi que les détails d’implémentation, avec des performances sur les serveurs plus récents de plusieurs cœurs.

## <a name="task-specific-optimizations"></a>Optimisations spécifiques à une tâche

Cette section résume les méthodes dans ces études de cas et dans d’autres tests, pour optimiser les charges de travail d’apprentissage spécifique. Charges de travail courantes incluent l’apprentissage du modèle, de fonctionnalités et l’ingénierie de fonctionnalité et divers scénarios pour calculer les scores : seule ligne, les lots de petite et lot volumineux.

### <a name="feature-engineering"></a>Ingénierie des caractéristiques

Une problématique avec R est qu’il est généralement traité sur un seul processeur. Il s’agit d’un goulot d’étranglement de performances pour de nombreuses tâches, notamment l’ingénierie de fonctionnalité. Dans la solution de mise en correspondance de reprise, la tâche de l’ingénierie de fonctionnalité seule créé 2 500 fonctionnalités entre les produits qui ont dû être combinée avec les fonctionnalités de 100 d’origine. Cette tâche prendrait beaucoup de temps si toutes les opérations effectuées sur un seul processeur.

Il existe plusieurs façons d’améliorer les performances d’ingénierie de fonctionnalité. Vous pouvez optimiser votre code R et conserver l’extraction des fonctions à l’intérieur du processus de modélisation ou déplacer le processus d’ingénierie de fonctionnalité dans SQL.

- À l’aide de R. Vous définissez une fonction et puis passez-le comme argument de [rxTransform](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxtransform) pendant la formation. Si le modèle prend en charge le traitement parallèle, la tâche d’ingénierie de fonctionnalité peut être traitée à l’aide de plusieurs unités centrales. À l’aide de cette approche, l’équipe de science des données observé une amélioration des performances de 16 % en termes de temps de calcul de score. Toutefois, cette approche nécessite un modèle qui prend en charge de la parallélisation et une requête qui peut être exécutée à l’aide d’un plan parallèle.

- Contexte de calcul utilisent les R avec SQL. Dans un environnement multiprocesseur avec isolé de ressources disponible pour l’exécution de lots distincts, vous pouvez obtenir une meilleure efficacité en isolant les requêtes SQL utilisés pour chaque lot, pour extraire des données à partir des tables et de limiter les données sur le même groupe de charges de travail. Méthodes utilisées pour isoler les lots incluent le partitionnement et utilisent de PowerShell pour exécuter des requêtes distinctes en parallèle.

- L’exécution en parallèle ad hoc : contexte de calcul dans un SQL Server, vous pouvez compter sur le moteur de base de données SQL pour appliquer l’exécution en parallèle si possible et si cette option est plus efficace.

- Utilisez T-SQL dans un processus de génération de fonctionnalités distincts. Precomputing les données de fonctionnalité à l’aide de SQL est généralement plus rapide.

### <a name="prediction-scoring-in-parallel"></a>Prédiction (score) en parallèle

Un des avantages de SQL Server est sa capacité à gérer un grand nombre de lignes en parallèle. Nulle part est cet avantage marqué comme dans le calcul de score. En règle générale le modèle n’a pas besoin l’accès à toutes les données pour calculer les scores, vous pouvez partitionner les données d’entrée, à chaque groupe de charges de travail du traitement d’une tâche.

Vous pouvez également envoyer les données d’entrée comme une seule requête, et SQL Server analyse ensuite la requête. Si un plan de requête parallèle peut être créé pour les données d’entrée, il automatiquement partitionne les données affectées aux nœuds et effectue des jointures nécessaires et les agrégations en parallèle ainsi.

Si vous êtes intéressé par les détails de la définition d’une procédure stockée pour une utilisation dans les scores, consultez l’exemple de projet sur [GitHub](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/SQLOptimizationTips/SQLR) et recherchez le fichier « step5_score_for_matching.sql ». L’exemple de script également début de requête assure le suivi et les heures de fin et les écritures de la durée de la console SQL afin que vous pouvez évaluer les performances.

### <a name="concurrent-scoring-using-resource-groups"></a>Calcul de score simultanées à l’aide de groupes de ressources

Pour faire évoluer le problème de calcul de score, la meilleure pratique est d’adopter l’approche Map/reduce dans lequel des millions d’éléments sont divisées en plusieurs lots. Ensuite, plusieurs tâches de calcul de score sont exécutées simultanément. Dans ce cadre, les lots sont traités sur différents ensembles de l’UC, et les résultats sont collectées et mis à jour dans la base de données.

C’est l’approche utilisée dans le scénario de mise en correspondance de reprise ; Toutefois, la gouvernance des ressources dans SQL Server sont essentielle pour l’implémentation de cette approche. En configurant des groupes de charges de travail pour les tâches de script externe, vous pouvez acheminer les travaux de calcul de score à différents groupes de processeurs de R et obtenir un débit plus rapide.

Gouvernance des ressources peuvent également aider à allouer de répartir les ressources disponibles sur le serveur (processeur et mémoire) pour réduire la concurrence de la charge de travail. Vous pouvez configurer des fonctions classifieur pour distinguer les différents types de travaux R : par exemple, vous pouvez décider que score appelé à partir d’une application est toujours prioritaire, tandis que les travaux reconversion ont une priorité basse. Cette isolation ressource peut potentiellement améliorer les temps d’exécution et fournir des performances plus prévisibles.

### <a name="concurrent-scoring-using-powershell"></a>Calcul de score simultanées à l’aide de PowerShell

Si vous décidez de partitionner les données vous-même, vous pouvez utiliser des scripts PowerShell d’exécuter plusieurs tâches de calcul de score simultanées. Pour ce faire, utilisez l’applet de commande Invoke-SqlCmd et lancer les tâches de calcul de score en parallèle.

Dans le scénario de mise en correspondance de reprise, la concurrence a été conçue comme suit :

- 20 processeurs divisé en quatre groupes de cinq processeurs. Chaque groupe d’unités centrales se trouve sur le même nœud NUMA.

- Nombre maximal de lots simultanés a été défini à huit.

- Chaque groupe de charges de travail doit gérer les deux tâches de calcul de score. Dès qu’une tâche a fini de lire des données et calculer les scores de démarrage, l’autre tâche peut commencer la lecture des données à partir de la base de données.

Pour voir les scripts PowerShell pour ce scénario, ouvrez le fichier experiment.ps1 dans le [Github projet](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/SQLOptimizationTips).

### <a name="storing-models-for-prediction"></a>Le stockage des modèles pour la prédiction

Lors de la formation et d’évaluation terminée et que vous avez sélectionné un meilleur modèle, nous vous recommandons de stocker le modèle dans la base de données afin qu’il soit disponible pour les prévisions. Le chargement du modèle précalculé à partir de la base de données pour la prédiction est efficace, car l’apprentissage de SQL Server utilise des algorithmes de sérialisation spéciale pour stocker et charger des modèles lors du déplacement entre R et la base de données.

> [!TIP]
> Dans SQL Server 2017, vous pouvez utiliser la fonction de prédiction pour effectuer le calcul du score du même si R n’est pas installé sur le serveur. Types de modèles limitée sont pris en charge à partir du package RevoScaleR.

Toutefois, selon l’algorithme que vous utilisez, certains modèles peuvent être très grandes, en particulier lorsque formé sur un jeu de données volumineux. Par exemple, les algorithmes tels que **lm** ou **glm** générer un grand nombre de données de synthèse et des règles. Car il existe des limites de la taille d’un modèle qui peut être stockée dans une colonne varbinary, nous vous recommandons d’éliminer les artefacts inutiles à partir du modèle avant de stocker le modèle dans la base de données de production.

## <a name="articles-in-this-series"></a>Articles de cette série

[Performances du paramétrage des R – introduction](../r/sql-server-r-services-performance-tuning.md)

[Réglage des performances pour R - configuration de SQL Server](../r/sql-server-configuration-r-services.md)

[Réglage des performances pour R - R optimisation de code et les données](../r/r-and-data-optimization-r-services.md)

[Réglage des performances - résultats de l’étude de cas](../r/performance-case-study-r-services.md)
