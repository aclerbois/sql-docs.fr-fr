---
title: Stockage (Analysis Services - données multidimensionnelles) du cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 05ff45fa98b578fce295ab2113abf301bc3b78ed
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="cube-storage-analysis-services---multidimensional-data"></a>Stockage de cube (Analysis Services - Données multidimensionnelles)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il se peut que le stockage n'inclue que les métadonnées de cube, ou toutes les données sources de la table de faits ainsi que les agrégations définies par des dimensions liées au groupe de mesures. La quantité de données stockée dépend du mode de stockage sélectionné et du nombre d'agrégations défini. Cette quantité de données stockées influence directement les performances des requêtes. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise plusieurs techniques destinées à minimiser l’espace requis pour le stockage des données de cube et les agrégations :  
  
-   Les options de stockage permettent de choisir les modes et emplacements de stockage qui conviennent le mieux aux données de cube.  
  
-   Un algorithme complexe permet de créer des agrégations de synthèse efficaces et de minimiser l'espace de stockage sans faire de compromis au niveau de la vitesse de réponse.  
  
-   Aucun stockage n'est alloué pour les cellules vides.  
  
 La définition du stockage est basée sur la partition, et au moins une partition existe pour chaque groupe de mesures d'un cube. Pour plus d’informations, consultez [Partitions &#40;Analysis Services - données multidimensionnelles&#41;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md), [traitement et Modes de stockage de Partition](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-partition-storage-modes-and-processing.md), [mesures et groupes de mesures](../../analysis-services/multidimensional-models/measures-and-measure-groups.md), et [créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md).  
  
## <a name="partition-storage"></a>Stockage des partitions  
 Le stockage d'un groupe de mesures peut être divisé en plusieurs partitions. Les partitions permettent de répartir un groupe de mesures en segments discrets sur un ou plusieurs serveurs, ainsi que d'optimiser le stockage et les performances des requêtes. Chaque partition d'un groupe de mesures peut être basée sur une source de données différente et être stockée en utilisant des paramètres de stockage différents.  
  
 Vous spécifiez la source de données d'une partition lorsque vous la créez. Vous pouvez aussi changer la source de données d'une partition existante. Un groupe de mesures peut être partitionné verticalement ou horizontalement. Chaque partition dans un groupe de mesures partitionné verticalement est basée sur une vue filtrée d'une seule table source. Par exemple, si un groupe de mesures est basé sur une seule table qui contient plusieurs années de données, vous pouvez créer une partition séparée pour les données de chaque année. En revanche, chaque partition dans un groupe de mesures partitionné horizontalement est basée sur une table séparée. Vous utiliserez donc les partitions horizontales si la source de données stocke les données de chaque année dans une table séparée.  
  
 Les partitions sont initialement créées avec les mêmes paramètres de stockage que le groupe de mesures dans lequel elles sont créées. Les paramètres de stockage déterminent si les données de détails et d'agrégations sont stockées dans un format multidimensionnel sur l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], dans un format relationnel sur le serveur source ou dans une combinaison des deux. Les paramètres de stockage déterminent également si la mise en cache proactive est employée pour répercuter automatiquement les modifications des données sources sur les données multidimensionnelles stockées sur le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 L'utilisateur ne voit pas les partitions des cubes. Cependant, le choix des paramètres de stockage des différentes partitions peut affecter la disponibilité immédiate des données, la quantité d'espace disque utilisé et les performances des requêtes. Des partitions peuvent être stockées sur plusieurs instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Vous disposez ainsi d'une approche en cluster du stockage du cube et d'une répartition de la charge entre plusieurs serveurs [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [traitement et Modes de stockage de Partition](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-partition-storage-modes-and-processing.md), [Partitions distantes](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-remote-partitions.md), et [Partitions &#40;Analysis Services - données multidimensionnelles&#41; ](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md).  
  
## <a name="linked-measure-groups"></a>Groupes de mesures liés  
 Cette approche peut nécessiter un espace disque considérable afin de stocker plusieurs copies d'un cube sur différentes instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], mais elle permet de réduire tangiblement l'espace nécessaire en remplaçant les copies du groupe de mesures par des groupes de mesures liés. Un groupe de mesures lié est basé sur un groupe de mesures d'un cube dans une autre base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], sur la même instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou sur une instance différente. Un groupe de mesures lié peut également être utilisé avec des dimensions liées provenant du même cube source. Les dimensions et les groupes de mesures liés utilisent les agrégations du cube source et n'ont aucun besoin de stockage de données propre. Par conséquent, en conservant les groupes de mesures et les dimensions sources dans une base de données et en créant des cubes et des dimensions liés dans des cubes d'autres bases de données, vous pouvez économiser de l'espace disque destiné autrement au stockage. Pour plus d’informations, consultez [des groupes de mesures liés](../../analysis-services/multidimensional-models/linked-measure-groups.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Agrégations et conceptions d’agrégation](../../analysis-services/multidimensional-models-olap-logical-cube-objects/aggregations-and-aggregation-designs.md)  
  
  
