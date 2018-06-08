---
title: Pour des objets de traitement Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d1cd8be08afc7ea0e8e7bf811db4e26208a2feae
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="processing-analysis-services-objects"></a>Traitement des objets Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le traitement affecte les types d’objets [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] suivants : bases de données, cubes, dimensions, groupes de mesures, partitions, et structures et modèles d’exploration de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Pour chaque objet, vous pouvez spécifier le niveau de traitement de l’objet ou spécifier l’option Traiter par défaut pour permettre à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de sélectionner automatiquement le niveau optimal de traitement. Pour plus d’informations sur les différents niveaux de traitement pour chaque objet, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
 Vous devez avoir connaissance des conséquences du comportement de traitement afin de réduire les répercussions négatives. Par exemple, le fait de traiter entièrement une dimension fait passer automatiquement toutes les partitions qui en dépendent à l'état non traité. Cela rend les cubes affectés indisponibles pour les requêtes tant que les partitions dépendantes n'ont pas été traitées.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Traitement d'une base de données](#bkmk_procdb)  
  
 [Traitement d'une dimension](#bkmk_procdim)  
  
 [Traitement d'un cube](#bkmk_proccube)  
  
 [Traitement d'un groupe de mesures](#bkmk_procmeasure)  
  
 [Traitement d'une partition](#bkmk_procpartition)  
  
 [Traitement des modèles et des structures d’exploration de données](#bkmk_procdm)  
  
##  <a name="bkmk_procdb"></a> Traitement d'une base de données  
 Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], une base de données contient des objets, mais pas de données. Lorsque vous traitez une base de données, vous demandez au serveur de traiter de manière récursive les objets qui stockent des données dans le modèle, telles que des dimensions, des partitions, des structures d'exploration de données et des modèles d'exploration de données.  
  
 Lorsque vous traitez une base de données, une partie ou l'ensemble des partitions, des dimensions et des modèles d'exploration de données que la base de données contient sont traités. Le type de traitement réel varie en fonction de l'état de chaque objet et de l'option de traitement que vous sélectionnez. Pour plus d’informations, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
##  <a name="bkmk_proccube"></a> Traitement d'un cube  
 Un cube peut être considéré comme un objet wrapper pour les groupes de mesures et les partitions. Un cube est constitué de dimensions et d'une ou plusieurs mesures, qui sont stockées dans des partitions. Les dimensions définissent la manière dont les données sont présentées dans le cube. Lorsque vous traitez un cube, une requête SQL est exécutée pour récupérer des valeurs dans la table de faits afin de remplir chaque membre du cube avec les valeurs de mesures appropriées. Il existe une valeur ou une valeur calculable pour tous les chemins d'accès spécifiques vers un nœud du cube.  
  
 Quand vous traitez un cube, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite toutes les dimensions non traitées dans le cube, ainsi qu’une partie ou l’ensemble des partitions comprises dans les groupes de mesures du cube. Les particularités dépendent de l'état des objets lorsque le traitement commence et de l'option de traitement que vous sélectionnez. Pour plus d’informations sur les options de traitement, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
 Le traitement d'un cube crée des fichiers destinés aux ordinateurs pour stocker les données factuelles significatives. Si des agrégations sont créées, elles sont stockées dans des fichiers de données d'agrégation. Vous pouvez alors parcourir le cube dans l’Explorateur d’objets dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou dans l’Explorateur de solutions dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]  
  
##  <a name="bkmk_procdim"></a> Traitement d'une dimension  
 Quand vous traitez une dimension, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] formule et exécute des requêtes sur des tables de dimensions pour retourner les informations nécessaires au traitement.  
  
|Pays|Région de vente|État|  
|-------------|------------------|-----------|  
|United States|West|California|  
|United States|West|Oregon|  
|United States|West|Washington|  
  
 Le traitement lui-même transforme les données tabulaires en hiérarchies utilisables. Ces hiérarchies sont des noms de membres entièrement articulés qui sont représentées en interne par des chemins d'accès numériques uniques. L'exemple suivant est une représentation textuelle d'une hiérarchie.  
  
||  
|-|  
|[United States]|  
|[United States].[West]|  
|[United States].[West].[California]|  
|[United States].[West].[Oregon]|  
|[United States].[West].[Washington]|  
  
 Le traitement de dimension n'entraîne pas la création ni la mise à jour des membres calculés, qui sont définis au niveau du cube. Les membres calculés sont affectés lorsque la définition du cube est mise à jour. En outre, le traitement des dimensions n'entraîne pas la création ni la mise à jour des agrégations. Il peut cependant provoquer la suppression d'agrégations. Les agrégations sont créées ou mises à jour uniquement durant le traitement de partition.  
  
 Lorsque vous traitez une dimension, soyez conscient que la dimension peut être utilisée dans plusieurs cubes. Lorsque vous traitez la dimension, ces cubes sont marqués comme non traités et deviennent indisponibles pour les requêtes. Pour traiter au même moment la dimension et les cubes liés, utilisez les paramètres de traitement par lots. Pour plus d’informations, consultez [Traitement par lots &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/batch-processing-analysis-services.md).  
  
##  <a name="bkmk_procmeasure"></a> Traitement d'un groupe de mesures  
 Quand vous traitez un groupe de mesures, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite une partie ou l’ensemble des partitions au sein du groupe de mesures, ainsi que toutes les dimensions non traitées qui participent au groupe de mesures. Les particularités du traitement dépendent de l'option de traitement que vous sélectionnez. Vous pouvez traiter un ou plusieurs groupes de mesures dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sans affecter d'autres groupes de mesures dans un cube.  
  
> [!NOTE]  
>  Vous pouvez traiter des groupes de mesures individuels par programmation ou en utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Vous ne pouvez pas traiter des groupes de mesures individuels dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]; en revanche, vous pouvez procéder au traitement par partition.  
  
##  <a name="bkmk_procpartition"></a> Traitement d'une partition  
 Pour une administration efficace d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , il est nécessaire de partitionner les données. Le traitement des partitions est unique car il prend en considération les contraintes d'utilisation et d'espace du disque dur, ainsi que les limitations relatives aux structures de données imposées par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour maintenir des temps de réponse courts pour les requêtes et un débit de traitement élevé, vous devez régulièrement créer, traiter et fusionner des partitions. Il est très important de reconnaître et de gérer le risque d'intégration de données redondantes durant la fusion de partitions. Pour plus d’informations, consultez [Fusionner des partitions dans Analysis Services &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/merge-partitions-in-analysis-services-ssas-multidimensional.md).  
  
 Quand vous traitez une partition, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite la partition et toutes les dimensions non traitées qui existent dans la partition, en fonction de l’option de traitement que vous sélectionnez. L'utilisation de partitions offre plusieurs avantages pour le traitement. Vous pouvez traiter une partition sans affecter les autres partitions d'un cube. Les partitions sont utiles pour stocker des données sujettes à l'écriture différée de cellule. L'écriture différée est une fonctionnalité qui permet à l'utilisateur d'effectuer une analyse de simulation en écrivant de nouvelles données dans la partition afin de voir les modifications prévues. Une partition d'écriture différée est requise si vous utilisez la fonctionnalité d'écriture différée de cellule de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le traitement des partitions en parallèle est utile car [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise plus efficacement la puissance de traitement et peut réduire sensiblement le temps total de traitement. Vous pouvez également traiter les partitions de manière séquentielle.  
  
##  <a name="bkmk_procdm"></a> Traitement des modèles et des structures d’exploration de données  
 Une structure d'exploration de données définit le domaine de données à partir duquel les modèles d'exploration de données vont être générés. Une structure d'exploration de données peut contenir plusieurs modèles d'exploration de données. Vous pouvez traiter une structure d'exploration de données indépendamment des modèles d'exploration de données qui lui sont associés. Lorsque vous traitez séparément une structure d'exploration de données, elle est remplie avec les données d'apprentissage de votre source de données.  
  
 Lorsqu'un modèle d'exploration de données est traité, les données d'apprentissage sont passées dans les algorithmes du modèle d'exploration de données, forment le modèle à l'aide de l'algorithme d'exploration de données et créent le contenu. Pour plus d’informations sur l’architecture des modèles d’exploration de données, consultez [Structures d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md).  
  
 Pour plus d’informations sur le traitement des structures et des modèles d’exploration de données, consultez [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Outils et approches de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/tools-and-approaches-for-processing-analysis-services.md)   
 [Le traitement par lots & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/batch-processing-analysis-services.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)  
  
  
