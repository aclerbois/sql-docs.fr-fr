---
title: Ensemble de lignes DISCOVER_STORAGE_TABLES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b87759d736044febc89099d493fb357d7e5153b9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverstoragetables-rowset"></a>Ensemble de lignes DISCOVER_STORAGE_TABLES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Permet au client d'identifier les tables incluses dans une base de données Analysis Services qui fonctionne en mode tabulaire ou SharePoint.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_STORAGE_TABLES** contient les colonnes suivantes.  
  
|**Nom de colonne**|**Indicateur de type**|**Longueur**|**Description**|  
|---------------------|------------------------|----------------|---------------------|  
|**DATABASE_NAME**|**DBTYPE_WSTR**||Spécifie le nom de la base de données qui contient les tables.<br /><br /> L'ensemble de lignes **DISCOVER_STORAGE_TABLES** peut être restreint à l'aide de cette colonne. Si cette colonne n'est pas utilisée pour restreindre l'ensemble de lignes, la base de données actuelle est utilisée.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Spécifie le cube ou modèle qui contient les tables.<br /><br /> L'ensemble de lignes **DISCOVER_STORAGE_TABLES** peut être restreint à l'aide de cette colonne.|  
|**NOM_GROUPE_MESURES**|**DBTYPE_WSTR**||Nom du groupe de mesures.|  
|**NOM_PARTITION**|**DBTYPE_WSTR**||Nom de la partition.|  
|**DIMENSION_NAME**|**DBTYPE_WSTR**||Nom de la dimension.|  
|**TABLE_ID**|**DBTYPE_WSTR**||ID de la table utilisée pour stocker les attributs de table.|  
|**TABLE_PARTITIONS_COUNT**|**DBTYPE_ WSTR**||Nombre de partitions de la table.|  
|**HINT_TABLE_TYPE**|**DBTYPE_ WSTR**||Conseil pour le type de table.|  
|**ROWS_COUNT**|**DBTYPE_UI4**||Nombre de lignes dans la partition.|  
|**RIVIOLATION_COUNT**|**DBTYPE_UI4**||Nombre de lignes avec des violations d'intégrité référentielle.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_STORAGE_TABLES** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|**Nom de colonne**|**Indicateur de type**|**État de la restriction**|  
|---------------------|------------------------|---------------------------|  
|**DATABASE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_GROUPE_MESURES**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**NOM_PARTITION**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
  
## <a name="example"></a>Exemple  
 L'exemple de code suivant retourne une liste de tables de stockages et le nombre de lignes dans chacune, à partir de la base de données par défaut sur la connexion actuelle.  
  
```  
SELECT TABLE_ID, ROWS_COUNT  
FROM $system.DISCOVER_STORAGE_TABLES  
ORDER BY TABLE_ID DESC  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de Analysis Services](../../../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md)  
  
  
