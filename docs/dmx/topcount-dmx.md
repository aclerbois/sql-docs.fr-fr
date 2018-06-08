---
title: TopCount (DMX) | Documents Microsoft
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
- TOPCOUNT
dev_langs:
- DMX
helpviewer_keywords:
- TopCount function
ms.assetid: cbe031c9-4ff0-45df-9810-ebaebacf161d
caps.latest.revision: 40
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 48a53d01219290dd50bd192a6ee2adf5b51ad7b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="topcount-dmx"></a>TopCount (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le nombre spécifié de lignes les plus hautes dans l'ordre décroissant, comme indiqué par une expression.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TopCount(<table expression>, <rank expression>, <count>)  
```  
  
## <a name="applies-to"></a>S'applique à  
 Une expression qui retourne une table, comme un \<référence de colonne de table >, ou une fonction qui retourne une table.  
  
## <a name="return-type"></a>Type de retour  
 \<Expression de table >  
  
## <a name="remarks"></a>Notes  
 La valeur fournie par le \<rank expression > argument détermine l’ordre décroissant de classement pour les lignes qui sont fournis dans le \<expression de table > argument et le nombre de lignes supérieur spécifié dans le \<count > argument est retourné.  
  
 La fonction TopCount a été introduite initialement pour activer les prédictions associatives et, en général, produit les mêmes résultats qu’une instruction qui inclut **SELECT TOP** et **ORDER BY** clauses. Vous obtiendrez de meilleures performances pour les prédictions associatives si vous utilisez la **prédire (DMX)** (fonction), qui prend en charge la spécification d’un nombre de prédictions à retourner.  
  
 Toutefois, il existe des situations où vous devrez peut-être encore utiliser TopCount. Par exemple, DMX ne prend pas en charge la **haut** qualificateur dans une instruction de sous-sélection. Le [PredictHistogram &#40;DMX&#41; ](../dmx/predicthistogram-dmx.md) fonction également ne gère pas l’ajout de **haut**.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent des requêtes de prédiction sur le modèle d’Association que vous créez à l’aide de la [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). Les requêtes retournent les mêmes résultats, mais le premier exemple utilise TopCount et le deuxième exemple utilise la fonction de prédiction.  
  
 Pour comprendre le fonctionnement de TopCount, il peut être utile d’abord exécuter une requête de prédiction qui retourne uniquement la table imbriquée.  
  
```  
SELECT Predict ([Association].[v Assoc Seq Line Items], INCLUDE_STATISTICS, 10)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
> [!NOTE]  
>  Dans cet exemple, la valeur fournie en tant qu'entrée contient un guillemet simple et doit donc être placée dans une séquence d'échappement en la préfaçant avec un autre guillemet simple. Si vous n'êtes pas certain de la syntaxe permettant d'insérer un caractère d'échappement, vous pouvez utiliser le Générateur de requêtes de prédiction pour créer la requête. Lorsque vous sélectionnez la valeur dans la liste déroulante, le caractère d'échappement requis est inséré pour vous. Pour plus d’informations, consultez [créer une requête Singleton dans le Concepteur d’exploration de données](../analysis-services/data-mining/create-a-singleton-query-in-the-data-mining-designer.md).  
  
 Résultats de l'exemple :  
  
|Modèle|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0.291283016|0.252695851|  
|Water Bottle|2866|0.192620472|0.175205052|  
|Patch kit|2113|0.142012232|0.132389356|  
|Mountain Tire Tube|1992|0.133879965|0.125304948|  
|Mountain-200|1755|0.117951475|0.111260823|  
|Road Tire Tube|1588|0.106727603|0.101229538|  
|Cycling Cap|1473|0.098998589|0.094256014|  
|Fender Set - Mountain|1415|0.095100477|0.090718432|  
|Mountain Bottle Cage|1367|0.091874454|0.087780332|  
|Road Bottle Cage|1195|0.080314537|0.077173962|  
  
 La fonction TopCount prend les résultats de cette requête et retourne le nombre spécifié de lignes plus petite valeur.  
  
```  
SELECT   
TopCount  
    (  
    Predict ([Association].[v Assoc Seq Line Items],INCLUDE_STATISTICS,10),  
    $SUPPORT,  
    3)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Le premier argument à la fonction TopCount est le nom d’une colonne de table. Dans cet exemple, la table imbriquée est retournée en appelant la fonction de prédiction et à l’aide de l’argument INCLUDE_STATISTICS.  
  
 Le deuxième argument à la fonction TopCount est la colonne dans la table imbriquée qui vous permettent de classer les résultats. Dans cet exemple, l'option INCLUDE_STATISTICS retourne les colonnes $SUPPORT, $PROBABILTY et $ADJUSTED PROBABILITY. Cet exemple utilise $SUPPORT pour classer les résultats.  
  
 Le troisième argument de la fonction TopCount indique le nombre de lignes à retourner, sous forme d’entier. Pour obtenir les trois produits supérieurs, tel que classé par $SUPPORT, tapez 3.  
  
 Résultats de l'exemple :  
  
|Modèle|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0.29…|0.25…|  
|Water Bottle|2866|0.19…|0.17…|  
|Patch kit|2113|0.14…|0.13…|  
  
 Toutefois, ce type de requête peut affecter les performances dans un environnement de production. Cela est dû au fait que la requête retourne un ensemble de toutes les prédictions de l'algorithme, trie ces prédictions et retourne les 3 prédictions supérieures.  
  
 L'exemple suivant fournit une autre instruction qui retourne les mêmes résultats mais s'exécute beaucoup plus rapidement. Cet exemple remplace TopCount par la fonction de prédiction, qui accepte un nombre de prédictions en tant qu’argument. Cet exemple utilise également la **$SUPPORT** mot clé pour récupérer directement la colonne de table imbriquée.  
  
```  
SELECT Predict ([Association].[v Assoc Seq Line Items], INCLUDE_STATISTICS, 3, $SUPPORT)  
```  
  
 Les résultats contiennent les 3 prédictions supérieures triées par la valeur de support. Vous pouvez remplacer $SUPPORT par $PROBABILITY ou $ADJUSTED_PROBABILITY pour retourner des prédictions classées par probabilité ou par probabilité ajustée. Pour plus d’informations, consultez **prédire (DMX)**.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [BottomCount &#40;DMX&#41;](../dmx/bottomcount-dmx.md)   
 [TopPercent &#40;DMX&#41;](../dmx/toppercent-dmx.md)   
 [TopSum &#40;DMX&#41;](../dmx/topsum-dmx.md)  
  
  
