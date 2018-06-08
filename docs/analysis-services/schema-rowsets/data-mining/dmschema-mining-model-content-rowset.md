---
title: Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1b1fec6efb8ec638c54226ffa9d2ef335beb19f0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingmodelcontent-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Permet à l'application cliente de parcourir le contenu d'un modèle d'exploration de données. Les applications clientes peuvent utiliser les restrictions d'opérations d'arborescence spéciales décrites à la fin de cette rubrique pour accéder au contenu du modèle d'exploration de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DMSCHEMA_MINING_MODEL_CONTENT** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] remplit cette colonne avec le nom de la base de données dont le modèle est un membre.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**||Nom de schéma non qualifié. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **VT_NULL**.|  
|**MODEL_NAME**|**DBTYPE_WSTR**||Nom du modèle auquel le contenu décrit par cette ligne est associé.|  
|**ATTRIBUTE_NAME**|**DBTYPE_WSTR**||Noms des attributs qui correspondent à ce nœud.|  
|**NOM_NŒUD**|**DBTYPE_WSTR**||Nom du nœud. Actuellement, cette colonne contient la même valeur que **NODE_UNIQUE_NAME**, bien que cela puisse changer dans les futures versions.|  
|**NODE_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du nœud.|  
|**NODE_TYPE**|**DBTYPE_I4**||Type de nœud. Génère l'une des valeurs suivantes (les algorithmes d'exploration de données tiers peuvent étendre cette liste) :<br /><br /> **DM_NODE_TYPE_CLASSIFICATION_TREE_ROOT** (**2**)<br /><br /> **DM_NODE_TYPE_TREE_INTERIOR** (**3**)<br /><br /> **DM_NODE_TYPE_TREE_DISTRIBUTION** (**4**)<br /><br /> **DM_NODE_TYPE_CLUSTER** (**5**)<br /><br /> **DM_NODE_TYPE_UNKNOWN** (**6**)<br /><br /> **DM_NODE_TYPE_ITEMSET** (**7**)<br /><br /> **DM_NODE_TYPE_ASSOCIATION_RULE** (**8**)<br /><br /> **DM_NODE_TYPE_NB_PREDICTABLE_ATTRIBUTE** (**9**)<br /><br /> **DM_NODE_TYPE_NB_INPUT_ATTRIBUTE** (**10**)<br /><br /> **DM_NODE_TYPE_NB_INPUT_ATTRIBUTE_STATE** (**11**)<br /><br /> **DM_NODE_TYPE_SEQUENCE** (**13**)<br /><br /> **DM_NODE_TYPE_TRANSITION** (**14**)<br /><br /> **DM_NODE_TYPE_TIME_SERIES** (**15**)<br /><br /> **DM_NODE_TYPE_TS_TREE** (**16**)<br /><br /> **DM_NODE_TYPE_NN_SUBNETWORK** (**17**) réseau neuronal, le sous-réseau<br /><br /> **DM_NODE_TYPE_NN_INPUT_LAYER** (**18**) réseau neuronal, la couche d’entrée (parent de nœuds d’entrée)<br /><br /> **DM_NODE_TYPE_NN_HIDDEN_LAYER** (**19**) réseau neuronal, couche masquée (parent de nœuds masqués)<br /><br /> **DM_NODE_TYPE_NN_OUTPUT_LAYER** (**20**) réseau neuronal, couche de sortie (parent de nœuds de sortie)<br /><br /> **DM_NODE_TYPE_NN_INPUT_NODE** (**21**) réseau neuronal, le nœud d’entrée<br /><br /> **DM_NODE_TYPE_NN_HIDDEN_NODE** (**22**) réseau neuronal, le nœud masqué<br /><br /> **DM_NODE_TYPE_NN_OUTPUT_NODE** (**23**) réseau neuronal, le nœud de sortie<br /><br /> **DM_NODE_TYPE_NN_MARGINAL_STAT_NODE** (**24**) réseau neuronal, le nœud des statistiques marginales<br /><br /> **DM_NODE_TYPE_REGRESSION_TREE_ROOT** (**25**)<br /><br /> **DM_NODE_TYPE_NB_MARGINAL_STAT_NODE** (**26**) réseau neuronal, le nœud des statistiques marginales|  
|**NODE_GUID**|**DBTYPE_GUID**||GUID du nœud. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**NODE_CAPTION**|**DBTYPE_WSTR**||Étiquette ou légende associée au nœud. Cette propriété est principalement utilisée à des fins d'affichage.|  
|**CHILDREN_CARDINALITY**|**DBTYPE_UI4**||Estimation du nombre d'enfants du nœud.|  
|**PARENT_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du parent du nœud. **NULL** est retournée pour tous les nœuds au niveau racine.|  
|**NODE_DESCRIPTION**|**DBTYPE_WSTR**||Description conviviale du nœud.|  
|**NODE_RULE**|**DBTYPE_WSTR**||Description XML de la règle incorporée dans le nœud.|  
|**MARGINAL_RULE**|**DBTYPE_WSTR**||Description XML de la règle se déplaçant vers le nœud à partir du nœud parent.|  
|**NODE_PROBABILITY**|**DBTYPE_R8**||Probabilité associée à ce nœud.|  
|**MARGINAL_PROBABILITY**|**DBTYPE_R8**||Probabilité d'accès au nœud à partir du nœud parent.|  
|**NODE_DISTRIBUTION**|**DBTYPE_HCHAPTER**||Table qui contient l'histogramme de probabilité du nœud.|  
|**NODE_SUPPORT**|**DBTYPE_R8**||Nombre de cas qui prennent en charge ce nœud.|  
|**MSOLAP_MODEL_COLUMN**|**DBTYPE_WSTR**||Nom de la colonne de la définition du modèle à laquelle ce nœud se rapporte.|  
|**MSOLAP_NODE_SCORE**|**DBTYPE_R8**||Score qui a été calculé pour ce nœud.|  
|**MSOLAP_NODE_SHORT_CAPTION**|**DBTYPE_WSTR**||Courte légende du nœud pouvant être utilisée à des fins d'affichage pour améliorer la lisibilité.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DMSCHEMA_MINING_MODEL_CONTENT** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**ATTRIBUTE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_NŒUD**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NODE_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NODE_TYPE**|**DBTYPE_I4**|Ce paramètre est facultatif.|  
|**NODE_GUID**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NODE_CAPTION**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**TREE_OPERATION**|**DBTYPE_UI4**|Ce paramètre est facultatif. (voir les remarques supplémentaires ci-dessous)|  
  
 La restriction, **TREE_OPERATION**, n’est pas sur une colonne spécifique de la **DMSCHEMA_MINING_MODEL_CONTENT** rowset ; au lieu de cela, il spécifie un opérateur d’arborescence. Le consommateur peut spécifier un **NODE_UNIQUE_NAME** l’opérateur d’arborescence et de restriction (**ancêtres**, **enfants**, **frères**, **PARENT**, **DESCENDANTS**, **SELF**) pour obtenir le jeu de membres demandé. Le **SELF** opérateur inclut la ligne pour le nœud lui-même dans la liste des lignes retournées. Le tableau suivant décrit les constantes qui composent la définition de bitmap de la **TREE_OPERATION** restriction. Elles peuvent être combinées à l’aide de la logique **ou** opérateur.  
  
|Constante|Valeur|  
|--------------|-----------|  
|**DMTREEOP_ANCESTORS**|**0 x 00000020**|  
|**DMTREEOP_CHILDREN**|**0 x 00000001**|  
|**DMTREEOP_SIBLINGS**|**0 x 00000002**|  
|**DMTREEOP_PARENT**|**0 x 00000004**|  
|**DMTREEOP_SELF**|**0 x 00000008**|  
|**DMTREEOP_DESCENDANTS**|**0 x 00000010**|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
