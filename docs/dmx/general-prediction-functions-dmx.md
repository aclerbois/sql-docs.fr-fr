---
title: Fonctions de prédiction générales (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- DMX
helpviewer_keywords:
- DMX [Analysis Services], functions
- mapping functions to query types [DMX]
- DMX [Analysis Services], prediction queries
- prediction queries [DMX]
- SELECT statement [DMX]
- Data Mining Extensions [Analysis Services], functions
- Data Mining Extensions [Analysis Services], prediction queries
ms.assetid: e128159a-0458-43c9-bfe9-129cb6cfbe1c
caps.latest.revision: 48
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: e1ae136608aab3a90430512dc809bfa179da7e51
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="general-prediction-functions-dmx"></a>Fonctions de prédiction générales (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Vous pouvez utiliser la **sélectionnez** instruction dans les Extensions DMX (Data Mining) pour créer différents types de requêtes. Une requête peut être utilisée pour retourner des informations sur le modèle d'exploration de données lui-même, afin de faire de nouvelles prédictions ou encore modifier le modèle en effectuant un apprentissage avec de nouvelles données. [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] fournit un éventail de fonctions spécialisées qui contrôlent le type d’informations qui sont retournées dans une requête. En ajoutant ces fonctions à une requête DMX, vous pouvez récupérer des statistiques ou des colonnes de données supplémentaires. Toutefois, pour chaque type de requête et chaque type de modèle, seules certaines fonctions sont prises en charge.  
  
## <a name="common-functions"></a>Fonctions communes  
 Vous pouvez utiliser des fonctions pour étendre les résultats retournés par un modèle d'exploration de données. Vous pouvez utiliser les fonctions suivantes pour toute **sélectionnez** instruction qui retourne une expression de table :  
  
|||  
|-|-|  
|[BottomCount &#40;DMX&#41;](../dmx/bottomcount-dmx.md)|[RangeMin &#40;DMX&#41;](../dmx/rangemin-dmx.md)|  
|[BottomPercent &#40;DMX&#41;](../dmx/bottompercent-dmx.md)|[TopCount &#40;DMX&#41;](../dmx/topcount-dmx.md)|  
|[Prédire & #40 ; DMX & #41 ;](../dmx/predict-dmx.md)|[TopPercent &#40;DMX&#41;](../dmx/toppercent-dmx.md)|  
|[RangeMax &#40;DMX&#41;](../dmx/rangemax-dmx.md)|[TopSum &#40;DMX&#41;](../dmx/topsum-dmx.md)|  
|[RangeMid &#40;DMX&#41;](../dmx/rangemid-dmx.md)||  
  
 De plus, les fonctions suivantes sont prises en charge pour la plupart des types de modèles :  
  
-   [Existe &#40;DMX&#41;](../dmx/exists-dmx.md)  
  
-   [IsDescendant & #40 ; DMX & #41 ;](../dmx/isdescendant-dmx.md)  
  
-   [IsTestCase &#40;DMX&#41;](../dmx/istestcase-dmx.md)  
  
-   [IsTrainingCase &#40;DMX&#41;](../dmx/istrainingcase-dmx.md)  
  
-   [Prédire & #40 ; DMX & #41 ;](../dmx/predict-dmx.md)  
  
-   [RangeMax &#40;DMX&#41;](../dmx/rangemax-dmx.md)  
  
-   [RangeMid &#40;DMX&#41;](../dmx/rangemid-dmx.md)  
  
-   [RangeMin &#40;DMX&#41;](../dmx/rangemin-dmx.md)  
  
-   [StructureColumn &#40;DMX&#41;](../dmx/structurecolumn-dmx.md)  
  
 Les algorithmes individuels peuvent prendre en charge des fonctions supplémentaires. Pour obtenir la liste des fonctions qui sont prises en charge par chaque type de modèle, consultez [requêtes d’exploration de données](../analysis-services/data-mining/data-mining-queries.md).  
  
## <a name="functions-specific-to-select-syntax"></a>Fonctions spécifiques à la syntaxe SELECT  
 Le tableau suivant répertorie les fonctions que vous pouvez utiliser pour chaque type de **sélectionnez** instruction.  
  
 Pour obtenir des informations générales sur les fonctions dans DMX, consultez [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md).  
  
|Type de requête|Fonctions prises en charge|Notes|  
|----------------|-------------------------|-------------|  
|[SELECT DISTINCT FROM \<modèle >](../dmx/select-distinct-from-model-dmx.md)|[RangeMin &#40;DMX&#41;](../dmx/rangemin-dmx.md)<br /><br /> [RangeMid &#40;DMX&#41;](../dmx/rangemid-dmx.md)<br /><br /> [RangeMax &#40;DMX&#41;](../dmx/rangemax-dmx.md)|Ces fonctions peuvent être utilisées pour fournir des valeurs maximales, des valeurs minimales et des moyennes pour toute colonne contenant un type de données numérique, que la colonne soit continue ou ait été discrétisée.|  
|[SELECT FROM \<modèle >. CONTENU](../dmx/select-from-model-content-dmx.md)<br /><br /> ou<br /><br /> [SELECT FROM \<modèle >. DIMENSION_CONTENT](../dmx/select-from-model-dimension-content-dmx.md)|[IsDescendant & #40 ; DMX & #41 ;](../dmx/isdescendant-dmx.md)|Cette fonction récupère les nœuds enfants pour le nœud spécifié dans le modèle. Elle peut être utilisée, par exemple, pour parcourir les nœuds dans le contenu du modèle d'exploration de données. La disposition des nœuds dans le contenu du modèle d'exploration de données dépend du type de modèle. Pour plus d’informations sur la structure pour chaque type de modèle d’exploration de données, consultez [contenu du modèle d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md).<br /><br /> Si vous avez enregistré le contenu du modèle d'exploration de données sous forme d'une dimension, vous pouvez aussi utiliser d'autres fonctions MDX (Multidimensional Expressions) disponibles pour interroger une hiérarchie d'attribut.|  
|[SELECT FROM \<modèle >. CAS](../dmx/select-from-model-cases-dmx.md)|[IsInNode & #40 ; DMX & #41 ;](../dmx/isinnode-dmx.md)<br /><br /> [Classe ClientSettingsGeneralFlag](../relational-databases/wmi-provider-configuration-classes/clientsettingsgeneralflag-class/clientsettingsgeneralflag-class.md)<br /><br /> [IsTrainingCase &#40;DMX&#41;](../dmx/istrainingcase-dmx.md)<br /><br /> [IsTestCase &#40;DMX&#41;](../dmx/istestcase-dmx.md)|La fonction Lag est pris en charge uniquement pour les modèles de série chronologique.<br /><br /> La fonction IsTestCase est pris en charge dans les modèles qui sont basées sur une structure qui a été créée à l’aide de l’option d’exclusion, pour créer un jeu de données de test. Si le modèle n'est pas basé sur une structure avec le jeu de test d'exclusion, tous les cas sont traités comme des cas d'apprentissage.|  
|[SELECT FROM \<modèle >. SAMPLE_CASES](../dmx/select-from-model-sample-cases-dmx.md)|[IsInNode & #40 ; DMX & #41 ;](../dmx/isinnode-dmx.md)|Dans ce contexte, la fonction IsInNode retourne un cas appartenant à un ensemble de cas d’exemple idéalisés.|  
|SELECT FROM \<modèle >. PMML|Non applicable. Utilisez plutôt des fonctions de requête XML.|Les représentations PMML ne sont prises en charge que pour les types de modèles suivants :<br /><br /> [!INCLUDE[msCoName](../includes/msconame-md.md)] Decision Trees<br /><br /> [!INCLUDE[msCoName](../includes/msconame-md.md)] Clustering|  
|[SELECT FROM \<modèle > PREDICTION JOIN](../dmx/select-from-model-prediction-join-dmx.md)|Fonctions de prédiction spécifiques à l'algorithme que vous utilisez pour générer le modèle.|Pour obtenir la liste des fonctions de prédiction pour chaque type de modèle, consultez [requêtes d’exploration de données](../analysis-services/data-mining/data-mining-queries.md).|  
|[SELECT FROM \<modèle >](../dmx/select-from-model-dmx.md)|Fonctions de prédiction spécifiques à l'algorithme que vous utilisez pour générer le modèle.|Pour obtenir la liste des fonctions de prédiction pour chaque type de modèle, consultez [requêtes d’exploration de données](../analysis-services/data-mining/data-mining-queries.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
