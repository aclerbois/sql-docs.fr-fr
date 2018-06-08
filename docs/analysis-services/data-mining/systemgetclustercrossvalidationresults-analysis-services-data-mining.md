---
title: SystemGetClusterCrossValidationResults (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ce11f7cb54d40336633d09a5d6601f0366c6bf55
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="systemgetclustercrossvalidationresults-analysis-services---data-mining"></a>SystemGetClusterCrossValidationResults (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Partitionne la structure d'exploration de données dans le nombre spécifié de sections croisées, effectue l'apprentissage d'un modèle pour chaque partition, puis retourne les mesures de précision de chaque partition.  
  
 **Remarque** : cette procédure stockée ne peut être utilisée qu’avec une structure d’exploration de données qui contient au moins un modèle de clustering. Pour la validation croisée des autres modèles, vous devez utiliser [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SystemGetClusterCrossValidationResults(  
<structure name>,   
[,<mining model list>]  
,<fold count>}  
,<max cases>  
<test list>])  
```  
  
## <a name="arguments"></a>Arguments  
 *structure d'exploration de données*  
 Nom d'une structure d'exploration de données dans la base de données active.  
  
 (obligatoire)  
  
 *mining model list*  
 Liste séparée par des virgules des modèles d'exploration de données à valider.  
  
 Si une liste de modèles d'exploration de données n'est pas spécifiée, la validation croisée est effectuée par rapport à tous les modèles de clustering associés à la structure spécifiée.  
  
> [!NOTE]  
>  Pour la validation croisée des modèles autres que des modèles de clustering, vous devez utiliser une procédure stockée distincte, [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md).  
  
 (Facultatif)  
  
 *nombre de replis*  
 Entier qui spécifie en combien de partitions séparer le jeu de données. La valeur minimale est 2. Le nombre maximal de replis est **maximum integer** ou le nombre de cas, la valeur la plus petite étant retenue.  
  
 Chaque partition contiendra environ le nombre de cas suivant : *max cases*/*fold count*.  
  
 Il n’y a pas de valeur par défaut.  
  
> [!NOTE]  
>  Le nombre de replis affecte considérablement le temps nécessaire pour effectuer la validation croisée. Si vous sélectionnez un nombre trop élevé, la requête risque de s’exécuter très longtemps et dans certains cas le serveur peut ne plus répondre ou dépasser le délai d’attente.  
  
 (obligatoire)  
  
 *max cases*  
 Entier qui spécifie le nombre maximal de cas qui peuvent être testés.  
  
 La valeur 0 indique que tous les cas de la source de données seront utilisés.  
  
 Si vous spécifiez un nombre supérieur au nombre réel de cas dans le jeu de données, tous les cas de la source de données sont utilisés.  
  
 (obligatoire)  
  
 *test list*  
 Chaîne qui spécifie les options de test.  
  
 **Remarque** : ce paramètre est réservé à un usage futur.  
  
 (Facultatif)  
  
## <a name="return-type"></a>Type de retour  
 Le tableau  Type de valeur renvoyée contient des scores pour chaque partition individuelle et des agrégats pour tous les modèles.  
  
 Le tableau suivant décrit les colonnes retournées.  
  
|Nom de la colonne| Description|  
|-----------------|-----------------|  
|ModelName|Nom du modèle qui a été testé.|  
|AttributeName|Nom de la colonne prédictible. Pour les modèles de cluster, toujours **null**.|  
|AttributeState|Valeur cible spécifiée dans la colonne prédictible. Pour les modèles de cluster, toujours **null.**|  
|PartitionIndex|Index de base 1 qui identifie la partition à laquelle s’appliquent les résultats.|  
|PartitionSize|Entier qui indique combien de cas ont été inclus dans chaque partition.|  
|Test|Type de test qui a été effectué.|  
|Measure|Nom de la mesure retournée par le test. Les mesures de chaque modèle dépendent du type de valeur prévisible. Pour obtenir une définition de chaque mesure, consultez [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md).<br /><br /> Pour obtenir la liste des mesures retournées pour chaque type prévisible, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).|  
|Valeur|Valeur de la mesure de test spécifiée.|  
  
## <a name="remarks"></a>Notes  
 Pour retourner des mesures de précision pour tout l’ensemble de données, utilisez [SystemGetClusterAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md).  
  
 Par ailleurs, si le modèle d’exploration de données a déjà été partitionné en replis, vous pouvez contourner le traitement et retourner uniquement les résultats de la validation croisée en utilisant [SystemGetClusterAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment partitionner une structure d'exploration de données en trois replis, puis comment tester deux modèles de clustering associés à la structure d'exploration de données.  
  
 La ligne trois du code répertorie les modèles d'exploration de données spécifiques que vous souhaitez tester. Si vous ne spécifiez pas la liste, tous les modèles de clustering associés à la structure sont utilisés.  
  
 La ligne quatre du code spécifie le nombre de replis, et la ligne cinq spécifie le nombre maximal de cas à utiliser.  
  
 Étant donné qu'il s'agit de modèles de clustering, vous n'avez pas besoin de spécifier un attribut ou une valeur prévisible.  
  
```  
CALL SystemGetClusterCrossValidationResults(  
[v Target Mail],  
[Cluster 1], [Cluster 2],  
3,  
10000  
)  
```  
  
 Exemples de résultats :  
  
|ModelName|AttributeName|AttributeState|PartitionIndex|PartitionSize|Test|Mesure|Valeur|  
|---------------|-------------------|--------------------|--------------------|-------------------|----------|-------------|-----------|  
|Cluster 1|||1|3025|Clustering|Probabilité de cas|0.930524511864121|  
|Cluster 1|||2|3025|Clustering|Probabilité de cas|0.919184178430778|  
|Cluster 1|||3|3024|Clustering|Probabilité de cas|0.929651120490248|  
|Cluster 2|||1|1289|Clustering|Probabilité de cas|0.922789726933607|  
|Cluster 2|||2|1288|Clustering|Probabilité de cas|0.934865535691068|  
|Cluster 2|||3|1288|Clustering|Probabilité de cas|0.924724595688798|  
  
## <a name="requirements"></a>Spécifications  
 La validation croisée est uniquement disponible dans [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)] à compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md)   
 [SystemGetClusterCrossValidationResults](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md)   
 [SystemGetClusterAccuracyResults & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md)  
  
  
