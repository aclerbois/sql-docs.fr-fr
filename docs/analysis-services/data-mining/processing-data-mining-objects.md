---
title: Traitement des objets d’exploration de données | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 017f1d751e81fa80b8a7e4c2655fd1de59459fed
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="processing-data-mining-objects"></a>Traitement des objets d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un objet d'exploration de données n'est rien de plus qu'un conteneur vide tant qu'il n'est pas traité. Le*traitement* d’un modèle d’exploration de données est également appelé *apprentissage*.  
  
 **Traitement des structures d’exploration de données :** une structure d’exploration de données reçoit les données d’une source de données externe, comme défini par les liaisons de colonne et les métadonnées d’utilisation, puis elle les lit. Ces données sont lues entièrement, puis analysées pour extraire différentes statistiques. Analysis Services contient une représentation compacte des données, appropriée pour l'analyse par les algorithmes d'exploration de données, dans un cache local. Vous pouvez conserver ce cache ou le supprimer lorsque les modèles ont été traités. Par défaut, le cache est stocké. Pour plus d’informations, consultez [Traiter une structure d’exploration de données](../../analysis-services/data-mining/process-a-mining-structure.md).  
  
 **Traitement des modèles d’exploration de données :** un modèle d’exploration de données est vide (contenant uniquement des définitions) jusqu’à ce qu’il soit traité. Pour traiter un modèle d'exploration de données, la structure d'exploration de données doit avoir fait l'objet d'un traitement. Le modèle d'exploration de données reçoit les données du cache de la structure d'exploration de données, il applique tous les filtres qui ont pu être créés sur le modèle, puis transmet le jeu de données via l'algorithme pour détecter des motifs. Une fois le modèle traité, il stocke uniquement les résultats du traitement, et non les données. Pour plus d’informations, consultez [Traiter un modèle d’exploration de données](../../analysis-services/data-mining/process-a-mining-model.md).  
  
 Le diagramme suivant illustre le flux de données lors du traitement d'une structure d'exploration de données et d'un modèle d'exploration de données.  
  
 ![Traitement des données : source à structure à modèle](../../analysis-services/data-mining/media/dmcon-modelarch.gif "traitement des données : source à structure à modèle")  
  
## <a name="viewing-the-results-of-processing"></a>Affichage des résultats du traitement  
 Lorsqu'une structure d'exploration de données a été traitée, elle contient une représentation compacte des données à utiliser dans l'analyse statistique. Si le cache n'a pas été effacé, il est possible d'accéder à ses données en utilisant l'une des méthodes suivantes :  
  
-   Création d'une requête Data Mining Extensions (DMX) sur le modèle et extraction dans la structure. Pour plus d’informations, consultez [SELECT FROM &#60;modèle&#62;.CASES &#40;DMX&#41;](../../dmx/select-from-model-cases-dmx.md).  
  
-   Navigation dans un modèle découlant de la structure et utilisation de l'une des options de l'interface utilisateur pour extraire des cas de structure. Pour plus d’informations, consultez [Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)ou [Extraire des données de cas à partir d’un modèle d’exploration de données](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md).  
  
-   Création d'une requête DMX sur les cas de structure. Pour plus d’informations, consultez [SELECT FROM &#60;structure&#62;.CASES](../../dmx/select-from-structure-cases.md).  
  
 Lorsqu'un modèle d'exploration de données a été traité, il contient uniquement les motifs découlant de l'analyse et les mappages des résultats du modèle vers les données d'apprentissage mises en cache. Vous pouvez parcourir ou interroger les résultats du modèle, appelés *contenu du modèle*, ou bien interroger le modèle et les cas de structure, s’ils ont été mis en cache.  
  
 Le contenu de chaque modèle d'exploration de données dépend de l'algorithme utilisé pour sa création. Par exemple, si un modèle est un modèle de clustering et si un autre est un modèle d'arbres de décision, le contenu de ces modèles sera très différent même s'ils utilisent exactement les mêmes données. Pour plus d’informations, consultez [Contenu du modèle d’exploration &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md).  
  
## <a name="processing-requirements"></a>Impératifs liés au traitement  
 Les impératifs liés au traitement peuvent être différents selon que vos modèles d'exploration de données sont basés uniquement sur les données relationnelles ou sur une source de données multidimensionnelles.  
  
 Pour la source de données relationnelles, le traitement requiert uniquement la création de données d'apprentissage et l'exécution d'algorithmes d'exploration sur ces données. Toutefois, les modèles d'exploration de données basés sur les objets OLAP, tels que les dimensions et les mesures, requièrent que les données sous-jacentes soient dans un état traité. Il peut être nécessaire que les objets multidimensionnels soient traités pour remplir le modèle d'exploration de données.  
  
 Pour plus d’informations, consultez [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’extraction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/drillthrough-queries-data-mining.md)   
 [Les Structures d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)   
 [Les modèles d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-models-analysis-services-data-mining.md)   
 [Architecture logique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/logical-architecture-analysis-services-data-mining.md)  
  
  
