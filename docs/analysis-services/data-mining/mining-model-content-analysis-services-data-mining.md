---
title: Contenu du modèle d’exploration de données (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7ecf592968e6bd025a0096d0ed3369029cbf4eec
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mining-model-content-analysis-services---data-mining"></a>Contenu du modèle d’exploration de données (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après avoir conçu et traité un modèle d'exploration de données à l'aide de données provenant de la structure d'exploration de données sous-jacente, celui-ci est complet et présente un *contenu de modèle d'exploration de données*. Vous pouvez utiliser ce contenu pour faire des prédictions ou analyser vos données.  
  
 Le contenu du modèle d'exploration de données inclut des métadonnées relatives au modèle, des statistiques sur les données, et les modèles découverts par l'algorithme d'exploration. Selon l'algorithme utilisé, le contenu du modèle peut inclure des formules de régression, les définitions de règles et jeux d'éléments, ou des pondérations et d'autres statistiques.  
  
 Indépendamment de l'algorithme utilisé, le contenu du modèle d'exploration de données est présenté dans une structure standard. Vous pouvez parcourir la structure dans la Visionneuse de l’arborescence de contenu générique Microsoft, fournie dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], puis basculer sur l’une des visionneuses personnalisées pour voir comment les informations sont interprétées et affichées graphiquement pour chaque type de modèle. Vous pouvez aussi créer des requêtes par rapport au contenu du modèle d'exploration de données en utilisant un client qui prend en charge l'ensemble de lignes de schéma MINING_MODEL_CONTENT. Pour plus d’informations, consultez [Tâches de requête d’exploration de données et procédures](../../analysis-services/data-mining/data-mining-query-tasks-and-how-tos.md).  
  
 Cette section décrit la structure de base du contenu fournie pour tous les types de modèles d'exploration de données. Elle décrit les types de nœuds qui sont communs à tous les modèles d'exploration de données et explique comment interpréter les informations.  
  
 [Structure du contenu du modèle d'exploration de données](#bkmk_Structure)  
  
 [Nœuds dans le contenu du modèle](#bkmk_Nodes)  
  
 [Contenu du modèle d'exploration de données par type d'algorithme](#bkmk_AlgoType)  
  
 [Outils pour afficher le contenu du modèle d'exploration de données](#bkmk_Viewing)  
  
 [Outils pour interroger le contenu du modèle d'exploration de données](#bkmk_Querying)  
  
##  <a name="bkmk_Structure"></a> Structure du contenu du modèle d'exploration de données  
 Le contenu de chaque modèle est présenté sous la forme d'une série de *nœuds*. Un nœud est un objet dans un modèle d'exploration de données qui contient des métadonnées et des informations sur une partie du modèle. Les nœuds sont organisés dans une hiérarchie. La disposition exacte des nœuds dans la hiérarchie, et la signification de celle-ci, dépend de l'algorithme que vous avez utilisé. Par exemple, si vous créez un modèle d'arbres de décision, le modèle peut contenir plusieurs arborescences, toutes connectées à la racine modèle ; si vous créez un modèle de réseau neuronal, le modèle peut contenir un ou plusieurs réseaux, plus un nœud de statistiques.  
  
 Le premier nœud dans chaque modèle est appelé le *nœud racine*, ou le nœud *parent du modèle* . Chaque modèle a un nœud racine (NODE_TYPE = 1). Le nœud racine contient généralement certaines métadonnées sur le modèle, et le nombre de nœuds enfants, mais il dispose de peu d'informations supplémentaires sur les modèles découverts par le modèle.  
  
 Selon l'algorithme utilisé pour créer le modèle, le nœud racine a un nombre variable de nœuds enfants. Les nœuds enfants ont des significations différentes et possèdent un contenu différent en fonction de l'algorithme, de la profondeur et de la complexité des données.  
  
##  <a name="bkmk_Nodes"></a> Nœuds dans le contenu du modèle d'exploration de données  
 Dans un modèle d'exploration de données, un nœud est un conteneur à usage général qui stocke une information sur l'ensemble ou une portion du modèle. La structure de chaque nœud est toujours la même et contient les colonnes définies par l'ensemble des lignes du schéma d'exploration de données. Pour plus d’informations, consultez [Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-model-content-rowset.md).  
  
 Chaque nœud inclut des métadonnées relatives au nœud, notamment un identificateur unique dans chaque modèle, l'ID du nœud parent et le nombre de nœuds enfants que possède le nœud. Les métadonnées identifient le modèle d'appartenance du nœud, et le catalogue de la base de données où ce modèle particulier est stocké. Le contenu supplémentaire fourni dans le nœud diffère selon le type d'algorithme utilisé pour créer le modèle, il peut contenir les éléments suivants :  
  
-   Nombre de cas figurant dans les données d’apprentissage qui prend en charge une valeur prédite particulière.  
  
-   Statistiques, telles que la moyenne, l'écart type ou la variance.  
  
-   Coefficients et formules.  
  
-   Définition de règles et pointeurs latéraux.  
  
-   Fragments XML qui décrivent une partie du modèle.  
  
### <a name="list-of-mining-content-node-types"></a>Liste des types de nœuds de contenu d'exploration de données  
 Le tableau suivant répertorie les différents types de nœud générés dans les modèles d'exploration de données. Comme chaque algorithme traite les informations différemment, chaque modèle génère seulement certains types spécifiques de nœuds. Si vous modifiez l'algorithme, le type de nœud peut changer. De plus, si vous retraitez le modèle, le contenu de chaque nœud peut changer.  
  
> [!NOTE]  
>  Si vous utilisez un service d’exploration de données différent, ou si vous créez vos propres algorithmes de plug-in, les types de nœuds personnalisés supplémentaires peuvent être disponibles.  
  
|NODE_TYPE ID|Étiquette de nœud|Contenu de nœud|  
|-------------------|----------------|-------------------|  
|1|Modèle|Nœud de métadonnées et de contenu racine. S'applique à tous les types de modèle.|  
|2|Arborescence|Nœud racine d'un arbre de classification. S'applique aux modèles d'arbre de décision|  
|3|Intérieur|Nœud fractionné intérieur dans une arborescence. S'applique aux modèles d'arbre de décision|  
|4|Distribution|Nœud de terminaison d'une arborescence. S'applique aux modèles d'arbre de décision|  
|5|Cluster|Cluster détecté par l'algorithme. S'applique aux modèles de clustering et aux modèles Sequence Clustering.|  
|6|Unknown|Type de nœud inconnu.|  
|7|ItemSet|Jeu d'éléments détecté par l'algorithme. S'applique aux modèles d'association ou aux modèles Sequence Clustering.|  
|8|AssociationRule|Règle d'association détectée par l'algorithme. S'applique aux modèles d'association ou aux modèles Sequence Clustering.|  
|9|PredictableAttribute|Attribut prévisible. S'applique à tous les types de modèle.|  
|10|InputAttribute|Attribut d'entrée. S'applique aux arbres de décision et aux modèles Naïve Bayes.|  
|11|InputAttributeState|Statistiques à propos des états d'un attribut d'entrée. S'applique aux arbres de décision et aux modèles Naïve Bayes.|  
|13|Séquence|Nœud supérieur pour un composant de modèle de Markov d'un cluster de séquence. S'applique aux modèles Sequence Clustering.|  
|14|Transition|Matrice de transition Markov. S'applique aux modèles Sequence Clustering.|  
|15|TimeSeries|Nœud non racine d'un arbre de série chronologique. S'applique aux modèles de série chronologique uniquement.|  
|16|TsTree|Nœud racine d'un arbre de série chronologique qui correspond à une série chronologique prévisible. S'applique aux modèles de série chronologique, et uniquement si le modèle a été créé à l'aide du paramètre MIXED.|  
|17|NNetSubnetwork|Un sous-réseau. S'applique aux modèles de réseau neuronal.|  
|18|NNetInputLayer|Groupe qui contient les nœuds de la couche d'entrée. S'applique aux modèles de réseau neuronal.|  
|19|NNetHiddenLayer|Groupes qui contiennent les nœuds qui décrivent la couche masquée. S'applique aux modèles de réseau neuronal.|  
|21|NNetOutputLayer|Groupes qui contiennent les nœuds de la couche de sortie. S'applique aux modèles de réseau neuronal.|  
|21|NNetInputNode|Nœud dans la couche d'entrée qui fait correspondre un attribut d'entrée aux états correspondants. S'applique aux modèles de réseau neuronal.|  
|22|NNetHiddenNode|Nœud de la couche masquée. S'applique aux modèles de réseau neuronal.|  
|23|NNetOutputNode|Nœud de la couche de sortie. Ce nœud correspond habituellement à un attribut de sortie et aux états correspondants. S'applique aux modèles de réseau neuronal.|  
|24|NNetMarginalNode|Statistiques marginales sur le jeu d'apprentissage. S'applique aux modèles de réseau neuronal.|  
|25|RegressionTreeRoot|Racine d'un arbre de régression. S'applique aux modèles de régression linéaire et aux modèles d'arbres de décision qui contiennent des attributs d'entrée continus.|  
|26|NaiveBayesMarginalStatNode|Statistiques marginales sur le jeu d'apprentissage. S'applique aux modèles Naïve Bayes.|  
|27|ArimaRoot|Nœud racine d'un modèle ARIMA. S'applique uniquement aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|28|ArimaPeriodicStructure|Structure périodique dans un modèle ARIMA. S'applique uniquement aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|29|ArimaAutoRegressive|Coefficient autorégressif pour un terme unique dans un modèle ARIMA.<br /><br /> S'applique uniquement aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|30|ArimaMovingAverage|Coefficient de moyenne mobile pour un terme unique dans un modèle ARIMA. S'applique uniquement aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|1000|CustomBase|Point de départ pour les types de nœuds personnalisés. Les types de nœuds personnalisés doivent être des entiers supérieurs en valeur à cette constante. S'applique aux modèles créés à l'aide des algorithmes de plug-in personnalisés.|  
  
### <a name="node-id-name-caption-and-description"></a>ID, nom, légende et description du nœud  
 Le nœud racine de tout modèle est toujours affecté de l’ID unique (**NODE_UNIQUE_NAME**) de 0. Tous les ID de nœud sont attribués automatiquement par Analysis Services et ne peuvent pas être modifiés.  
  
 Le nœud racine pour chaque modèle contient également des métadonnées de base relatives au modèle. Ces métadonnées incluent la base de données Analysis Services qui contient le modèle (**MODEL_CATALOG**), le schéma (**MODEL_SCHEMA)** et le nom du modèle (**MODEL_NAME)**. Toutefois, ces informations sont répétées dans tous les nœuds du modèle, donc il n'est pas nécessaire d'interroger le nœud racine pour obtenir ces métadonnées.  
  
 En plus d’un nom utilisé comme identificateur unique, chaque nœud a un *nom* (**NODE_NAME**). Ce nom est créé automatiquement par l'algorithme à des fins d'affichage et ne peut pas être modifié.  
  
> [!NOTE]  
>  L'algorithme de gestion de clusters Microsoft permet aux utilisateurs d'attribuer des noms conviviaux à chaque cluster. Toutefois, ces noms conviviaux ne sont pas conservés sur le serveur, et si vous retraitez le modèle, l'algorithme génère de nouveaux noms de cluster.  
  
 La *légende* et *description* de chaque nœud sont générées automatiquement par l'algorithme et servent d'étiquettes pour vous aider à comprendre le contenu du nœud. Le texte généré pour chaque champ dépend du type de modèle. Dans certains cas, le nom, la légende et la description peuvent contenir la même chaîne exactement, mais dans certains modèles, la description peut contenir des informations supplémentaires. Consultez la rubrique sur le type de modèle individuel pour des informations sur l'implémentation.  
  
> [!NOTE]  
>  Le serveur Analysis Services prend en charge le changement de nom des nœuds seulement si vous générez des modèles à l'aide d'un algorithme de plug-in personnalisé qui implémente le changement de nom. Pour permettre le changement de nom, vous devez substituer les méthodes lorsque vous créez l'algorithme de plug-in.  
  
### <a name="node-parents-node-children-and-node-cardinality"></a>Parents du nœud, enfants du nœud et cardinalité du nœud.  
 La relation entre nœuds parents et enfants dans une arborescence est déterminée par la valeur de la colonne PARENT_UNIQUE_NAME. Cette valeur est stockée dans le nœud enfant et vous indique l'ID du nœud parent. Les exemples suivants illustrent la manière dont ces informations peuvent être utilisées :  
  
-   Un PARENT_UNIQUE_NAME qui est NULL indique que le nœud est le nœud supérieur du modèle.  
  
-   Si la valeur de PARENT_UNIQUE_NAME est égale à 0, le nœud doit être un descendant direct du nœud supérieur dans le modèle. En effet, l'ID du nœud racine est toujours 0.  
  
-   Vous pouvez utiliser des fonctions dans une requête DMX (Data Mining Extensions) pour rechercher les descendants ou les parents d'un nœud particulier. Pour plus d’informations sur l’utilisation de fonctions dans des requêtes, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
 La*cardinalité* fait référence au nombre d'éléments dans un ensemble. Dans le contexte d'un modèle d'exploration de données traité, la cardinalité indique le nombre d'enfants dans un nœud particulier. Par exemple, si un modèle d'arbre de décision a un nœud pour [Yearly Income], et ce nœud a deux nœuds enfants, un pour la condition [Yearly Income] = Élevé et un pour la condition, [Yearly Income] = Bas, la valeur de CHILDREN_CARDINALITY pour le nœud [Yearly Income] serait 2.  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], seuls les nœuds enfants immédiats sont comptés lors du calcul de la cardinalité d'un nœud. Toutefois, si vous créez un algorithme de plug-in personnalisé, vous pouvez surcharger CHILDREN_CARDINALITY pour compter la cardinalité différemment. Cela peut s'avérer utile, par exemple, si vous souhaitez compter le nombre total de descendants, pas juste les enfants immédiats.  
  
 Même si la cardinalité est comptée de la même façon pour tous les modèles, votre interprétation ou l'utilisation de la valeur de cardinalité diffère selon le type de modèle. Par exemple, dans un modèle de clustering, la cardinalité du nœud supérieur indique le nombre total de clusters trouvés. Dans d'autres types de modèles, la cardinalité peut avoir toujours une valeur définie selon le type de nœud. Pour plus d'informations sur la manière d'interpréter la cardinalité, consultez la rubrique sur le type de modèle individuel.  
  
> [!NOTE]  
>  Certains modèles, tels que ceux créés par l'algorithme MNR (Microsoft Neural Network), contiennent aussi un type de nœud spécial qui fournit des statistiques descriptives sur les données d'apprentissage du modèle entier. Par définition, ces nœuds n'ont jamais de nœuds enfants.  
  
### <a name="node-distribution"></a>node distribution  
 La colonne NODE_DISTRIBUTION contient une table imbriquée qui fournit dans de nombreux nœuds des informations importantes et détaillées sur les modèles découverts par l'algorithme. Les statistiques exactes fournies dans cette table changent selon le type de modèle, la position du nœud dans l'arborescence, et si l'attribut prévisible est une valeur numérique continue ou une valeur discrète ; toutefois, elles peuvent inclure les valeurs minimales et maximales d'un attribut, les pondérations attribuées aux valeurs, le nombre de cas dans un nœud, les coefficients utilisés dans une formule de régression, et les mesures statistiques telles que l'écart type et la variance. Pour plus d'informations sur la manière d'interpréter la distribution de nœud, consultez la rubrique pour le type spécifique du type de modèle que vous utilisez.  
  
> [!NOTE]  
>  La table NODE_DISTRIBUTION peut être vide, selon le type de nœud. Par exemple, certains nœuds servent uniquement à organiser une collection de nœuds enfants, et ce sont les nœuds enfants qui contiennent les statistiques détaillées.  
  
 La table NODE_DISTRIBUTION imbriquée contient toujours les colonnes ci-dessous. Le contenu de chaque colonne varie selon le type de modèle. Pour plus d'informations sur des types de modèle spécifiques, consultez [Contenu du modèle d'exploration de données par type d'algorithme](#bkmk_AlgoType).  
  
 ATTRIBUTE_NAME  
 Le contenu varie par algorithme. Peut être le nom d'une colonne, telle qu'un attribut prévisible, une règle, un jeu d'éléments ou une information interne à l'algorithme, telle que la partie d'une formule.  
  
 Cette colonne peut également contenir une paire attribut/valeur.  
  
 ATTRIBUTE_VALUE  
 Valeur de l'attribut nommé dans ATTRIBUTE_NAME.  
  
 Si le nom d'attribut est une colonne, puis dans le cas le plus simple, ATTRIBUTE_VALUE contient une des valeurs discrètes pour cette colonne.  
  
 Selon le mode de traitement des valeurs par l’algorithme, ATTRIBUTE_VALUE peut également contenir un indicateur qui indique si une valeur existe pour l’attribut (**Existing**), ou si la valeur est NULL (**Missing**).  
  
 Par exemple, si votre modèle est configuré pour rechercher des clients qui ont acheté au moins une fois un article particulier, la colonne ATTRIBUTE_NAME peut contenir la paire attribut/valeur qui définit l’article en question, telle que `Model = 'Water bottle'`, et la colonne ATTRIBUTE_VALUE contiendrait uniquement le mot clé **Existing** ou **Missing**.  
  
 SUPPORT  
 Nombre de cas qui ont cette paire attribut/valeur, ou qui contiennent ce jeu d'éléments ou règle.  
  
 En général, pour chaque nœud, la valeur de support indique combien de cas dans le jeu d'apprentissage sont inclus dans le nœud actuel. Dans la plupart des types de modèle, le support représente un nombre exact de cas. Les valeurs de support sont utiles parce que vous pouvez consulter la distribution de données dans vos cas d'apprentissage sans devoir interroger les données d'apprentissage. Le serveur Analysis Services utilise également ces valeurs stockées pour calculer la probabilité stockée par rapport à la probabilité antérieure pour déterminer si l'inférence est forte ou faible.  
  
 Par exemple, dans un arbre de classification, la valeur de support indique le nombre de cas qui possèdent la combinaison d'attributs décrite.  
  
 Dans un arbre de décision, la somme de support à chaque niveau d'une arborescence est égale au support de son nœud parent. Par exemple, si un modèle qui contient 1200 cas est également divisé par genre, puis est sous-divisé par trois valeurs pour Revenu (Income) (Bas, Moyen et Élevé), les nœuds enfants du nœud (2), qui sont les nœuds (4), (5) et (6), sont toujours égaux au même nombre de cas que le nœud (2).  
  
|ID de nœud et attributs de nœud|Nombre de supports|  
|---------------------------------|-------------------|  
|(1) Racine du modèle|1200|  
|(2) Sexe = Homme<br /><br /> (3) Sexe = Femme|600<br /><br /> 600|  
|(4) Sexe = Home et Revenu = Élevé<br /><br /> (5) Sexe = Homme et Revenu = Moyen<br /><br /> (6) Sexe = Homme et Revenu = Bas|200<br /><br /> 200<br /><br /> 200|  
|(7) Sexe = Femme et Revenu = Élevé<br /><br /> (8) Sexe = Femme et Revenu = Moyen<br /><br /> (9) Sexe = Femme et Revenu = Bas|200<br /><br /> 200<br /><br /> 200|  
  
 Pour un modèle de clustering, le nombre du support peut être pondéré pour inclure les probabilités d'appartenance à plusieurs clusters. L'appartenance à plusieurs clusters est la méthode de clustering par défaut. Dans ce scénario, comme chaque cas n'appartient pas nécessairement à un seul et unique cluster, le support dans ces modèles peut ne pas atteindre 100 pour cent sur l'ensemble des clusters.  
  
 PROBABILITY  
 Indique la probabilité pour ce nœud spécifique dans le modèle entier.  
  
 En général, la probabilité représente le support pour cette valeur particulière, divisée par le nombre total de cas dans le nœud (NODE_SUPPORT).  
  
 Toutefois, la probabilité est ajustée légèrement pour éliminer l'écart provoqué par des valeurs manquantes dans les données.  
  
 Par exemple, si les valeurs actuelles pour [Enfants Totaux] sont 'Un' et 'Deux', il est souhaitable d'éviter de créer un modèle qui prédit qu'il est impossible de n'avoir pas d'enfants, ou d'avoir trois enfants. Pour garantir que les valeurs manquantes sont improbables, mais pas impossibles, l'algorithme ajoute toujours 1 au nombre de valeurs réelles pour tout attribut.  
  
 Exemple :  
  
 Probabilité de [Enfants Totaux = Un] = [Nombre de cas où Enfants Totaux = Un] + 1/[Nombre de tous les cas] + 3  
  
 Probabilité de [Enfants Totaux = Deux] = [Nombre de cas où Enfants Totaux = Deux] +1/[Nombre de tous les cas] +3  
  
> [!NOTE]  
>  L’ajustement de 3 est calculé en ajoutant 1 au nombre total de valeurs existantes, n.  
  
 Après ajustement, les probabilités pour toutes les valeurs sont encore égales à 1. La probabilité pour la valeur sans données (dans cet exemple, [Enfants Totaux = 'Zéro', 'Trois', ou une autre valeur]) commence à un niveau non nul très bas, et augmente lentement au fur et à mesure de l'ajout de cas.  
  
 variance  
 Indique la variance des valeurs dans le nœud. Par définition, la variance est toujours de 0 pour les valeurs discrètes. Si le modèle prend en charge des valeurs continues, la variance est calculée comme σ (sigma), à l’aide du dénominateur n, ou le nombre de cas dans le nœud.  
  
 Il y a deux définitions dans l’utilisation générale pour représenter l’écart type (**StDev**). Une méthode pour calculer l'écart type prend en considération le décalage, et une autre méthode calcule l'écart type sans utiliser le décalage. En général, les algorithmes d'exploration de données Microsoft n'utilisent pas le décalage lors du calcul de l'écart type.  
  
 La valeur qui apparaît dans la table NODE_DISTRIBUTION est la valeur réelle pour tous les attributs discrets et discrétisés, et la moyenne pour les valeurs continues.  
  
 VALUE_TYPE  
 Indique le type des données de la valeur ou un attribut, et l'utilisation de la valeur. Certains types de valeurs s'appliquent uniquement à certains types de modèle :  
  
|ID VALUE_TYPE|Étiquette de valeur|Nom de type de valeur|  
|--------------------|-----------------|---------------------|  
|1|Missing|Indique que les données de cas ne contenaient pas de valeur pour cet attribut. L’état **Missing** est calculé séparément des attributs qui ont des valeurs.|  
|2|Existing|Indique que les données de cas contiennent une valeur pour cet attribut.|  
|3|Continu|Indique que la valeur de l'attribut est une valeur numérique continue et par conséquent peut être représentée par une moyenne ainsi que la variance et l'écart type.|  
|4|Discret|Indique une valeur, soit numérique, soit texte, traitée comme discrète.<br /><br /> **Remarque** Les valeurs discrètes peuvent être aussi manquantes ; toutefois, elles sont traitées différemment durant les calculs. Pour plus d’informations, consultez [Valeurs manquantes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md).|  
|5|Discrétisé|Indique que l'attribut contient des valeurs numériques qui ont été discrétisées. La valeur sera une chaîne mise en forme qui décrit les compartiments de discrétisation.|  
|6|Existant|Indique que l'attribut a des valeurs numériques continues et que les valeurs ont été fournies dans les données, au contraire des valeurs manquantes ou déduites.|  
|7|Coefficient|Indique une valeur numérique qui représente un coefficient.<br /><br /> Un coefficient est une valeur appliquée lors du calcul de la valeur de la variable dépendante. Par exemple, si votre modèle crée une formule de régression qui prédit le revenu selon l’âge, le coefficient est utilisé dans la formule qui relie l'âge au revenu.|  
|8|Gain du score|Indique une valeur numérique qui représente le gain de score pour un attribut.|  
|9|Statistiques|Indique une valeur numérique qui représente une statistique pour un régresseur.|  
|10|Nom unique de nœud|Indique que la valeur ne doit pas être traitée comme numérique ou chaîne, mais comme l'identificateur unique d'un autre nœud de contenu dans un modèle.<br /><br /> Par exemple, dans un modèle de réseau neuronal, les ID fournissent des pointeurs à partir des nœuds dans la couche de sortie vers les nœuds de la couche masquée, et des nœuds de la couche masquée vers les nœuds de la couche d'entrée.|  
|11|Intercepter|Indique une valeur numérique qui représente l'interception dans une formule de régression.|  
|12|Périodicité|Indique que la valeur dénote une structure périodique dans un modèle.<br /><br /> S'applique uniquement aux modèles de série chronologique qui contiennent un modèle ARIMA.<br /><br /> Remarque : l’algorithme MTS (Microsoft Time Series) détecte automatiquement des structures périodiques en fonction des données d’apprentissage. Par conséquent, les périodicités dans le modèle final peuvent inclure des valeurs de périodicité que vous n'avez pas fournies comme paramètre lors de la création du modèle.|  
|13|Ordre autorégressif|Indique que la valeur représente le nombre de séries autorégressives.<br /><br /> S'applique aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|14|Ordre des moyennes mobiles|Valeur qui représente le nombre de moyennes mobiles dans une série.<br /><br /> S'applique aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|15|Ordre des différences|Indique que la valeur représente une valeur qui indique le nombre de fois où la série fait l'objet d'une différenciation.<br /><br /> S'applique aux modèles de série chronologique qui utilisent l'algorithme ARIMA.|  
|16|Booléen|Représente un type booléen.|  
|17|Autres|Représente une valeur personnalisée définie par l'algorithme.|  
|18|Chaîne rendue au préalable|Représente une valeur personnalisée que l'algorithme rend sous la forme d'une chaîne. Aucune mise en forme n'a été appliquée par le modèle objet.|  
  
 Les types valeur sont dérivés de l'énumération ADMOMD.NET. Pour plus d’informations, consultez <xref:Microsoft.AnalysisServices.AdomdServer.MiningValueType>.  
  
### <a name="node-score"></a>Score du nœud  
 La signification du score de nœud diffère selon le type de modèle et peut également être spécifique au type de nœud. Pour plus d’informations sur la méthode de calcul de NODE_SCORE pour chaque type de modèle et de nœud, consultez [Contenu du modèle d’exploration de données par type d’algorithme](#bkmk_AlgoType).  
  
### <a name="node-probability-and-marginal-probability"></a>Probabilité du nœud et probabilité marginale  
 L'ensemble de lignes de schéma du modèle d'exploration de données inclut les colonnes NODE_PROBABILITY et MARGINAL_PROBABILITY pour tous les types de modèle. Ces colonnes contiennent uniquement des valeurs dans les nœuds où une valeur de probabilité est explicite. Par exemple, le nœud racine d'un modèle ne contient jamais un score de probabilité.  
  
 Dans les nœuds qui fournissent des scores de probabilité, la probabilité du nœud et les probabilités marginales représentent des calculs différents.  
  
-   La**probabilité marginale** est la probabilité d'atteindre le nœud à partir de son parent.  
  
-   La**probabilité du nœud** est la probabilité d'atteindre le nœud à partir de la racine.  
  
-   La**probabilité du nœud** est toujours inférieure ou égale à la **probabilité marginale**.  
  
 Par exemple, si l'alimentation de tous les clients dans un arbre de décision est répartie de manière égale par sexe (et aucune valeur ne manque), la probabilité des nœuds enfants doit être .5. Toutefois, supposons que chacun des nœuds pour le sexe est également divisé par niveaux de revenu, Élevé, Moyen, et Bas. Dans ce cas, le score MARGINAL_PROBABILITY pour chaque nœud enfant doit toujours être .33 mais la valeur NODE_PROBABILTY sera le produit de toutes les probabilités qui mènent à ce nœud et donc toujours inférieure à la valeur MARGINAL_PROBABILITY.  
  
|Niveau de nœud/attribut et valeur|Probabilité marginale|probabilité du nœud|  
|----------------------------------------|--------------------------|----------------------|  
|Racine du modèle<br /><br /> Tous les clients cibles|1|1|  
|Clients cibles répartis par sexe|.5|.5|  
|Clients cibles répartis par sexe, et répartis de nouveau en trois directions selon le revenu|.33|.5 * .33 = .165|  
  
### <a name="node-rule-and-marginal-rule"></a>Règle du nœud et règle marginale  
 L'ensemble de lignes de schéma du modèle d'exploration de données inclut aussi les colonnes NODE_PROBABILITY et MARGINAL_PROBABILITY pour tous les types de modèle. Ces colonnes contiennent des fragments XML qui peuvent être utilisés pour sérialiser un modèle, ou représenter certaines parties de la structure de modèle. Ces colonnes peuvent être vides pour certains nœuds, si une valeur n'a aucune signification.  
  
 Deux types de règles XML sont fournis, semblables aux deux types de valeurs de probabilité. Le fragment XML dans MARGINAL_RULE définit l'attribut et la valeur du nœud actuel, alors que le fragment XML dans NODE_RULE décrit le chemin d'accès au nœud actuel de la racine modèle.  
  
##  <a name="bkmk_AlgoType"></a> Contenu du modèle d'exploration de données par type d'algorithme  
 Chaque algorithme stocke des types différents d'informations dans le cadre de son schéma de contenu. Par exemple, l'algorithme de gestion des clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] génère de nombreux nœuds enfants qui représentent chacun un cluster possible. Chaque nœud de cluster contient des règles qui décrivent des caractéristiques que partagent les éléments dans le cluster. Par opposition, l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) ne contient pas de nœuds enfants ; à la place, le nœud parent pour le modèle contient l'équation qui décrit la relation linéaire découverte par analyse.  
  
 La table suivante propose des liens vers des rubriques pour chaque type d'algorithme.  
  
-   **Rubriques de contenu de modèle :** Expliquent la signification de chaque type de nœud pour chaque type d'algorithme et fournissent des recommandations sur les nœuds les plus pertinents dans un type de modèle particulier.  
  
-   **Rubriques de requêtes :** Fournissent des exemples de requêtes par rapport à un type de modèle particulier et des recommandation sur la manière d'interpréter les résultats.  
  
|Type d'algorithme ou de modèle|model content|Interrogation des modèles d'exploration de données|  
|-----------------------------|-------------------|----------------------------|  
|Modèles de règles d'association|[Contenu du modèle d’exploration de données pour les modèles d’association &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-association-models-analysis-services-data-mining.md)|[Exemples de requêtes de modèle association](../../analysis-services/data-mining/association-model-query-examples.md)|  
|Modèles de clustering|[Contenu du modèle d’exploration de données pour les modèles d’arbre de décision & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md)|[Exemples de requêtes modèle de clustering](../../analysis-services/data-mining/clustering-model-query-examples.md)|  
|Modèle d'arbres de décision|[Contenu du modèle d’exploration de données pour les modèles d’arbre de décision & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md)|[Exemples de requêtes de modèle d’arbre de décision](../../analysis-services/data-mining/decision-trees-model-query-examples.md)|  
|Modèles de régression linéaire|[Contenu du modèle d’exploration de données pour les modèles de régression linéaire &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md)|[Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md)|  
|Modèles de régression logistique|[Contenu du modèle d’exploration de données pour les modèles de régression logistique &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md)|[Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md)|  
|Modèles Naïve Bayes|[Contenu du modèle d’exploration de données pour les modèles Naive Bayes & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md)|[Exemples de requêtes de modèle Naive Bayes](../../analysis-services/data-mining/naive-bayes-model-query-examples.md)|  
|Modèles de réseau neuronal|[Contenu du modèle d’exploration de données pour les modèles de réseau neuronal &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md)|[Exemples de requêtes de modèle de réseau neuronal](../../analysis-services/data-mining/neural-network-model-query-examples.md)|  
|Sequence clustering|[Contenu du modèle d’exploration de données pour les modèles Sequence Clustering &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-sequence-clustering-models.md)|[Exemples de requêtes de modèle MSC (Sequence Clustering)](../../analysis-services/data-mining/sequence-clustering-model-query-examples.md)|  
|Modèles de séries chronologiques|[Contenu du modèle d’exploration de données pour les modèles de série chronologique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md)|[Exemples de requêtes de modèle de série chronologique](../../analysis-services/data-mining/time-series-model-query-examples.md)|  
  
##  <a name="bkmk_Viewing"></a> Outils pour afficher le contenu du modèle d'exploration de données  
 Lorsque vous parcourez ou explorez un modèle dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez consulter les informations dans la **Visionneuse de l'arborescence de contenu générique**qui est disponible dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 La Visionneuse de l'arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)] affiche les colonnes, les règles, les propriétés, les attributs, les nœuds et autre contenu du modèle en utilisant les mêmes informations disponibles dans l'ensemble de lignes du schéma du contenu du modèle d'exploration de données. L'ensemble de lignes de schéma du contenu est une infrastructure générique permettant de présenter des informations détaillées sur le contenu d'un modèle d'exploration de données. Vous pouvez consulter le contenu du modèle dans tout client qui prend en charge des ensembles de lignes hiérarchiques. La visionneuse dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] présente ces informations dans une visionneuse de table HTML qui présente tous les modèles dans un format cohérent, en facilitant la compréhension de la structure des modèles que vous créez. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md).  
  
##  <a name="bkmk_Querying"></a> Outils pour interroger le contenu du modèle d'exploration de données  
 Pour extraire un contenu de modèle d'exploration de données, vous devez créer une requête en fonction du modèle d'exploration de données.  
  
 La méthode la plus simple pour créer une requête de contenu est d'exécuter l'instruction DMX suivante dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
```  
SELECT * FROM [<mining model name>].CONTENT  
```  
  
 Pour plus d’informations, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
 Vous pouvez également interroger le contenu du modèle d'exploration de données à l'aide des ensembles de lignes du schéma d'exploration de données. Un ensemble de lignes de schéma est une structure standard que les clients utilisent pour découvrir, parcourir et interroger des informations relatives aux structures et modèles d'exploration de données. Vous pouvez interroger les ensembles de lignes de schéma en utilisant les instructions XMLA, Transact-SQL ou DMX.  
  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous pouvez aussi accéder aux informations dans les ensembles de lignes de schéma d'exploration de données en ouvrant une connexion à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et en interrogeant les tables système. Pour plus d’informations, consultez [Ensembles de lignes de schéma d’exploration de données &#40;SSAS&#41;](../../analysis-services/data-mining/data-mining-schema-rowsets-ssas.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Visionneuse de l’arborescence de contenu générique Microsoft & #40 ; exploration de données & #41 ;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c)   
 [Algorithmes d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)  
  
  
