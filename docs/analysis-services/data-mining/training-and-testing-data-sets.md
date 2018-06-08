---
title: Apprentissage et jeux de données de test | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fe6d39614bbeaca70f8e0e6d205be5cbcbc05bbc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="training-and-testing-data-sets"></a>Jeux de données d'apprentissage et de test
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La séparation des données en jeux d'apprentissage et jeux de test correspond à une partie importante de l'évaluation des modèles d'exploration de données. En général, lorsque vous séparez un jeu de données en un jeu d'apprentissage et un jeu de test, la plupart des données sont utilisées pour l'apprentissage et une plus petite partie des données est utilisée pour les tests. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] échantillonne de manière aléatoire les données afin de s'assurer que les jeux de test et d'apprentissage sont semblables. L'utilisation de données similaires pour l'apprentissage et les tests vous permet de minimiser les effets des différences de données et de mieux comprendre les caractéristiques du modèle.  
  
 Après le traitement d'un modèle à l'aide du jeu d'apprentissage, vous testez le modèle en effectuant des prédictions sur le jeu de test. Comme les données dans le jeu de test contiennent déjà des valeurs connues pour l'attribut que vous souhaitez prédire, il est facile de déterminer si les prédictions du modèle sont correctes.  
  
## <a name="creating-test-and-training-sets-for-data-mining-structures"></a>Création de jeux d'apprentissage et de test pour les structures d'exploration de données  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous séparez le jeu de données d'origine au niveau de la structure d'exploration de données. Les informations sur la taille des jeux de données d'apprentissage et de test, ainsi que les lignes qui appartiennent à un jeu, sont stockées avec la structure, et tous les modèles basés sur cette structure peuvent utiliser les jeux pour effectuer l'apprentissage et le test.  
  
 Vous pouvez définir un jeu de données de test sur une structure d'exploration de données de plusieurs façons :  
  
-   en utilisant l'Assistant Exploration de données pour diviser une structure d'exploration de données lorsque vous la créez ;  
  
-   en modifiant les propriétés de la structure sous l'onglet **Structure d'exploration de données** du Concepteur de modèle d'exploration de données ;  
  
-   en créant et modifiant par programmation les structures à l'aide des objets AMO (Analysis Management Objects) ou du langage de définition de données (DDL) XML.  
  
### <a name="using-the-data-mining-wizard-to-divide-a-mining-structure"></a>Utilisation de l'Assistant Exploration de données pour diviser une structure d'exploration de données  
 Par défaut, après avoir défini les sources de données pour une structure d'exploration de données, l'Assistant Exploration de données divise les données en deux jeux : l'un avec 70 % des données source pour l'apprentissage du modèle et l'autre avec 30 % des données source pour tester le modèle. Ces valeurs par défaut ont été choisies parce qu’un rapport 70-30 est souvent utilisé dans l’exploration de données, mais avec [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vous pouvez modifier ce rapport en fonction de vos besoins.  
  
 Vous pouvez également configurer l'Assistant pour définir un nombre maximal de cas d'apprentissage ou vous pouvez associer les limites pour permettre un pourcentage maximal de cas jusqu'à un nombre maximal spécifié de cas. Lorsque vous spécifiez à la fois un pourcentage maximal de cas et un nombre maximal de cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise la plus petite des deux limites comme taille du jeu de test. Par exemple, si vous spécifiez une exclusion de 30 % pour les scénarios de test et un nombre maximal de scénarios de test égal à 1000, la taille du jeu de test ne dépassera jamais 1 000 scénarios. Cela peut être utile si vous souhaitez garantir que la taille de votre jeu de test reste cohérente même si des données d'apprentissage supplémentaires sont ajoutées au modèle.  
  
 Si vous utilisez la même vue de source de données pour des structures d'exploration de données différentes et que vous souhaitez vous assurer que les données sont divisées approximativement de la même manière pour toutes les structures d'exploration de données et leurs modèles, vous devez spécifier la valeur de départ qui est utilisée pour initialiser l'échantillonnage aléatoire. Quand vous spécifiez une valeur pour **HoldoutSeed**, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise cette valeur pour commencer l’échantillonnage. Dans le cas contraire, l'échantillonnage utilise un algorithme de hachage sur le nom de la structure d'exploration de données pour créer la valeur de départ.  
  
> [!NOTE]  
>  Si vous créez une copie de la structure d'exploration de données à l'aide des instructions **EXPORT** et **IMPORT** , la nouvelle structure d'exploration de données aura les mêmes jeux de données de test et d'apprentissage, car le processus d'exportation crée un nouvel ID mais utilise le même nom. Toutefois, si deux structures d'exploration de données utilisent la même source de données sous-jacente tout en ayant des noms différents, les jeux créés pour chaque structure d'exploration de données seront différents.  
  
### <a name="modifying-structure-properties-to-create-a-test-data-set"></a>Modification des propriétés de structure pour créer un jeu de données de test  
 Si vous créez et traitez une structure d'exploration de données, puis décidez ultérieurement de mettre de côté un jeu de données de test, vous pouvez modifier les propriétés de la structure d'exploration de données. Pour changer la façon dont les données sont partitionnées, modifiez les propriétés suivantes :  
  
|Propriété| Description|  
|--------------|-----------------|  
|**HoldoutMaxCases**|Spécifie le nombre maximal de scénarios à inclure dans le jeu de test.|  
|**HoldoutMaxPercent**|Spécifie le nombre de cas à inclure dans le jeu de test en tant que pourcentage du jeu de données complet. Pour n'avoir aucun jeu de données, il convient de spécifier 0.|  
|**HoldoutSeed**|Spécifie une valeur entière à utiliser comme valeur de départ lors de la sélection aléatoire de données pour les partitions. Cette valeur n'affecte pas le nombre de cas dans le jeu d'apprentissage ; à la place, elle garantit que la partition peut être répétée.|  
  
 Si vous ajoutez ou modifiez un jeu de données de test dans une structure existante, vous devez retraiter la structure et tous les modèles associés. En outre, comme la division des données source provoque l'apprentissage du modèle sur un autre sous-ensemble des données, vous pouvez constater des résultats différents de votre modèle.  
  
### <a name="specifying-holdout-programmatically"></a>Spécification de l'exclusion par programmation  
 Vous pouvez définir des jeux de données d'apprentissage et de test sur une structure d'exploration de données à l'aide des instructions DMX, AMO ou DDL XML. L'instruction ALTER MINING STRUCTURE ne prend pas en charge l'utilisation de paramètres d'exclusion.  
  
-   **DMX** Dans le langage DMX (Data Mining Extensions), l’instruction CREATE MINING STRUCTURE a été étendue pour inclure une clause WITH HOLDOUT.  
  
-   **ASSL** Vous pouvez soit créer une nouvelle structure d’exploration de données, soit ajouter un jeu de données de test à une structure d’exploration de données existante à l’aide du langage de script [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (ASSL).  
  
-   **AMO** Vous pouvez également afficher et modifier les jeux de données d'exclusion à l'aide d'AMO.  
  
 Vous pouvez afficher les informations relatives au jeu de données d'exclusion dans une structure d'exploration de données existante en interrogeant l'ensemble de lignes de schéma d'exploration de données. Vous pouvez pour cela effectuer un appel DISCOVER ROWSET ou utiliser une requête DMX.  
  
## <a name="retrieving-information-about-holdout-data"></a>Récupération d'informations sur les données d'exclusion  
 Par défaut, toutes les informations relatives aux jeux de données d'apprentissage et de test sont mises en cache afin que vous puissiez utiliser les données existantes pour effectuer l'apprentissage, puis tester les nouveaux modèles. Vous pouvez également définir des filtres à appliquer aux données d'exclusion mises en cache afin de pouvoir évaluer le modèle sur des sous-ensembles des données.  
  
 La façon dont les cas sont divisés en jeux de données d'apprentissage et de test dépend de la façon dont vous configurez l'exclusion et les données que vous fournissez. Si vous souhaitez déterminer le nombre de cas utilisés pour l'apprentissage ou le test ou trouver des détails supplémentaires sur les cas inclus dans les jeux d'apprentissage et de test, vous pouvez interroger la structure du modèle en créant une requête DMX. Par exemple, la requête ci-dessous retourne les cas qui ont été utilisés dans le jeu d'apprentissage du modèle.  
  
```  
SELECT * from <structure>.CASES WHERE IsTrainingCase()  
```  
  
 Pour extraire uniquement les scénarios de test et filtrer également les scénarios de test sur l'une des colonnes dans la structure d'exploration de données, utilisez la syntaxe suivante :  
  
```  
SELECT * from <structure>.CASES WHERE IsTestCase() AND <structure column name> = '<value>'  
```  
  
## <a name="limitations-on-the-use-of-holdout-data"></a>Limitations sur l'utilisation de données d'exclusion  
  
-   Pour utiliser l’exclusion, la propriété <xref:Microsoft.AnalysisServices.MiningStructureCacheMode> de la structure d’exploration de données doit avoir la valeur par défaut **KeepTrainingCases**. Si vous modifiez la propriété **CacheMode** en lui affectant **ClearAfterProcessing**, puis retraitez la structure d'exploration de données, la partition sera perdue.  
  
-   Vous ne pouvez pas supprimer les données d'un modèle de série chronologique ; par conséquent, vous ne pouvez pas séparer les données sources en jeux d'apprentissage et de test. Si vous commencez par créer une structure d'exploration de données et un modèle, puis choisissez l'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series), l'option pour créer un jeu de données d'exclusion est désactivée. L'utilisation des données d'exclusion est également désactivée si la structure d'exploration de données contient une colonne KEY TIME au niveau du cas ou de la table imbriquée.  
  
-   Il est possible de configurer accidentellement le jeu de données d'exclusion de sorte que le jeu de données complet soit utilisé pour le test et qu'il ne reste pas de données pour l'apprentissage. Cependant, dans cette éventualité, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] générera une erreur afin que vous puissiez résoudre le problème. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vous prévient également lorsque la structure est traitée, si plus de 50 % des données ont été exclues pour les tests.  
  
-   Dans la plupart des cas, la valeur d'exclusion par défaut 30 fournit un bon équilibre entre les données d'apprentissage et de test. Il n'existe pas de méthode simple pour déterminer le volume minimal du jeu de données permettant de fournir un apprentissage suffisant ou le volume maximal du jeu d'apprentissage permettant d'éviter un surajustement. Toutefois, après avoir construit un modèle, vous pouvez utiliser la validation croisée pour évaluer le jeu de données par rapport à un modèle particulier.  
  
-   Outre les propriétés répertoriées dans le tableau précédent, une propriété en lecture seule, **HoldoutActualSize**, est fournie dans les objets AMO et le langage DDL XML. Toutefois, comme la taille réelle d'une partition ne peut pas être déterminée précisément avant le traitement de la structure, vous devez vérifier si le modèle a été traité avant de récupérer la valeur de la propriété **HoldoutActualSize** .  
  
## <a name="related-content"></a>Contenu connexe  
  
|Rubriques|Liens|  
|------------|-----------|  
|Décrit comment les filtres sur un modèle interagissent avec les jeux de données d'apprentissage et de test.|[Filtres pour les modèles d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md)|  
|Décrit comment l'utilisation des données d'apprentissage et de test affecte la validation croisée.|[La Validation croisée & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)|  
|Fournit des informations sur les interfaces de programmation pour utiliser les jeux d'apprentissage et de test dans une structure d'exploration de données.|[Concepts et modèle objet AMO](../../analysis-services/multidimensional-models/analysis-management-objects/amo-concepts-and-object-model.md)<br /><br /> [Élément MiningStructure &#40;ASSL&#41;](../../analysis-services/scripting/objects/miningstructure-element-assl.md)|  
|Fournit la syntaxe DMX pour créer des ensembles d'exclusion.|[CRÉER UNE STRUCTURE D’EXPLORATION DE DONNÉES & #40 ; DMX & #41 ;](../../dmx/create-mining-structure-dmx.md)|  
|Récupérer des informations sur les cas dans les jeux d'apprentissage et de test.|[Ensembles de lignes de schéma d'exploration de données](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)<br /><br /> [Ensembles de lignes de schéma d’exploration de données &#40;SSAs&#41;](../../analysis-services/data-mining/data-mining-schema-rowsets-ssas.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Outils d’exploration de données](../../analysis-services/data-mining/data-mining-tools.md)   
 [Concepts d’exploration de données](../../analysis-services/data-mining/data-mining-concepts.md)   
 [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md)   
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
