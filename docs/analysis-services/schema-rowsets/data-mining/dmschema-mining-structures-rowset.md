---
title: Ensemble de lignes DMSCHEMA_MINING_STRUCTURES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0aac1d7fb0d8c54ae18cd7b45f5f414a02eafffe
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingstructures-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_STRUCTURES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations sur les structures d'exploration de données contenues dans le catalogue actuel.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DMSCHEMA_MINING_STRUCTURES** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**STRUCTURE_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue.|  
|**STRUCTURE_SCHEMA**|**DBTYPE_WSTR**||Nom de schéma non qualifié. **NULL** si les schémas ne sont pas pris en charge par le fournisseur.|  
|**NOM_STRUCTURE**|**DBTYPE_WSTR**||Nom de la structure. Cette colonne ne peut pas contenir de valeur **NULL**.|  
|**STRUCTURE_GUID**|**DBTYPE_GUID**||GUID qui identifie la structure de manière unique. **NULL** si les ID de propriété ne sont pas pris en charge par le fournisseur.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Description concise de la structure. **NULL** si aucune description n'est associée à la structure.|  
|**STRUCTURE_PROPID**|**DBTYPE_UI4**||ID de propriété de la structure. **NULL** si les ID de propriété ne sont pas pris en charge par le fournisseur.|  
|**DATE_DE_CRÉATION**|**DBTYPE_DBTIMESTAMP**||Date de création de la structure. **NULL** si cette valeur n'est pas disponible auprès du fournisseur.|  
|**DATE_DE_MODIFICATION**|**DBTYPE_DBTIMESTAMP**||Date de dernière modification de la structure. **NULL** si cette valeur n'est pas disponible auprès du fournisseur.|  
|**CREATION_STATEMENT**|**DBTYPE_WSTR**||(Facultatif) Instruction utilisée pour créer le modèle d'exploration de données d'origine.|  
|**IS_POPULATED**|**DBTYPE_BOOL**||Valeur booléenne qui indique si la structure est remplie.<br /><br /> **VARIANT_TRUE** si la structure est remplie ; **VARIANT_FALSE** dans le cas contraire.|  
|**LAST_PROCESSED**|**DBTYPE_DBTIMESTAMP**||Date du dernier traitement de la structure. **NULL** si cette valeur n'est pas disponible auprès du fournisseur.|  
|**HOLDOUT_MAXPERCENT**|**DBTYPE_ UI1**||Valeur spécifiée par l'utilisateur qui indique le pourcentage maximal de cas d'entrée exclus en tant que jeu de test.<br /><br /> 0 ou **NULL** indique qu'il n'existe aucune limite.|  
|**HOLDOUT_MAXCASES**|**DBTYPE_UI8**||Valeur spécifiée par l'utilisateur qui indique le nombre maximal de cas d'entrée exclus en tant que jeu de test.<br /><br /> 0 ou **NULL** indique qu'il n'existe aucune limite.|  
|**HOLDOUT_SEED**|**DBTYPE_UI8**||Valeur spécifiée par l'utilisateur qui est employée comme valeur de départ pour le partitionnement répété.<br /><br /> La valeur 0 indique qu'un hachage de l'ID de la structure d'exploration de données est employé comme valeur de départ.|  
|**HOLDOUT_ACTUAL_SIZE**|**DBTYPE_UI8**||Si la structure d'exploration de données est traitée, cette valeur indique la taille réelle du jeu de données de test, exprimée en nombre de cas.<br /><br /> **NULL** indique que la structure d'exploration de données n'est pas traitée.|  
  
 L'ensemble de lignes est trié selon **STRUCTURE_CATALOG**, **STRUCTURE_SCHEMA**, **STRUCTURE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DMSCHEMA_MINING_STRUCTURES** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**STRUCTURE_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**STRUCTURE_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_STRUCTURE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
