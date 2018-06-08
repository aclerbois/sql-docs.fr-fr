---
title: SystemGetAccuracyResults (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 54fc91b67a695110383c19422befab0d7b0f7a9d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="systemgetaccuracyresults-analysis-services---data-mining"></a>SystemGetAccuracyResults (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Retourne les mesures de précision de validation croisée d'une structure d'exploration de données et de tous les modèles connexes, à l'exclusion des modèles de clustering.  
  
 Cette procédure stockée retourne les mesures du jeu de données dans son ensemble sous forme de partition unique. Pour partitionner le jeu de données en sections croisées et retourner les mesures pour chaque partition, utilisez [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md).  
  
> [!NOTE]  
>  Cette procédure stockée n'est pas prise en charge pour les modèles créés à l'aide de l’algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) ou de l’algorithme MSC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering). Également, pour les modèles de clustering, utilisez la procédure stockée distincte [SystemGetClusterAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SystemGetAccuracyResults(<mining structure>,   
[,<mining model list>]  
,<data set>  
,<target attribute>  
[,<target state>]  
[,<target threshold>]  
[,<test list>])  
```  
  
## <a name="arguments"></a>Arguments  
 *structure d'exploration de données*  
 Nom d'une structure d'exploration de données dans la base de données active.  
  
 (Obligatoire)  
  
 *model list*  
 Liste séparée par des virgules des modèles à valider.  
  
 La valeur par défaut est **null**. Tous les modèles applicables sont alors utilisés. Lorsque la valeur par défaut est utilisée, les modèles de clustering sont automatiquement exclus de la liste des candidats au traitement.  
  
 (Facultatif)  
  
 *data set*  
 Valeur entière indiquant quelle partition de la structure d'exploration de données est utilisée pour le test. Cette valeur est dérivée d'un masque de bits qui représente la somme des valeurs suivantes, où chaque valeur individuelle est facultative :  
  
|||  
|-|-|  
|Cas d'apprentissage|0x0001|  
|Scénarios de test|0x0002|  
|Filtre de modèle|0x0004|  
  
 Pour obtenir la liste complète des valeurs possibles, consultez la section Remarques de cette rubrique.  
  
 (obligatoire)  
  
 *target attribute*  
 Chaîne qui contient le nom d'un objet prévisible. Un objet prévisible peut être une colonne, une colonne de table imbriquée ou une colonne clé de table imbriquée d'un modèle d'exploration de données.  
  
 (obligatoire)  
  
 *target state*  
 Chaîne qui contient une valeur spécifique à prédire.  
  
 Si une valeur est spécifiée, les mesures sont collectées pour cet état spécifique.  
  
 Si aucune valeur n'est spécifiée, ou si la valeur null est spécifiée, les mesures sont calculées pour l'état le plus probable pour chaque prédiction.  
  
 La valeur par défaut est **null**.  
  
 (Facultatif)  
  
 *target threshold*  
 Nombre compris entre 0.0 et 1 qui spécifie la probabilité minimale pour que la valeur de prédiction soit comptabilisée comme correcte.  
  
 La valeur par défaut est **null**, ce qui signifie que toutes les prédictions sont comptabilisées comme correctes.  
  
 (Facultatif)  
  
 *test list*  
 Chaîne qui spécifie les options de test. Ce paramètre est réservé à un usage futur.  
  
 (Facultatif)  
  
## <a name="return-type"></a>Type de retour  
 L'ensemble de lignes retourné contient des scores pour chaque partition et des agrégats pour tous les modèles.  
  
 Le tableau ci-dessous dresse la liste des colonnes renvoyées par **GetValidationResults**.  
  
|Nom de la colonne| Description|  
|-----------------|-----------------|  
|Modèle|Nom du modèle qui a été testé. **All** indique que le résultat est un agrégat de tous les modèles.|  
|AttributeName|Nom de la colonne prédictible.|  
|AttributeState|Une valeur cible dans la colonne prédictible.<br /><br /> Si cette colonne contient une valeur, les mesures sont collectées uniquement pour l'état spécifié.<br /><br /> Si cette valeur n'est spécifiée, ou si la valeur null est spécifiée, les mesures sont calculées pour l'état le plus probable pour chaque prédiction.|  
|PartitionIndex|Indique la partition à laquelle le résultat s'applique.<br /><br /> Pour cette procédure, toujours 0.|  
|PartitionCases|Entier qui indique le nombre de lignes dans le jeu de cas, selon la  *\<jeu de données >* paramètre.|  
|Test|Type de test qui a été effectué.|  
|Measure|Nom de la mesure retournée par le test. Les mesures de chaque modèle dépendent du type de modèle et du type de valeur prévisible.<br /><br /> Pour obtenir la liste des mesures retournées pour chaque type prévisible, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).<br /><br /> Pour obtenir une définition de chaque mesure, consultez [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md).|  
|Valeur|Valeur de la mesure spécifiée.|  
  
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
 Cet exemple retourne les mesures de précision d’un modèle d’arbre de décision unique, `v Target Mail DT`, associé à la structure d’exploration de données `vTargetMail` . Le code de la ligne quatre indique que les résultats doivent être basés sur les scénarios de test, filtrés pour chaque modèle par le filtre spécifique à ce modèle.  `[Bike Buyer]` spécifie la colonne qui doit être prédite, et le 1 sur la ligne suivante indique que le modèle sera évalué uniquement pour la valeur 1 spécifique, qui signifie « Oui, achètera ».  
  
 La dernière ligne du code spécifie que la valeur de seuil d'état est 0,5. Cela signifie que les prédictions dont la probabilité est supérieure à 50 pour cent doivent être comptabilisées comme de « bonnes » prédictions lors du calcul de la précision.  
  
```  
CALL SystemGetAccuracyResults (  
[vTargetMail],  
[vTargetMail DT],  
6,  
'Bike Buyer',  
1,  
0.5  
)  
```  
  
 Exemples de résultats :  
  
|ModelName|AttributeName|AttributeState|PartitionIndex|PartitionSize|Test|Mesure|Valeur|  
|---------------|-------------------|--------------------|--------------------|-------------------|----------|-------------|-----------|  
|v Target Mail DT|Bike Buyer|1|0|1638|classification.|Vrai positif|605|  
|v Target Mail DT|Bike Buyer|1|0|1638|classification.|Faux positif|177|  
|v Target Mail DT|Bike Buyer|1|0|1638|classification.|Vrai négatif|501|  
|v Target Mail DT|Bike Buyer|1|0|1638|classification.|Faux négatif|355|  
|v Target Mail DT|Bike Buyer|1|0|1638|Vraisemblance|Score du journal|-0.598454638753028|  
|v Target Mail DT|Bike Buyer|1|0|1638|Vraisemblance|Finesse|0.0936717116894395|  
|v Target Mail DT|Bike Buyer|1|0|1638|Vraisemblance|Erreur quadratique moyenne|0.361630800104946|  
  
## <a name="requirements"></a>Spécifications  
 La validation croisée est uniquement disponible dans [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)] depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetAccuracyResults](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md)   
 [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetClusterAccuracyResults & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md)  
  
  
