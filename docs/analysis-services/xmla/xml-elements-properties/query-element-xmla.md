---
title: Query, élément (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 861f216ac263de32b9f2afc3e0fcd4e43b3dfb4a
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576171"
---
# <a name="query-element-xmla"></a>Élément Query (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient une requête dans le [requêtes](../../../analysis-services/xmla/xml-elements-properties/queries-element-xmla.md) collection utilisée par le [DesignAggregations](../../../analysis-services/xmla/xml-elements-commands/designaggregations-element-xmla.md) commande pendant l’optimisation de l’utilisation.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Queries>  
   ...  
   <Query>...</Query>  
   ...  
</Queries>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|String|  
|Valeur par défaut|None|  
|Cardinalité|0-1 : élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Requêtes](../../../analysis-services/xmla/xml-elements-properties/queries-element-xmla.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 La commande **DesignAggregations** prend en charge l'optimisation basée sur l'utilisation en incluant un ou plusieurs éléments **Query** dans la collection **Queries** de la commande. Chaque élément **Query** représente une requête d'objectif que le processus de conception utilise pour définir des agrégations qui visent les requêtes utilisées le plus fréquemment. Vous pouvez spécifier vos propres requêtes d’objectif, ou vous pouvez utiliser les informations stockées par une instance d’Analysis Services dans le journal des requêtes pour récupérer des informations sur les requêtes les plus fréquemment utilisées.  
  
 Si vous concevez des agrégations itérative, il vous suffit de passer des requêtes d’objectif dans la première **DesignAggregations** commande car le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance stocke ces requêtes d’objectif et les utilise pendant les **DesignAggregations** commandes. Une fois les requêtes d'objectif passées dans la première commande **DesignAggregations** d'un procédé itératif, toute commande **DesignAggregations** suivante qui contient des requêtes d'objectif dans la propriété **Queries** génère une erreur.  
  
 L'élément **Query** contient une valeur délimitée par des virgules qui renferme les arguments suivants :  
  
 *Frequency*,*Dataset*[,*Dataset*...]  
  
 *Fréquence*  
 Un facteur de pondération qui correspond au nombre de fois que la requête a été exécutée antérieurement. Si l'élément **Query** représente une nouvelle requête, la valeur *Frequency* représente le facteur de pondération utilisé par le processus de conception pour évaluer la requête. À mesure que la valeur de la fréquence s'accroît, le poids associé à la requête pendant le processus de conception augmente.  
  
 *Jeu de données*  
 Une chaîne numérique qui spécifie quels attributs d'une dimension doivent être inclus dans la requête. Cette chaîne doit comporter autant de caractères que la dimension compte d'attributs. Zéro (0) indique que l'attribut à la position ordinale spécifiée n'est pas inclus dans la requête pour la dimension spécifiée, alors qu'un (1) indique que l'attribut à la position ordinale spécifiée est inclus dans la requête pour la dimension spécifiée.  
  
 Par exemple, la chaîne  « 011 » se rapporte à une requête concernant une dimension à trois attributs, dont le deuxième et le troisième sont inclus dans la requête.  
  
> [!NOTE]  
>  Certains attributs, qualifiés d'exclus, ne sont pas pris en compte dans le dataset. Pour plus d’informations sur les attributs exclus, consultez [propriétés (XMLA)](../../../analysis-services/xmla/xml-elements-properties/query-element-xmla.md).  
  
 Chaque dimension dans le groupe de mesures qui contient la conception d'agrégation est représentée par une valeur *Dataset* dans l'élément **Query** . L'ordre des valeurs *Dataset* doit correspondre à celui des dimensions incluses dans le groupe de mesures.  
  
## <a name="see-also"></a>Voir aussi
 [Conception d’agrégations &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/designing-aggregations-xmla.md)   
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
