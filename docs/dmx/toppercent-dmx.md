---
title: TopPercent (DMX) | Documents Microsoft
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
- TOPPERCENT
dev_langs:
- DMX
helpviewer_keywords:
- TopPercent function
ms.assetid: 0b407ab2-2a69-4cbd-ae13-bdd29654fa86
caps.latest.revision: 37
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: bccca2b1a3411b8c5ee36d6dbf7a972fd67a9e83
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="toppercent-dmx"></a>TopPercent (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Le **TopPercent** fonction retourne, dans l’ordre décroissant, les lignes les plus haut d’une table dont le total cumulé est au moins un pourcentage spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TopPercent(<table expression>, <rank expression>, <percent>)  
```  
  
## <a name="applies-to"></a>S'applique à  
 Une expression qui retourne une table, comme un \<référence de colonne de table >, ou une fonction qui retourne une table.  
  
## <a name="return-type"></a>Type de retour  
 \<Expression de table >  
  
## <a name="remarks"></a>Notes  
 Le **TopPercent** fonction retourne les lignes en ordre décroissant d’en fonction de la valeur évaluée de la \<rank expression > argument pour chaque ligne, telles que la somme de la \<rank expression > valeurs soit au moins égale au pourcentage spécifié par le \<% > argument. **TopPercent** retourne le plus petit nombre d’éléments possible tout en correspondant à la valeur du pourcentage spécifiée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une requête de prédiction sur le modèle d’Association que vous créez à l’aide de la [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c).  
  
 Pour comprendre le fonctionnement de TopPercent, il peut être utile d’abord exécuter une requête de prédiction qui retourne uniquement la table imbriquée.  
  
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
  
 La fonction TopPercent prend les résultats de cette requête et retourne les lignes avec la valeur la plus élevée à cette somme au pourcentage spécifié.  
  
```  
SELECT   
TopPercent  
    (  
    Predict ([Association].[v Assoc Seq Line Items],INCLUDE_STATISTICS,10),  
    $SUPPORT,  
    50)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Le premier argument à la fonction TopPercent est le nom d’une colonne de table. Dans cet exemple, la table imbriquée est retournée en appelant la fonction de prédiction et à l’aide de l’argument INCLUDE_STATISTICS.  
  
 Le deuxième argument à la fonction TopPercent est la colonne dans la table imbriquée qui vous permettent de classer les résultats. Dans cet exemple, l'option INCLUDE_STATISTICS retourne les colonnes $SUPPORT, $PROBABILTY et $ADJUSTED PROBABILITY. Cet exemple utilise $SUPPORT car les valeurs de support ne sont pas fractionnaires et sont donc plus faciles à vérifier.  
  
 Le troisième argument de la fonction TopPercent Spécifie le pourcentage, en tant que double. Pour obtenir les lignes des produits principaux dont la somme est égale à 50 pour cent du support total, tapez 50.  
  
 Résultats de l'exemple :  
  
|Modèle|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0.29…|0.25…|  
|Water Bottle|2866|0.19…|0.17…|  
|Patch kit|2113|0.14…|0.13…|  
|Mountain Tire Tube|1992|0.133…|0.12…|  
  
 **Remarque** cet exemple est fourni uniquement pour illustrer l’utilisation de TopPercent. L'exécution de cette requête peut prendre beaucoup de temps, en fonction de la taille de votre jeu de données.  
  
> [!WARNING]  
>  Les fonctions MDX pour TOPPERCENT et BOTTOMPERCENT peuvent générer des résultats inattendus lorsque les valeurs utilisées pour calculer le pourcentage incluent les nombres négatifs. Ce comportement n'affecte pas les fonctions DMX. Pour plus d’informations, consultez [BottomPercent &#40;MDX&#41;](../mdx/bottompercent-mdx.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
