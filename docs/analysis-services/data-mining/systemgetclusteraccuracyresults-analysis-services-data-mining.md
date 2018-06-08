---
title: SystemGetClusterAccuracyResults (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b9521cfd6e2b9ec0d08f290c60167c682c98e46f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="systemgetclusteraccuracyresults-analysis-services---data-mining"></a>SystemGetClusterAccuracyResults (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Retourne les mesures de précision de validation croisée d'une structure d'exploration de données et de tous les modèles de clustering connexes.  
  
 Cette procédure stockée retourne les mesures du jeu de données dans sa totalité sous forme de partition unique. Pour partitionner le jeu de données en sections croisées et retourner les mesures pour chaque partition, utilisez [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md).  
  
> [!NOTE]  
>  Cette procédure stockée fonctionne uniquement pour les modèles de clustering. Pour les autres modèles, utilisez [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SystemGetClusterAccuracyResults(  
<mining structure>   
[,<mining model list>]  
,<data set>  
,<test list>])  
```  
  
## <a name="arguments"></a>Arguments  
 *structure d'exploration de données*  
 Nom d'une structure d'exploration de données dans la base de données active.  
  
 (Obligatoire)  
  
 *mining model list*  
 Liste séparée par des virgules des modèles à valider.  
  
 La valeur par défaut est **null**, ce qui signifie que tous les modèles applicables sont utilisés. Lorsque la valeur par défaut est utilisée, les modèles qui ne sont pas des modèles de clustering sont automatiquement exclus de la liste des candidats au traitement.  
  
 (Facultatif)  
  
 *jeu de données*  
 Valeur entière indiquant quelle partition de la structure d'exploration de données doit être utilisée pour le test. Cette valeur est dérivée d'un masque de bits qui représente la somme des valeurs suivantes, où chaque valeur individuelle est facultative :  
  
|||  
|-|-|  
|Cas d'apprentissage|0x0001|  
|Scénarios de test|0x0002|  
|Filtre de modèle|0x0004|  
  
 Pour obtenir la liste complète des valeurs possibles, consultez la section Remarques de cette rubrique.  
  
 (Obligatoire)  
  
 *test list*  
 Chaîne qui spécifie les options de test. Ce paramètre est réservé à un usage futur.  
  
 (Facultatif)  
  
## <a name="return-type"></a>Type de retour  
 Tableau qui contient des scores pour chaque partition individuelle et des agrégats pour tous les modèles.  
  
 Le tableau ci-dessous dresse la liste des colonnes renvoyées par **SystemGetClusterAccuracyResults**. Pour en savoir plus sur l’interprétation des informations retournées par la procédure stockée, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).  
  
|Nom de la colonne| Description|  
|-----------------|-----------------|  
|ModelName|Nom du modèle qui a été testé. **All** indique que le résultat est un agrégat de tous les modèles.|  
|AttributeName|Non applicable aux modèles de clustering.|  
|AttributeState|Non applicable aux modèles de clustering.|  
|PartitionIndex|Numéro qui indique la partition.<br /><br /> Pour cette procédure stockée, ce numéro est toujours 0.|  
|PartitionCases|Entier qui indique combien de cas ont été testés.|  
|Test|Type de test qui a été effectué.|  
|Measure|Nom de la mesure retournée par le test. Les mesures de chaque modèle dépendent du type de modèle et du type de valeur prévisible.<br /><br /> Pour obtenir la liste des mesures retournées pour chaque type prévisible, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).<br /><br /> Pour obtenir une définition de chaque mesure, consultez [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md).|  
|Valeur|Score de probabilité qui indique la vraisemblance du cas de cluster.|  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant fournit des exemples des valeurs que vous pouvez utiliser pour spécifier les données de la structure d'exploration de données qui sont utilisées pour la validation croisée. Si vous souhaitez utiliser des scénarios de test pour la validation croisée, la structure d'exploration de données doit déjà contenir un jeu de données de test. Pour plus d’informations sur la définition d’un jeu de données de test quand vous créez une structure d’exploration de données, consultez [Jeux de données d’apprentissage et de test](../../analysis-services/data-mining/training-and-testing-data-sets.md).  
  
|Valeur de type entier| Description|  
|-------------------|-----------------|  
|1|Seuls les cas d'apprentissage sont utilisés.|  
|2|Seuls les scénarios de test sont utilisés.|  
|3|Les cas d'apprentissage et les scénarios de test sont utilisés.|  
|4|Combinaison incorrecte.|  
|5|Seuls les cas d'apprentissage sont utilisés. Le filtre de modèle est appliqué.|  
|6|Seuls les scénarios de test sont utilisés. Le filtre de modèle est appliqué.|  
|7|Les cas d'apprentissage et les scénarios de test sont utilisés. Le filtre de modèle est appliqué.|  
  
 Pour plus d’informations sur les scénarios dans lesquels utiliser la validation croisée, consultez [Test et validation &#40;exploration de données&#41;](../../analysis-services/data-mining/testing-and-validation-data-mining.md).  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne les mesures de précision de deux modèles de clustering, nommées `Cluster 1` et `Cluster 2`, associés à la structure d'exploration de données vTargetMail. Le code de la ligne quatre indique que les résultats doivent reposer sur les scénarios de test seulement, sans utiliser les filtres éventuellement associés à chaque modèle.  
  
```  
CALL SystemGetClusterAccuracyResults (  
[vTargetMail],  
[Cluster 1], [Cluster 2],  
2  
)  
```  
  
 Exemples de résultats :  
  
|ModelName|AttributeName|AttributeState|PartitionIndex|PartitionSize|Test|Mesure|Valeur|  
|---------------|-------------------|--------------------|--------------------|-------------------|----------|-------------|-----------|  
|Cluster 1|||0|5545|Clustering|Probabilité de cas|0.796514342249313|  
|Cluster 2|||0|5545|Clustering|Probabilité de cas|0.732122471228572|  
  
## <a name="requirements"></a>Spécifications  
 La validation croisée est uniquement disponible dans [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)] à compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md)   
 [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md)   
 [SystemClusterGetAccuracyResults](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md)  
  
  
