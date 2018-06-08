---
title: SELECT INTO (DMX) | Documents Microsoft
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
f1_keywords:
- SELECT
- SELECT_INTO
- SELECT INTO
dev_langs:
- DMX
helpviewer_keywords:
- mining models [Analysis Services], copying
- SELECT INTO statement
- mining models [Analysis Services], creating
- copying mining models
ms.assetid: 31ab9b4c-e20d-41ee-886f-6665c22c6ad5
caps.latest.revision: 42
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: ab2052950624fa7c322336e3855bda72e74726f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="select-into-dmx"></a>SELECT INTO (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Crée un modèle d'exploration de données, sur la base de la structure d'exploration de données d'un modèle existant. Le **SELECT INTO** crée le nouveau modèle d’exploration de données en copiant le schéma et autres informations qui ne sont pas spécifiques à l’algorithme.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SELECT INTO <new model>   
USING <algorithm> [(<parameter list>)] [WITH DRILLTHROUGH[,] [FILTER(<expression>)]]  
FROM <existing model>  
```  
  
## <a name="arguments"></a>Arguments  
 *nouveau modèle*  
 Nom unique du nouveau modèle en cours de création.  
  
 *Algorithme*  
 Nom défini par le fournisseur d'un algorithme d'exploration de données  
  
 *Liste de paramètres*  
 Ce paramètre est facultatif. Liste séparée par des virgules des paramètres définis par le fournisseur de l'algorithme.  
  
 *expression*  
 Expression dont le résultat est une condition de filtre valide sur les données d'apprentissage. Pour plus d’informations sur les expressions qui peuvent être utilisées comme filtres, consultez [filtres pour les modèles d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md).  
  
 *modèle existant*  
 Nom du modèle existant à copier.  
  
## <a name="remarks"></a>Notes  
 Si l'apprentissage du modèle existant est effectué, le nouveau modèle est automatiquement traité lors de l'exécution de cette instruction. Dans le cas contraire, le nouveau modèle reste non traité.  
  
 Le **SELECT INTO** instruction fonctionne uniquement si la structure du modèle existant est compatible avec l’algorithme du nouveau modèle. Par conséquent, cette instruction s'avère très utile pour créer et tester rapidement des modèles basés sur le même algorithme. Si vous modifiez le type d'algorithme, le nouvel algorithme doit prendre en charge le type de données de chaque colonne du modèle existant, sinon une erreur peut se produire lors du traitement du modèle.  
  
 Le **WITH DRILLTHROUGH** clause active l’extraction sur le nouveau modèle d’exploration de données. L'extraction ne peut être activée que lors de la création du modèle.  
  
## <a name="example-1-altering-the-parameters-of-the-model"></a>Exemple 1 : modification des paramètres du modèle  
 L’exemple suivant crée un nouveau modèle d’exploration de données basé sur un modèle d’exploration de données existant, `TM_Clustering`, que vous créez dans le [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). Le paramètre CLUSTER_COUNT est modifié de telle sorte que cinq clusters au maximum existent dans ce nouveau modèle. En revanche, le modèle existant utilise la valeur par défaut 10.  
  
```  
SELECT * INTO [New_Clustering]  
USING [Microsoft_Clustering] (CLUSTER_COUNT = 5)   
FROM [TM Clustering]  
```  
  
## <a name="example-2-adding-a-filter-to-the-model"></a>Exemple 2 : ajout d'un filtre au modèle  
 L'exemple suivant crée un modèle d'exploration de données basé sur un modèle existant et ajoute un filtre. Le filtre restreint les données d'apprentissage uniquement aux clients qui habitent dans une région donnée.  
  
```  
SELECT * INTO [Clustering Europe Region]  
USING [Microsoft_Clustering] WITH FILTER(Region='Europe')  
FROM [TM Clustering]  
```  
  
> [!NOTE]  
>  Les filtres appliqués à la table de cas peuvent être modifiés à l'aide de l'instruction SELECT INTO, tel qu'indiqué dans cet exemple ; toutefois, si le modèle d'origine contient un filtre sur une table imbriquée, ce filtre ne peut pas être modifié ou supprimé en utilisant cette syntaxe, mais il est copié tel quel à partir du modèle d'origine. Pour créer un modèle avec un autre filtre sur une table imbriquée, utilisez la syntaxe ALTER STRUCTURE...ADD MODEL.  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; instructions de définition de données](../dmx/dmx-statements-data-definition.md)   
 [Data Mining Extensions &#40;DMX&#41; instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
