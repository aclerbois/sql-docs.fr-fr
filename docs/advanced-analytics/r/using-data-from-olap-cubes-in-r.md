---
title: À l’aide des données à partir de cubes OLAP dans R (SQL Server Machine Learning) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 0753fc84ea6516da63e1e49dc68063780b99361b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="using-data-from-olap-cubes-in-r"></a>À l’aide des données à partir de cubes OLAP dans R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Le **olapR** package est un package R, fourni par Microsoft pour une utilisation avec Machine Learning Server et SQL Server, qui vous permet d’exécuter les requêtes MDX pour obtenir des données à partir de cubes OLAP. Avec ce package, vous n’avez pas besoin de créer des serveurs liés ou de nettoyage des ensembles de lignes aplati ; Vous pouvez obtenir des données OLAP directement à partir de R.

Cet article décrit l’API, ainsi que d’une vue d’ensemble de OLAP et MDX pour les utilisateurs de R peut être de nouveau aux bases de données de cube multidimensionnel.

> [!IMPORTANT]
> Une instance d’Analysis Services peut prendre en charge des cubes multidimensionnels classiques, ou dans les modèles tabulaires, mais une instance ne peut pas prendre en charge deux types de modèles. Par conséquent, avant de tenter de générer une requête MDX sur un cube, vérifiez que l’instance Analysis Services contient les modèles multidimensionnels.

## <a name="what-is-an-olap-cube"></a>Qu’est un cube OLAP ?

OLAP est l’abréviation de traitement analytique en ligne. Des solutions OLAP sont couramment utilisées pour capturer et stocker des données critiques au fil du temps. Les données OLAP sont utilisées pour l’analytique d’entreprise par différents outils, tableaux de bord et visualisations. Pour plus d’informations, consultez [traitement analytique en ligne](https://en.wikipedia.org/wiki/Online_analytical_processing).

Microsoft fournit [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services), ce qui vous permet de concevoir, déployer et interroger des données OLAP sous la forme de _cubes_ ou _les modèles tabulaires_. Un cube est une base de données multidimensionnel. _Dimensions_ ressemblent à facettes des données, ou facteurs dans r : dimensions vous permet d’identifier un sous-ensemble spécifique de données que vous souhaitez synthétiser ou analyser. Par exemple, le temps est une dimension importante, bien afin que de nombreuses solutions OLAP incluent plusieurs calendriers définis par défaut, à utiliser lors de la segmentation et de résumer les données. 

Pour des raisons de performances, une base de données OLAP calcule souvent résumés (ou _agrégations_) à l’avance, puis les stocke pour une récupération plus rapide. Récapitulatifs sont basées sur *mesures*, qui représentent des formules qui peuvent être appliquées à des données numériques. Vous utilisez les dimensions pour définir un sous-ensemble de données et vous devez calculez la mesure sur ces données. Par exemple, vous utiliseriez une mesure pour calculer le total des ventes pour une ligne de produit spécifique sur plusieurs trimestres moins taxes, pour signaler les coûts d’expédition moyenne pour un fournisseur particulier, year-to-date cumulatives salaires et ainsi de suite.

MDX, abréviation d’expressions multidimensionnelles, est la langue utilisée pour interroger des cubes. En règle générale, une requête MDX contient une définition de données qui inclut une ou plusieurs dimensions et au moins une mesure, bien que les requêtes MDX peuvent obtenir beaucoup plus complexes et inclure windows propagées, cumulatives moyennes, des sommes, rangs ou centiles. 

Voici d’autres termes qui peuvent être utiles lorsque vous commencez à créer des requêtes MDX :

+ *Le découpage* prend un sous-ensemble du cube à l’aide de valeurs à partir d’une seule dimension.

+ Le*découpage en cubes* crée un sous-cube en spécifiant une plage de valeurs sur plusieurs dimensions.

+ La*descente dans la hiérarchie* permet de naviguer d’un résumé vers les détails.

+ La*montée dans la hiérarchie* permet de passer des détails à un niveau d’agrégation plus élevé.

+ Le*regroupement* résume les données sur une dimension.

+ La*rotation* faire pivoter le cube ou la sélection de données.

## <a name="how-to-use-olapr-to-create-mdx-queries"></a>L’utilisation des olapR pour créer des requêtes MDX

L’article suivant fournit des exemples détaillés de la syntaxe de création ou de l’exécution de requêtes sur un cube :

+ [Comment créer des requêtes MDX à l’aide de R](../../advanced-analytics/r/how-to-create-mdx-queries-using-olapr.md)

## <a name="olapr-api"></a>olapR API

Le package **olapR** prend en charge deux méthodes de création de requêtes MDX :

- **Utilisez le Générateur MDX.** Utilisez les fonctions R dans le package pour générer une requête MDX simple, en choisissant un cube, puis en définissant des segments et les axes. Il s’agit d’un moyen simple pour créer une requête MDX valide, si vous n’avez pas accès à des outils traditionnels OLAP, ou que vous ne disposez pas des connaissances approfondies du langage MDX.

    Toutes les requêtes MDX peuvent être créés à l’aide de cette méthode, MDX peut être complexe. Toutefois, cette API prend en charge la plupart des opérations plus courantes et utiles, y compris tranche, dés, exploration vers le bas, rollup et pivot dans les dimensions de N.

+ **Copier-coller MDX bien formé.** Créer manuellement, puis les coller dans une requête MDX. Cette option est la meilleure si vous avez des requêtes MDX existants que vous souhaitez réutiliser, ou si la requête que vous souhaitez générer est trop complexe pour **olapR** à gérer.

    Après avoir généré votre MDX à l’aide d’un utilitaire client, tels que SSMS ou Excel, enregistrez la chaîne de requête. Fournir cette chaîne MDX en tant qu’argument à la *Gestionnaire de requête SSAS* dans les **olapR** package. Le fournisseur envoie la requête au serveur Analysis Services spécifié et passe à nouveau les résultats sur R. 

Pour obtenir des exemples de génération MDX de requête ou exécuter une requête MDX existante, voir [comment créer des requêtes MDX à l’aide de R](../../advanced-analytics/r/how-to-create-mdx-queries-using-olapr.md).

## <a name="known-issues"></a>Problèmes connus

Cette section répertorie certains problèmes connus et les questions courantes sur la **olapR** package.

### <a name="tabular-model-support"></a>Prise en charge de modèle tabulaire

Si vous vous connectez à une instance d’Analysis Services qui contient un modèle tabulaire, le `explore` fonction signale une réussite avec une valeur de retour de la valeur TRUE. Toutefois, les objets de modèle tabulaire sont différents des objets multidimensionnels, et la structure d’une base de données multidimensionnelle est différente de celle d’un modèle tabulaire.

Bien que DAX (analyse des données Expressions) est le langage généralement utilisé avec les modèles tabulaires, vous pouvez concevoir des requêtes MDX valides par rapport à un modèle tabulaire, si vous êtes déjà familiarisé avec MDX. Vous ne pouvez pas utiliser les constructeurs olapR pour générer des requêtes MDX valides par rapport à un modèle tabulaire.

Toutefois, les requêtes MDX sont inefficace permet d’extraire des données à partir d’un modèle tabulaire. Si vous avez besoin obtenir des données à partir d’un modèle tabulaire pour une utilisation dans R, envisagez les méthodes à la place :

+ Activez le mode DirectQuery sur le modèle et ajouter le serveur comme serveur lié dans SQL Server. 
+ Si le modèle tabulaire a été créé sur un relationnelle mini-data warehouse, obtenez les données directement à partir de la source.

### <a name="how-to-determine-whether-an-instance-contains-tabular-or-multidimensional-models"></a>Comment déterminer si une instance contient des modèles tabulaires ou multidimensionnels

Une seule instance d’Analysis Services peut contenir qu’un seul type de modèle, si elle peut contenir plusieurs modèles. La raison est qu’il existe des différences fondamentales entre les modèles tabulaires et les modèles multidimensionnels contrôlant la façon dont les données sont stockées et traitées. Par exemple, les modèles tabulaires sont stockées en mémoire et tirer parti des index columnstore pour effectuer des calculs très rapides. Dans les modèles multidimensionnels, les données sont stockées sur le disque et les agrégations sont définies à l’avance et récupérées à l’aide de requêtes MDX.

Si vous vous connectez à Analysis Services à l’aide d’un client tel que SQL Server Management Studio, vous pouvez indiquer un coup de œil le type de modèle est pris en charge, en examinant l’icône de la base de données.

Vous pouvez également afficher et interroger les propriétés du serveur pour déterminer le type de modèle prend en charge de l’instance. Le **mode serveur** propriété prend en charge les deux valeurs : _multidimensionnels_ ou _tabulaire_.

Consultez l’article suivant pour obtenir des informations générales sur les deux types de modèles :

+ [Comparaison des modèles multidimensionnels et tabulaires](https://docs.microsoft.com/sql/analysis-services/comparing-tabular-and-multidimensional-solutions-ssas)

Consultez l’article suivant pour plus d’informations sur l’interrogation des propriétés du serveur :

+ [OLE DB pour OLAP Schema Rowsets](https://docs.microsoft.com/sql/analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets)

### <a name="writeback-is-not-supported"></a>L’écriture différée n’est pas pris en charge.

Il n’est pas possible de réécrire les résultats des calculs de R personnalisés dans le cube.

En général, même si un cube est activé pour l’écriture différée, seules les opérations limitées sont pris en charge, et une configuration supplémentaire peut être nécessaire. Nous vous recommandons d’utiliser MDX pour ces opérations.

+ [Dimensions activées en écriture](https://docs.microsoft.com/sql/analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions)
+ [Partitions activées en écriture](https://docs.microsoft.com/sql/analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-write-enabled-partitions)
+ [Définir l’accès personnalisés aux données des cellules](https://docs.microsoft.com/sql/analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services)

### <a name="long-running-mdx-queries-block-cube-processing"></a>Bloquent les requêtes MDX à long terme du traitement du cube

Bien que le **olapR** package effectue uniquement des opérations de lecture, longues requêtes MDX peuvent créer des verrous qui empêchent le cube à partir d’en cours de traitement. Testez toujours vos requêtes MDX à l’avance afin que vous sachiez que la quantité de données doit être retournée.

Si vous essayez de vous connecter à un cube est verrouillé, vous pouvez obtenir une erreur de l’entrepôt de données SQL Server ne peut pas être atteint. Les solutions suggérées incluent l’activation des connexions à distance, la vérification du serveur ou nom de l’instance et ainsi de suite ; Toutefois, envisagez la possibilité d’une connexion ouverte précédente.

Un administrateur SSAS peut empêcher des problèmes de verrouillage en identifiant et terminer les sessions ouvertes. Une propriété de délai d’attente peut également être appliquée à des requêtes MDX au niveau du serveur pour forcer l’arrêt de toutes les requêtes longues.

## <a name="resources"></a>Ressources

Si vous ne connaissez OLAP ou à des requêtes MDX, consultez ces articles de Wikipedia : 

+ [Cubes OLAP](https://en.wikipedia.org/wiki/OLAP_cube)
+ [Requêtes MDX](https://en.wikipedia.org/wiki/MultiDimensional_eXpressions)
