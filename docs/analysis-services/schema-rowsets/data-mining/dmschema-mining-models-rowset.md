---
title: Ensemble de lignes DMSCHEMA_MINING_MODELS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 708a1b58f99e7257369351d82d54f45a863c0ca1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingmodels-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_MODELS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Énumère les modèles d'exploration de données du catalogue actuel. Le **DMSCHEMA_MINING_MODELS** ensemble de lignes inclut des informations telles que les noms de modèle, date de traitement et l’algorithme d’exploration de données associée à chaque modèle d’exploration de données.  
  
 . Le **DMSCHEMA_MINING_MODELS** de lignes du schéma est très similaire à la [DBSCHEMA_TABLES](../../../analysis-services/schema-rowsets/ole-db/dbschema-tables-rowset.md) de lignes du schéma et peut être utilisé de la même façon.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DMSCHEMA_MINING_MODELS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**|Nom du catalogue. Défini d'après le nom de la base de données dont le modèle est membre.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**|Nom de schéma non qualifié. Cette colonne n’est pas pris en charge par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**MODEL_NAME**|**DBTYPE_WSTR**|Le nom du modèle d’exploration de données. Cette colonne contient le nom du modèle d'exploration de données ; elle n'est jamais vide.|  
|**MODEL_TYPE**|**DBTYPE_WSTR**|Type de modèle.|  
|**MODEL_GUID**|**DBTYPE_GUID**|GUID du modèle.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description conviviale du modèle.|  
|**MODEL_PROPID**|**DBTYPE_UI4**|ID de propriété du modèle. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**DATE_DE_CRÉATION**|**DBTYPE_DBTIMESTAMP**|Date de création du modèle.|  
|**DATE_DE_MODIFICATION**|**DBTYPE_DBTIMESTAMP**|Date de la dernière modification de la définition du modèle.|  
|**SERVICE_TYPE_ID**|**DBTYPE_UI4**|Énumération qui identifie le type d'algorithme d'exploration de données utilisé par le modèle. Les valeurs possibles pour ce type sont les suivantes :<br /><br /> **DM_SERVICETYPE_CLASSIFICATION** (1)<br /><br /> **DM_SERVICETYPE_SEGMENTATION**(2)<br /><br /> **ASSOCIATION DE DM_SERVICETYPE_**(4)<br /><br /> **DM_SERVICETYPE_ DENSITY_ESTIMATE**(8)<br /><br /> **DM_SERVICETYPE_SEQUENCE**(16)|  
|**SERVICE_NAME**|**DBTYPE_WSTR**|Nom spécifique au fournisseur de l'algorithme d'exploration de données utilisé par le modèle.|  
|**CREATION_STATEMENT**|**DBTYPE_WSTR**|Instruction utilisée pour créer le modèle d'exploration de données.|  
|**PREDICTION_ENTITY**|**DBTYPE_WSTR**|Liste délimitée par des virgules indiquant les colonnes d'exploration de données pouvant être prédites.|  
|**IS_POPULATED**|**DBTYPE_BOOL**|Indicateur booléen qui indique si le modèle est rempli.<br /><br /> **TRUE** si le modèle est renseigné ; sinon, **FALSE**.|  
|**MINING_PARAMETERS**|**DBTYPE_WSTR**|Liste des paramètres qui ont été utilisés lors de la création du modèle, séparés par des virgules.|  
|**MINING_STRUCTURE**|**DBTYPE_WSTR**|ID de la structure d'exploration de données sur laquelle repose le modèle.|  
|**LAST_PROCESSED**|**DBTYPE_DBTIMESTAMP**|Date du dernier traitement du modèle.|  
|**MSOLAP_IS_DRILLTHROUGH_ENABLED**|**DBTYPE_BOOL**|Indicateur booléen qui indique si le modèle prend en charge l'extraction.|  
|**FILTRE**|**DBTYPE_WSTR**|Expression de filtre qui est associée au modèle d'exploration de données.<br /><br /> Une valeur NULL ou une chaîne vide indique qu'aucun filtre n'est appliqué.|  
|**TRAINING_SET_SIZE**|**DBTYPE_UIS**|Nombre de cas contenus dans le jeu d'apprentissage du modèle d'exploration de données une fois que la structure a été traitée et une fois que les filtres ont été appliqués au modèle.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DMSCHEMA_MINING_MODELS** ensemble de lignes peut être restreint sur les colonnes dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_TYPE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SERVICE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SERVICE_TYPE_ID**|**DBTYPE_UI4**|Ce paramètre est facultatif.|  
|**MINING_STRUCTURE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
 Pour obtenir des exemples d’interrogation de cet ensemble de lignes, consultez [les paramètres utilisés pour créer un modèle d’exploration de données de requête](../../../analysis-services/data-mining/query-the-parameters-used-to-create-a-mining-model.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
