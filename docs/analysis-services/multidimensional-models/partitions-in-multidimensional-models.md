---
title: Partitions dans les modèles multidimensionnels | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8efdd2dcf936270a93ffd9cac694fd2aaccbe6e1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="partitions-in-multidimensional-models"></a>Partitions dans les modèles multidimensionnels
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], une *partition* permet le stockage physique des données de faits chargées dans un groupe de mesures. Une partition unique est créée automatiquement pour chaque groupe de mesures, mais il est courant de créer des partitions supplémentaires qui segmentent encore davantage les données, ce qui permet un traitement plus efficace et des performances de requête plus rapides.  
  
 Le traitement est plus efficace car les partitions peuvent être traitées indépendamment et en parallèle, sur un ou plusieurs serveurs. Les requêtes s'exécutent plus rapidement car chaque partition peut être configurée de manière à ce que les modes de stockage et les optimisations d'agrégation génèrent des temps de réponse plus courts. Par exemple, choisir le stockage MOLAP pour les partitions qui contiennent des données plus récentes s'avère généralement plus rapide que le stockage ROLAP. De même, si vous partitionnez par date, les partitions qui contiennent les données plus récentes peuvent bénéficier de davantage d'optimisations que les partitions qui contiennent des données plus anciennes auxquelles l'accès est moins fréquent. Notez que varier le stockage et la conception d'agrégation par partition aura un impact négatif sur les futures opérations de fusion. Veillez à déterminer si la fusion est un élément essentiel de votre stratégie de la gestion des partitions avant d'optimiser des partitions individuelles.  
  
> [!NOTE]  
>  La prise en charge de plusieurs partitions est disponible dans les versions Business Intelligence Edition et Enterprise Edition. L'édition standard ne prend pas en charge plusieurs partitions. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).  
  
## <a name="important-considerations-when-designing-a-partitioning-strategy"></a>Considérations importantes à prendre en compte lors de la conception d'une stratégie de partitionnement  
 L'intégrité des données d'un cube repose sur les données distribuées parmi les partitions de cube du sorte qu'aucune donnée ne soit dupliquée entre les partitions. Lorsque les données sont récapitulées à partir des partitions, tous les éléments de données qui sont présents dans plusieurs partitions sont résumés comme s'il s'agissait d'éléments de données différents. Cela peut entraîner des résumés incorrects et la distribution de données erronées à l'utilisateur final. Par exemple, si une transaction commerciale du produit X est dupliquée dans les tables de faits pour deux partitions, les résumés des ventes du produit X peuvent inclure une double comptabilité de la transaction dupliquée.  
  
 Les partitions peuvent être fusionnées ; vous pouvez utiliser cette fonctionnalité dans votre stratégie globale de mise à jour des données et de stockage. Les partitions peuvent être fusionnées uniquement si elles ont les mêmes mode de stockage et conception d'agrégation. Pour créer des partitions candidates à une prochaine fusion, vous pouvez copier la conception d'agrégation d'une autre partition lorsque vous créez des partitions. Vous pouvez également modifier une partition après qu'elle a été créée pour copier la conception d'agrégation d'une autre partition. La fusion de partitions doit également être effectuée avec soin pour éviter la duplication de données dans la partition résultante, ce qui peut rendre les données du cube inexactes.  
  
## <a name="local-partitions"></a>Partitions locales  
 Les partitions locales sont des partitions définies, traitées et stockées sur un même serveur. Si vous possédez des groupes de mesures volumineux dans un cube, vous pouvez les partitionner à l'extérieur du cube afin que le traitement s'effectue en parallèle sur les partitions. L'avantage est que le traitement parallèle permet une exécution plus rapide. Comme un travail de traitement de partition ne doit pas nécessairement se terminer pour qu'un autre puisse commencer, ils peuvent s'exécuter en parallèle. Pour plus d’informations, consultez [Create and Manage a Local Partition &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md).  
  
## <a name="remote-partitions"></a>Partitions distantes  
 Les partitions distantes sont des partitions définies sur un serveur, mais traitées et stockées sur un autre serveur. Pour distribuer le stockage de vos données et métadonnées entre plusieurs serveurs, utilisez les partitions distantes. Habituellement, lorsque vous passez de la phase de développement à celle de production, le volume des données sous analyse croît de plusieurs fois sa taille. Face à des blocs de données aussi importants, une alternative consistant à distribuer ces données entre plusieurs ordinateurs est possible. Cela ne tient pas seulement au fait qu'un seul ordinateur n'est pas en mesure de stocker toutes les données, mais aussi au fait que vous voulez que plusieurs ordinateurs traitent les données en parallèle. Pour plus d’informations, consultez [Create and Manage a Remote Partition &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md).  
  
## <a name="aggregations"></a>Aggregations  
 Les agrégations sont des résumés précalculés de données de cubes qui permettent à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de fournir des réponses rapides à des requêtes. Vous pouvez contrôler le nombre d'agrégations créées pour un groupe de mesures en définissant des limites de stockage ou des gains de performance, ou en arrêtant arbitrairement le processus de génération d'agrégations après un certain temps d'exécution. Un plus grand nombre d'agrégations ne signifie pas nécessairement de meilleurs résultats. Chaque nouvelle agrégation présente un coût, en termes d'espace disque et de temps de traitement. Nous vous recommandons de créer des agrégations pour un gain de performances de trente pour cent, puis d’augmenter ce nombre uniquement si les tests ou l’expérience le justifient. Pour plus d’informations, consultez [Conception d’agrégations &#40;Analysis Services - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/designing-aggregations-analysis-services-multidimensional.md).  
  
## <a name="partition-merging-and-editing"></a>Fusion et modification de partitions  
 Si deux partitions utilisent la même conception d'agrégation, vous pouvez fusionner ces deux partitions en une seule. Par exemple, si vous disposez d'une dimension d'inventaire partitionnée par mois, à la fin de chaque mois du calendrier, vous pouvez fusionner la partition du mois avec celle de l'année jusqu'à cette date. De cette manière, la partition du mois en cours peut être traitée et analysée rapidement, tandis que le reste de l'année en mois doit simplement être traité de nouveau lors de la fusion. Ce traitement répété nécessite plus de temps et peut être exécuté moins souvent. Pour plus d’informations sur la gestion du processus de fusion de partitions, consultez [Merge Partitions in Analysis Services &#40;SSAS - Multidimensional&#41;](../../analysis-services/multidimensional-models/merge-partitions-in-analysis-services-ssas-multidimensional.md). Pour modifier les partitions de cube à l’aide de l’onglet **Partitions** dans le Concepteur de Cube, consultez [Edit or Delete Partitions &#40;Analyisis Services - Multidimensional&#41;](../../analysis-services/multidimensional-models/edit-or-delete-partitions-analyisis-services-multidimensional.md).  
  
## <a name="related-topics"></a>Rubriques connexes  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Créer et gérer une Partition locale & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md)|Contient des informations sur la façon de partitionner des données à l'aide de filtres ou de tables de faits différentes sans duplication des données.|  
|[Définir le stockage des partitions & #40 ; Analysis Services - multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/set-partition-storage-analysis-services-multidimensional.md)|Explique comment configurer le stockage de partitions.|  
|[Modifier ou supprimer des Partitions & #40 ; Analysis Services - multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/edit-or-delete-partitions-analyisis-services-multidimensional.md)|Décrit comment afficher et modifier des partitions.|  
|[Fusionner des Partitions dans Analysis Services & #40 ; SSAS - multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/merge-partitions-in-analysis-services-ssas-multidimensional.md)|Contient des informations sur la façon de fusionner les partitions qui ont des tables de faits ou des segments de données différents sans duplication des données.|  
|[Définir l’écriture différée de Partition](../../analysis-services/multidimensional-models/set-partition-writeback.md)|Explique comment activer l'écriture sur une partition.|  
|[Créer et gérer une Partition distante & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md)|Décrit comment créer et gérer une partition distante.|  
  
  
