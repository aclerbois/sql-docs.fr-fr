---
title: SystemGetCrossValidationResults (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2febed19e2bd481a8e442f115f9691e5abb6be4b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="systemgetcrossvalidationresults-analysis-services---data-mining"></a>SystemGetCrossValidationResults (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Partitionne la structure d'exploration de données dans le nombre spécifié de sections croisées, effectue l'apprentissage d'un modèle pour chaque partition, puis retourne les mesures de précision de chaque partition.  
  
> [!NOTE]  
>  Cette procédure stockée ne peut pas être utilisée pour effectuer la validation croisée de modèles de clustering ou de modèles créés à l’aide de l’algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) ou de l’algorithme MSC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering). Pour la validation croisée des modèles de clustering, vous pouvez utiliser la procédure stockée distincte [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SystemGetCrossValidationResults(  
<mining structure>  
[, <mining model list>]  
,<fold count>  
,<max cases>  
,<target attribute>  
[,<target state>]  
[,<target threshold>]  
[,<test list>])  
```  
  
## <a name="arguments"></a>Arguments  
 *structure d'exploration de données*  
 Nom d'une structure d'exploration de données dans la base de données active.  
  
 (obligatoire)  
  
 *mining model list*  
 Liste séparée par des virgules des modèles d'exploration de données à valider.  
  
 Si un nom de modèle contient des caractères qui ne sont pas valides dans le nom d'un identificateur, ce nom doit être placé entre parenthèses.  
  
 Si une liste de modèles d'exploration de données n'est pas spécifiée, la validation croisée est effectuée par rapport à tous les modèles associés à la structure spécifiée et contenant un attribut prévisible.  
  
> [!NOTE]  
>  Pour la validation croisée des modèles de clustering, vous devez utiliser une procédure stockée distincte, [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md).  
  
 (Facultatif)  
  
 *nombre de replis*  
 Entier qui spécifie en combien de partitions séparer le jeu de données. La valeur minimale est 2. Le nombre maximal de replis est **maximum integer** ou le nombre de cas, la valeur la plus petite étant retenue.  
  
 Chaque partition contiendra environ le nombre de cas suivant : *max cases*/*fold count*.  
  
 Il n’y a pas de valeur par défaut.  
  
> [!NOTE]  
>  Le nombre de replis affecte considérablement le temps nécessaire pour effectuer la validation croisée. Si vous sélectionnez un nombre trop élevé, la requête risque de s'exécuter très longtemps et dans certains cas le serveur peut ne plus répondre ou dépasser le délai d'attente.  
  
 (obligatoire)  
  
 *max cases*  
 Entier qui spécifie le nombre maximal de cas qui peuvent être testés pour tous les replis.  
  
 La valeur 0 indique que tous les cas de la source de données seront utilisés.  
  
 Si vous spécifiez une valeur supérieure au nombre réel de cas dans le jeu de données, tous les cas de la source de données sont utilisés.  
  
 Il n’y a pas de valeur par défaut.  
  
 (obligatoire)  
  
 *target attribute*  
 Chaîne qui contient le nom de l'attribut prévisible. Un attribut prévisible peut être une colonne, une colonne de table imbriquée ou une colonne clé de table imbriquée d'un modèle d'exploration de données.  
  
> [!NOTE]  
>  L'existence de l'attribut cible est validée uniquement au moment de l'exécution.  
  
 (obligatoire)  
  
 *target state*  
 Formule qui spécifie la valeur à prédire. Si une valeur cible est spécifiée, les mesures sont recueillies uniquement pour la valeur spécifiée.  
  
 Si aucune valeur n’est spécifiée ou si elle est **null**, les mesures sont calculées pour l’état le plus probable pour chaque prédiction.  
  
 La valeur par défaut est **null**.  
  
 Une erreur est générée pendant la validation si la valeur spécifiée n'est pas valide pour l'attribut spécifié, ou si la formule n'est pas du type correct pour l'attribut spécifié.  
  
 (Facultatif)  
  
 *target*  *threshold*  
 **Double** supérieur à 0 et inférieur à 1. Indique le score de probabilité minimal qui doit être obtenu pour que la prédiction de l'état cible spécifié soit comptabilisée comme correcte.  
  
 Une prédiction ayant une probabilité inférieure ou égale à cette valeur est considérée comme incorrecte.  
  
 Si aucune valeur n’est spécifiée ou si elle est **null**, l’état le plus probable est utilisé, quel que soit son score de probabilité.  
  
 La valeur par défaut est **null**.  
  
> [!NOTE]  
>  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne génère pas d’une erreur si vous définissez *seuil d’état* à 0.0, mais vous ne devez jamais utiliser cette valeur. En effet, un seuil de 0.0 signifie que les prédictions avec une probabilité de 0 pour cent sont comptabilisées comme correctes.  
  
 (Facultatif)  
  
 *test list*  
 Chaîne qui spécifie les options de test.  
  
 **Remarque** : ce paramètre est réservé à un usage futur.  
  
 (Facultatif)  
  
## <a name="return-type"></a>Type de retour  
 L'ensemble de lignes retourné contient des scores pour chaque partition dans chaque modèle.  
  
 Le tableau suivant décrit les colonnes de l'ensemble de lignes.  
  
|Nom de la colonne| Description|  
|-----------------|-----------------|  
|ModelName|Nom du modèle qui a été testé.|  
|AttributeName|Nom de la colonne prédictible.|  
|AttributeState|Valeur cible spécifiée dans la colonne prédictible. Si cette valeur est **null**, la prédiction la plus probable a été utilisée.<br /><br /> Si cette colonne contient une valeur, la précision du modèle est évaluée uniquement par rapport à cette valeur.|  
|PartitionIndex|Index de base 1 qui identifie la partition à laquelle s'appliquent les résultats.|  
|PartitionSize|Entier qui indique combien de cas ont été inclus dans chaque partition.|  
|Test|Catégorie du test qui a été effectué. Pour obtenir une description des catégories et des tests inclus dans chaque catégorie, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).|  
|Measure|Nom de la mesure retournée par le test. Les mesures de chaque modèle dépendent du type de valeur prévisible. Pour obtenir une définition de chaque mesure, consultez [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md).<br /><br /> Pour obtenir la liste des mesures retournées pour chaque type prévisible, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).|  
|Valeur|Valeur de la mesure de test spécifiée.|  
  
## <a name="remarks"></a>Notes  
 Pour retourner des mesures de précision pour le jeu de données complet, utilisez [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md).  
  
 Si le modèle d’exploration de données a déjà été partitionné en replis, vous pouvez contourner le traitement et retourner uniquement les résultats de la validation croisée en utilisant [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment partitionner une structure d'exploration de données pour la validation croisée en deux replis, puis comment tester deux modèles d'exploration de données associés à la structure d'exploration de données, `[v Target Mail]`.  
  
 La ligne trois du code répertorie les modèles d'exploration de données que vous souhaitez tester. Si vous ne spécifiez pas la liste, tous les modèles qui ne sont pas des modèles de clustering et qui sont associés à la structure sont utilisés. La ligne quatre du code spécifie le nombre de partitions. Dans la mesure où aucune valeur n’est spécifiée pour *max cases*, tous les cas de la structure d’exploration de données sont utilisés et distribués de manière égale entre les partitions.  
  
 La ligne cinq spécifie l'attribut prévisible, Bike Buyer, et la ligne six spécifie la valeur à prédire, 1 (qui signifie « oui, achètera »).  
  
 La valeur NULL de la ligne sept indique qu'il n'existe pas de seuil de probabilité minimal à atteindre. Par conséquent, la première prédiction dont la probabilité est différente de zéro est utilisée pour évaluer la précision.  
  
```  
CALL SystemGetCrossValidationResults(  
[v Target Mail],  
[Target Mail DT], [Target Mail NB],  
2,  
'Bike Buyer',  
1,  
NULL  
)  
```  
  
 Exemples de résultats :  
  
|ModelName|AttributeName|AttributeState|PartitionIndex|PartitionSize|Test|Mesure|Valeur|  
|---------------|-------------------|--------------------|--------------------|-------------------|----------|-------------|-----------|  
|Target Mail DT|Bike Buyer|1|1|500|classification.|Vrai positif|144|  
|Target Mail DT|Bike Buyer|1|1|500|classification.|Faux positif|105|  
|Target Mail DT|Bike Buyer|1|1|500|classification.|Vrai négatif|186.|  
|Target Mail DT|Bike Buyer|1|1|500|classification.|Faux négatif|65|  
|Target Mail DT|Bike Buyer|1|1|500|Vraisemblance|Score du journal|-0.619042807138345|  
|Target Mail DT|Bike Buyer|1|1|500|Vraisemblance|Finesse|0.0740963734002671|  
|Target Mail DT|Bike Buyer|1|1|500|Vraisemblance|Erreur quadratique moyenne|0.346946279977653|  
|Target Mail DT|Bike Buyer|1|2|500|classification.|Vrai positif|162|  
|Target Mail DT|Bike Buyer|1|2|500|classification.|Faux positif|86|  
|Target Mail DT|Bike Buyer|1|2|500|classification.|Vrai négatif|165|  
|Target Mail DT|Bike Buyer|1|2|500|classification.|Faux négatif|87|  
|Target Mail DT|Bike Buyer|1|2|500|Vraisemblance|Score du journal|0.654117781086519|  
|Target Mail DT|Bike Buyer|1|2|500|Vraisemblance|Finesse|0.038997399132084|  
|Target Mail DT|Bike Buyer|1|2|500|Vraisemblance|Erreur quadratique moyenne|0.342721344892651|  
  
## <a name="requirements"></a>Spécifications  
 La validation croisée est uniquement disponible dans [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)] depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [SystemGetCrossValidationResults](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md)   
 [SystemGetClusterCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetClusterAccuracyResults & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md)  
  
  
