---
title: SELECT DISTINCT FROM &lt;modèle &gt; (DMX) | Documents Microsoft
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
- DISTINCT
- SELECT
dev_langs:
- DMX
helpviewer_keywords:
- discrete columns [DMX]
- discretized columns [DMX]
- SELECT DISTINCT FROM <model> statement
- continuous columns
ms.assetid: 0ab44ef6-1c3b-4809-a687-4d5d13f343af
caps.latest.revision: 44
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 5754cbeb07789b0d5f7a3f51386f108633cda83e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="select-distinct-from-ltmodel-gt-dmx"></a>SELECT DISTINCT FROM &lt;modèle &gt; (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne tous les états possibles de la colonne sélectionnée dans le modèle. Les valeurs qui sont retournées varient selon que la colonne spécifiée contient des valeurs discrètes, des valeurs numériques discrétisées ou des valeurs numériques continues.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SELECT [FLATTENED] DISTINCT [TOP <n>] <expression list> FROM <model>   
[WHERE <condition list>][ORDER BY <expression>]  
```  
  
## <a name="arguments"></a>Arguments  
 *n*  
 Ce paramètre est facultatif. Entier spécifiant le nombre de lignes à retourner.  
  
 *liste d’expressions*  
 Liste séparée par des virgules des identificateurs des colonnes associées (dérivées du modèle) ou expressions.  
  
 *model*  
 Identificateur du modèle  
  
 *liste des conditions*  
 Condition pour restreindre les valeurs retournées de la liste des colonnes.  
  
 *expression*  
 Ce paramètre est facultatif. Expression qui retourne une valeur scalaire.  
  
## <a name="remarks"></a>Notes  
 Le **SELECT DISTINCT FROM** instruction fonctionne uniquement avec une seule colonne ou un ensemble de colonnes associées. Cette clause ne fonctionne pas avec un ensemble de colonnes non associées.  
  
 Le **SELECT DISTINCT FROM** instruction vous permet de référencer directement une colonne à l’intérieur d’une table imbriquée. Par exemple :  
  
```  
<model>.<table column reference>.<column reference>  
```  
  
 Les résultats de la **SELECT DISTINCT FROM \<modèle >** instruction varient, selon le type de colonne. Le tableau ci-dessous décrit les types de colonnes pris en charge et le résultat de l'instruction.  
  
|Type de colonne|Sortie|  
|-----------------|------------|  
|Discret|Valeurs uniques de la colonne|  
|Discrétisé|Point milieu de chaque compartiment discrétisé de la colonne.|  
|Continu|Point milieu des valeurs de la colonne|  
  
## <a name="discrete-column-example"></a>Exemple de colonne discrète  
 L’exemple de code suivant est basé sur le `[TM Decision Tree]` modèle que vous créez dans le [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). La requête retourne les valeurs uniques qui existent dans la colonne discrète `Gender`.  
  
```  
SELECT DISTINCT [Gender]  
FROM [TM Decision Tree]  
```  
  
 Résultats de l'exemple :  
  
|Gender|  
|------------|  
||  
|F|  
|M|  
  
 Pour les colonnes qui contiennent des valeurs discrètes, les résultats incluent systématiquement l'état Missing, présenté comme une valeur Null.  
  
## <a name="continuous-column-example"></a>Exemple de colonne continue  
 L'exemple de code suivant retourne le point milieu, l'âge minimal et l'âge maximal de toutes les valeurs de la colonne.  
  
```  
SELECT DISTINCT [Age] AS [Midpoint Age],   
    RangeMin([Age]) AS [Minimum Age],   
    RangeMax([Age]) AS [Maximum Age]  
FROM [TM Decision Tree]  
```  
  
 Résultats de l'exemple :  
  
|Midpoint Age|Minimum Age|Maximum Age|  
|------------------|-----------------|-----------------|  
||||  
|62|26|97|  
  
 La requête retourne également une ligne unique de valeurs Null, pour représenter les valeurs manquantes.  
  
## <a name="discretized-column-example"></a>Exemple de colonne discrétisée.  
 L'exemple de code suivant retourne le point milieu et les valeurs maximales et minimales de chaque compartiment créé par l'algorithme pour la colonne, [`Yearly Income]`. Pour reproduire les résultats de cet exemple, vous devez créer une nouvelle structure d'exploration de données identique à `[Targeted Mailing]`. Dans l’Assistant, modifiez le type de contenu de la `Yearly Income` colonne à partir de **continu** à **Discretized**.  
  
> [!NOTE]  
>  Vous pouvez également modifier le modèle d'exploration de données créé dans le Didacticiel sur l'exploration de données de base afin de discrétiser la colonne de structure d'exploration de données, [`Yearly Income]`. Pour plus d’informations sur la procédure à suivre, consultez [modifier la discrétisation d’une colonne dans un modèle d’exploration de données](../analysis-services/data-mining/change-the-discretization-of-a-column-in-a-mining-model.md). Toutefois, le fait de modifier le discrétisation de la colonne force le retraitement de la structure d'exploration de données et modifie les résultats des autres modèles créés à l'aide de cette structure.  
  
```  
SELECT DISTINCT [Yearly Income] AS [Bucket Average],   
    RangeMin([Yearly Income]) AS [Bucket Minimum],   
    RangeMax([Yearly Income]) AS [Bucket Maximum]  
FROM [TM Decision Tree]  
```  
  
 Résultats de l'exemple :  
  
|Bucket Average|Bucket Minimum|Bucket Maximum|  
|--------------------|--------------------|--------------------|  
||||  
|24610.7|10000|39221.41|  
|55115.73|39221.41|71010.05|  
|84821.54|71010.05|98633.04|  
|111633.9|98633.04|124634.7|  
|147317.4|124634.7|170000|  
  
 Vous constatez que les valeurs de la colonne [Yearly Income] ont été discrétisées dans cinq compartiments, plus une ligne supplémentaire de valeurs Null pour représenter les valeurs manquantes.  
  
 Le nombre de décimales dans les résultats dépend du client utilisé pour l'interrogation. Dans cet exemple, ils ont été arrondis à deux décimales, à la fois pour des raisons de simplicité et pour refléter les valeurs affichées dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
 Par exemple, si vous parcourez le modèle en utilisant la Visionneuse d'arbre de décision et que vous cliquez sur un nœud qui contient des clients regroupés par revenu, les propriétés de nœud suivantes s'affichent dans l'info-bulle :  
  
 Age >=69 AND Yearly Income < 39221.41  
  
> [!NOTE]  
>  La valeur minimale du compartiment minimal et la valeur maximale du compartiment maximal correspondent aux valeurs observées la plus élevée et la moins élevée. Les valeurs hors de cette plage observée sont supposées appartenir aux compartiments minimaux et maximaux.  
  
## <a name="see-also"></a>Voir aussi  
 [SÉLECTIONNEZ &AMP;#40;DMX&AMP;#41;](../dmx/select-dmx.md)   
 [Data Mining Extensions &#40;DMX&#41; instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
