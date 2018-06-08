---
title: Ensemble de lignes DMSCHEMA_MINING_SERVICES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0cf94fa4fb22e38ac52513d2dba52500df3bb9d5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingservices-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_SERVICES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit la description de chaque algorithme d'exploration de données que le fournisseur prend en charge.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DMSCHEMA_MINING_SERVICES** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**SERVICE_NAME**|**DBTYPE_WSTR**|Nom de l'algorithme. Cette colonne est spécifique au fournisseur.|  
|**SERVICE_TYPE_ID**|**DBTYPE_UI4**|Cette colonne contient une bitmap qui décrit le service d'exploration de données. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] remplit cette colonne avec l’une des valeurs suivantes :<br /><br /> **DM_SERVICETYPE_CLASSIFICATION** (**1**)<br /><br /> **DM_SERVICETYPE_CLUSTERING** (**2**)|  
|**SERVICE_DISPLAY_NAME**|**DBTYPE_WSTR**|Nom complet localisable de l'algorithme.|  
|**SERVICE_GUID**|**DBTYPE_GUID**|GUID de l'algorithme.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description conviviale de l'algorithme.|  
|**PREDICTION_LIMIT**|**DBTYPE_UI4**|Nombre maximal de prédictions que le modèle et l'algorithme peuvent fournir.|  
|**SUPPORTED_DISTRIBUTION_FLAGS**|**DBTYPE_WSTR**|Liste d'indicateurs, délimitée par des virgules, qui décrivent les distributions statistiques prises en charge par l'algorithme. Cette colonne contient une ou plusieurs des valeurs suivantes :<br /><br /> «**NORMAL**»<br /><br /> «**JOURNAL NORMAL**»<br /><br /> «**UNIFORME**»|  
|**SUPPORTED_INPUT_CONTENT_TYPES**|**DBTYPE_WSTR**|Liste d'indicateurs, délimitée par des virgules, qui décrivent les types de contenu d'entrée pris en charge par l'algorithme. Cette colonne contient une ou plusieurs des valeurs suivantes :<br /><br /> «**CLÉ**»<br /><br /> «**DISCRÈTE**»<br /><br /> «**CONTINU**»<br /><br /> «**DISCRETIZED**»<br /><br /> «**COMMANDÉ**»<br /><br /> « CLÉ **SÉQUENCE**»<br /><br /> «**CYCLIQUE**»<br /><br /> «**PROBABILITÉ**»<br /><br /> «**VARIANCE**»<br /><br /> «**STDEV**»<br /><br /> «**PRISE EN CHARGE**»<br /><br /> «**VARIANCE DE PROBABILITÉ**»<br /><br /> «**PROBABILITÉ STDEV**»<br /><br /> «**TEMPS CLÉ**»|  
|**SUPPORTED_PREDICTION_CONTENT_TYPES**|**DBTYPE_WSTR**|Liste d'indicateurs, délimitée par des virgules, qui décrivent les types de contenu de prédiction pris en charge par l'algorithme. Cette colonne contient une ou plusieurs des valeurs suivantes :<br /><br /> «**CLÉ**»<br /><br /> «**DISCRÈTE**»<br /><br /> «**CONTINU**»<br /><br /> «**DISCRETIZED**»<br /><br /> «**COMMANDÉ**»<br /><br /> « CLÉ **SÉQUENCE** »<br /><br /> «**CYCLIQUE**»<br /><br /> «**PROBABILITÉ**»<br /><br /> «**VARIANCE**»<br /><br /> «**STDEV**»<br /><br /> «**PRISE EN CHARGE**»<br /><br /> «**VARIANCE DE PROBABILITÉ**»<br /><br /> «**PROBABILITÉ STDEV**»<br /><br /> "KEY TIME"|  
|**SUPPORTED_MODELING_FLAGS**|**DBTYPE_WSTR**|Liste, délimitée par des virgules, des indicateurs de modélisation pris en charge par l'algorithme. Cette colonne contient une ou plusieurs des valeurs suivantes :<br /><br /> «**MODEL_EXISTENCE_ONLY**»<br /><br /> «**RÉGRESSEUR**»<br /><br /> <br /><br /> Notez que les indicateurs spécifiques au fournisseur peuvent également être définies.|  
|**SUPPORTED_SOURCE_QUERY**|**DBTYPE_WSTR**|Cette colonne est prise en charge à des fins de compatibilité descendante.|  
|**TRAINING_COMPLEXITY**|**DBTYPE_I4**|Durée prévue de l'apprentissage :<br /><br /> **DM_TRAINING_COMPLEXITY_LOW** indique que le temps d’exécution est relativement court, et il est proportionnel à l’entrée.<br /><br /> **DM_TRAINING_COMPLEXITY_MEDIUM** indique que le temps d’exécution peuvent être long, mais il est généralement proportionnel à l’entrée.<br /><br /> **DM_TRAINING_COMPLEXITY_HIGH** indique que le temps d’exécution est long, et il peut croître de manière exponentielle de la relation pour le nombre de cas d’apprentissage.|  
|**PREDICTION_COMPLEXITY**|**DBTYPE_I4**|Durée prévue de la prédiction :<br /><br /> **DM_PREDICTION_COMPLEXITY_LOW** indique que le temps d’exécution est relativement court, et il est proportionnel à l’entrée.<br /><br /> **DM_PREDICTION_COMPLEXITY_MEDIUM** indique que le temps d’exécution peuvent être long, mais il est généralement proportionnel à l’entrée.<br /><br /> **DM_PREDICTION_COMPLEXITY_HIGH** indique que le temps d’exécution est long, et il peut croître de manière exponentielle de la relation pour le nombre de cas d’apprentissage.|  
|**EXPECTED_QUALITY**|**DBTYPE_I4**|Qualité attendue du modèle produit avec cet algorithme :<br /><br /> **DM_EXPECTED_QUALITY_LOW**<br /><br /> **DM_EXPECTED_QUALITY_MEDIUM**<br /><br /> **DM_EXPECTED_QUALITY_HIGH**|  
|**MISE À L’ÉCHELLE**|**DBTYPE_I4**|Évolutivité de l'algorithme :<br /><br /> **DM_SCALING_LOW**<br /><br /> **DM_SCALING_MEDIUM**<br /><br /> **DM_SCALING_HIGH**|  
|**ALLOW_INCREMENTAL_INSERT**|**DBTYPE_BOOL**|Valeur booléenne qui indique si l'algorithme prend en charge l'apprentissage incrémentiel, c'est-à-dire la mise à jour des modèles découverts selon de nouvelles données factuelles, au lieu de la redécouverte complète des modèles.|  
|**ALLOW_PMML_INITIALIZATION**|**DBTYPE_BOOL**|Valeur booléenne qui indique s'il est possible de créer des modèles d'exploration de données en fonction d'une chaîne PMML 2.1.<br /><br /> Si **TRUE**, l’algorithme d’exploration de données prend en charge l’initialisation à partir du contenu PMML 2.1.|  
|**CONTRÔLE**|**DBTYPE_I4**|Prise en charge fournie par le service si l'apprentissage est interrompu :<br /><br /> **DM_CONTROL_NONE** indique que l’algorithme ne peut pas être annulé après son démarrage à l’apprentissage du modèle.<br /><br /> **DM_CONTROL_CANCEL** indique que l’algorithme peut être annulé une fois qu’il démarre à l’apprentissage du modèle, mais doit être redémarré pour reprendre l’apprentissage.<br /><br /> **DM_CONTROL_SUSPENDRESUME** indique que l’algorithme peut être annulé et reprendre à tout moment, mais résultats ne sont pas disponibles tant que la configuration est terminée.<br /><br /> **DM_CONTROL_SUSPENDWITHRESULT** indique que l’algorithme peut être annulé et reprendre à tout moment, et que des résultats incrémentiels peuvent être obtenues.|  
|**ALLOW_DUPLICATE_KEY**|**DBTYPE_BOOL**|Valeur booléenne qui indique si les cas peuvent contenir des clés dupliquées.<br /><br /> Si **VARIANT_TRUE**, cas sont autorisés à contenir des clés dupliquées.|  
|**VIEWER_TYPE**|**DBTYPE_WSTR**|Visionneuse recommandée pour ce modèle. |  
|**HELP_FILE**|**DBTYPE_WSTR**|(Facultatif) Nom du fichier qui contient la documentation de ce service.|  
|**HELP_CONTEXT**|**DBTYPE_I4**|(Facultatif) ID de contexte d'aide de ce service.|  
|**MSOLAP_SUPPORTS_ANALYSIS_SERVICES_DDL**|**DBTYPE_WSTR**|Version de la DDL prise en charge. 0 indique qu'aucune DDL n'est prise en charge.|  
|**MSOLAP_SUPPORTS_OLAP_MINING_MODELS**|**DBTYPE_BOOL**|Valeur booléenne qui indique s'il est possible de créer des modèles d'exploration de données OLAP.<br /><br /> Si **TRUE**, les modèles d’exploration de données OLAP peuvent être créés. Requiert **MSOLAP_SUPPORTS_ANALYSIS_SERVICES_DDL** à être différente de zéro.|  
|**MSOLAP_SUPPORTS_DATA_MINING_DIMENSIONS**|**DBTYPE_BOOL**|Valeur booléenne qui indique s'il est possible de créer des dimensions d'exploration de données.<br /><br /> Si **TRUE**, dimensions d’exploration de données peuvent être créées.|  
|**MSOLAP_SUPPORTS_DRILLTHROUGH**|**DBTYPE_BOOL**|Valeur booléenne qui indique si le service prend en charge les fonctionnalités d'extraction.<br /><br /> Si **TRUE**, le service prend en charge les fonctionnalités d’extraction.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DMSCHEMA_MINING_SERVICES** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**SERVICE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SERVICE_TYPE_ID**|**DBTYPE_UI4**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
