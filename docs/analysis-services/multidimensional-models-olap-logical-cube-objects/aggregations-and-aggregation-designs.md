---
title: Agrégations et conceptions d’agrégation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: af9eab2fb7d3df0fcb8a7416b43dd387583f4762
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="aggregations-and-aggregation-designs"></a>Agrégations et conceptions d'agrégation
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un objet <xref:Microsoft.AnalysisServices.AggregationDesign> définit un ensemble de définitions d'agrégation qu'il est possible de partager sur plusieurs partitions.  
  
 Un objet <xref:Microsoft.AnalysisServices.Aggregation> représente le résumé des données d'un groupe de mesures à une certaine granularité des dimensions.  
  
 Un objet <xref:Microsoft.AnalysisServices.Aggregation> simple est composé des informations de base et des dimensions. Les informations de base incluent le nom de l'agrégation, l'ID, les annotations et une description. Les dimensions sont une collection d'objets <xref:Microsoft.AnalysisServices.AggregationDimension> qui contiennent la liste des attributs de granularité de la dimension.  
  
 Les agrégations sont des données de synthèse précalculées à partir de cellules feuilles. Elles améliorent les temps de réponse aux requêtes en préparant les réponses avant que les questions ne soient posées. Par exemple, lorsqu'une table de faits du Data Warehouse contient des centaines de milliers de lignes, une requête demandant le total des ventes hebdomadaires pour une ligne de produit particulière peut prendre beaucoup de temps si toutes les lignes de la table de faits doivent être analysées et additionnées au moment de la requête pour calculer la réponse. En revanche, la réponse peut être presque immédiate si les données de synthèse fournies en réponse à cette requête ont été précalculées. Dans la technologie OLAP, ce calcul préalable des données de synthèse a lieu durant le traitement et constitue le facteur essentiel pour offrir des réponses rapides.  
  
 La technologie OLAP a recours à des cubes pour organiser les données de synthèse en structures multidimensionnelles. Les dimensions et leurs hiérarchies d'attributs reflètent les requêtes qu'il est possible de lancer sur un cube. Dans la structure multidimensionnelle du cube, les agrégations sont stockées dans des cellules définies par des coordonnées spécifiées par les dimensions. Par exemple, la question « Quelles furent les ventes du produit X en 1998 pour la région Nord-Ouest? » implique trois dimensions (produit, temps et géographie) et une mesure (ventes). La valeur figurant dans la cellule du cube qui se trouve aux coordonnées spécifiées (produit X, 1998, Nord-Ouest) correspond à la réponse. Il s'agit d'une valeur numérique unique.  
  
 D'autres types de questions peuvent retourner plusieurs valeurs. Par exemple « Quelles furent les ventes d'équipements par trimestre et par région pour l'année 1998 ? » Les requêtes de ce type renvoient des ensembles de cellules dont les coordonnées répondent aux critères spécifiés. Le nombre de cellules renvoyées par cette requête dépend du nombre d'éléments au niveau Équipement de la dimension Produit, des quatre trimestres de l'année 1998 et du nombre de régions dans la dimension Géographie. Si toutes les données de synthèse ont été précalculées dans des agrégations, le temps de réponse à la requête dépend uniquement du temps nécessaire à l'extraction des cellules spécifiées. Aucun calcul ni lecture des données de la table de faits ne sont nécessaires.  
  
 Bien que le calcul préalable de toutes les agrégations possibles dans un cube permette d'obtenir les temps de réponse les plus rapides pour toutes les requêtes, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut facilement calculer certaines valeurs agrégées à partir d'autres agrégations précalculées. De plus, le calcul de toutes les agrégations possibles nécessite un temps de traitement et une capacité de stockage non négligeables. Par conséquent, il est nécessaire de trouver un compromis entre les besoins de stockage et le pourcentage d'agrégations possibles qui sont précalculées. Si aucune agrégation n'est précalculée (0 %), le temps de traitement et l'espace de stockage requis pour un cube sont réduits au minimum, mais le temps de réponse aux requêtes peut être long car pour répondre à chaque requête, les données nécessaires doivent être extraites des cellules feuilles puis agrégées au moment de la requête. Par exemple, afficher un seul résultat en réponse à la première question posée plus haut, « Quelles ont été les ventes du produit X en 1998 pour la région Nord-Ouest ? », peut impliquer la lecture de plusieurs milliers de lignes de données, l'extraction de la valeur de la colonne utilisée pour fournir la mesure Ventes pour chacune d'elle et enfin le calcul de la somme. De plus, le temps nécessaire pour extraire ces données varie selon le mode de stockage choisi pour les données (MOLAP, HOLAP ou ROLAP).  
  
## <a name="designing-aggregations"></a>Conception d'agrégations  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] intègre un algorithme complexe pour sélectionner des agrégations pour les précalculer et afin que d’autres agrégations puissent être calculées rapidement à partir de valeurs précalculées. Par exemple, si les agrégations font l'objet d'un calcul préalable pour le niveau Month d'une hiérarchie Time, le calcul d'un niveau Quarter nécessite seulement la synthèse de trois nombres qui peuvent être calculés rapidement à la demande. Cette technique permet d'économiser du temps de traitement et de réduire les besoins de stockage avec un impact minimal sur les temps de réponse aux requêtes.  
  
 L'Assistant Conception d'agrégation fournit des options qui permettent de désigner un mode de stockage et des contraintes de pourcentage dans l'algorithme et d'aboutir ainsi à un compromis satisfaisant entre les temps de réponse et les besoins de stockage. Cependant, l'algorithme de cet assistant suppose que toutes les requêtes possibles sont également probables. L'Assistant Optimisation de l'utilisation vous permet d'affiner la structure des agrégations pour un groupe de mesures en analysant les requêtes qui ont été soumises par les applications clientes. En utilisant cet Assistant pour ajuster l'agrégation d'un cube, vous pouvez accélérer les réponses aux requêtes fréquentes et ralentir les réponses aux requêtes plus rares, sans trop affecter l'espace de stockage nécessaire au cube.  
  
 Les agrégations sont conçues à l'aide des Assistants, mais elles ne sont véritablement calculées qu''au moment où la partition pour laquelle elles sont conçues est traitée. Après avoir créé l'agrégation, si la structure du cube change ou si des données sont ajoutées ou modifiées dans les tables sources du cube, vous devez généralement recréer les agrégations du cube et retraiter ce dernier.  
  
## <a name="see-also"></a>Voir aussi  
 [Traitement et Modes de stockage de partitions](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-partition-storage-modes-and-processing.md)  
  
  
