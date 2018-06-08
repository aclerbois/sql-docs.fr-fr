---
title: Ensemble de lignes MDSCHEMA_MEASURES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0d5d5e53b79605d71c1df3d7ce83192af5d77ec6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemameasures-rowset"></a>Ensemble de lignes MDSCHEMA_MEASURES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit chaque mesure dans un cube.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_MEASURES** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**||Nom du catalogue auquel appartient cette mesure. Valeur**NULL** si le fournisseur ne prend pas en charge les catalogues.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**||Nom du schéma auquel appartient cette mesure. **NULL** si le fournisseur ne prend pas en charge les schémas.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Nom du cube auquel appartient cette mesure.|  
|**MEASURE_NAME**|**DBTYPE_WSTR**||Nom de la mesure.|  
|**MEASURE_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique de la mesure. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**MEASURE_CAPTION**|**DBTYPE_WSTR**||Étiquette ou légende associée à la mesure. Principalement utilisée à des fins d'affichage. Si une légende n’existe pas, **MEASURE_NAME** est retourné.|  
|**MEASURE_GUID**|**DBTYPE_GUID**||Non pris en charge.|  
|**MEASURE_AGGREGATOR**|**DBTYPE_I4**||Énumération qui indique comment une mesure a été dérivée. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> **MDMEASURE_AGGR_SUM** (**1**) identifie que la mesure agrège à partir de **somme**.<br /><br /> **MDMEASURE_AGGR_COUNT** (**2**) identifie que la mesure agrège à partir de **nombre**.<br /><br /> **MDMEASURE_AGGR_MIN** (**3**) identifie que la mesure agrège à partir de **MIN**.<br /><br /> **MDMEASURE_AGGR_MAX** (**4**) identifie que la mesure agrège à partir de **MAX**.<br /><br /> **MDMEASURE_AGGR_AVG** (**5**) identifie que la mesure agrège à partir de **AVG**.<br /><br /> **MDMEASURE_AGGR_VAR** (**6**) identifie que la mesure agrège à partir de **VAR**.<br /><br /> **MDMEASURE_AGGR_STD** (**7**) identifie que la mesure agrège à partir de **STDEV**.<br /><br /> **MDMEASURE_AGGR_DST** (**8**) identifie que la mesure agrège à partir de **comptage de valeurs**.<br /><br /> **MDMEASURE_AGGR_NONE** (**9**) identifie que la mesure agrège à partir de **NONE**.<br /><br /> **MDMEASURE_AGGR_AVGCHILDREN** (**10**) identifie que la mesure agrège à partir de **AVERAGEOFCHILDREN**.<br /><br /> **MDMEASURE_AGGR_FIRSTCHILD** (**11**) identifie que la mesure agrège à partir de **FIRSTCHILD**.<br /><br /> **MDMEASURE_AGGR_LASTCHILD** (**12**) identifie que la mesure agrège à partir de **LASTCHILD**.<br /><br /> **MDMEASURE_AGGR_FIRSTNONEMPTY** (**13**) identifie que la mesure agrège à partir de **FIRSTNONEMPTY**,<br /><br /> **MDMEASURE_AGGR_LASTNONEMPTY** (**14**) identifie que la mesure agrège à partir de **LASTNONEMPTY**.<br /><br /> **MDMEASURE_AGGR_BYACCOUNT** (**15**) identifie que la mesure agrège à partir de **BYACCOUNT**.<br /><br /> **MDMEASURE_AGGR_CALCULATED** (**127**) indique que la mesure a été dérivée à partir d’une formule qui n’était pas celles citées ci-dessus.<br /><br /> **MDMEASURE_AGGR_UNKNOWN** (**0**) indique que la mesure a été dérivée à partir d’une fonction d’agrégation inconnue ou de la formule.|  
|**DATA_TYPE**|**DBTYPE_UI2**||Type de données de la mesure.|  
|**NUMERIC_PRECISION**|**DBTYPE_UI2**||Précision maximale de la propriété si le type de données de l'objet de mesure est une valeur numérique exacte. **NULL** pour tous les autres types de propriété.|  
|**NUMERIC_SCALE**|**DBTYPE_I2**||Le nombre de chiffres à droite de la virgule décimale si l’indicateur de type de l’objet de mesures est **DBTYPE_NUMERIC** ou **DBTYPE_DECIMAL**. Sinon, cette valeur est **NULL**.|  
|**MEASURE_UNITS**|**DBTYPE_WSTR**||Non pris en charge|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Description explicite de la mesure. **NULL** si aucune description n’existe.|  
|**EXPRESSION**|**DBTYPE_WSTR**||Une expression pour le membre.|  
|**MEASURE_IS_VISIBLE**|**DBTYPE_BOOL**||Valeur booléenne qui retourne toujours True. Si la mesure n'est pas visible, elle n'est pas incluse dans l'ensemble de lignes du schéma.|  
|**LEVELS_LIST**|**DBTYPE_WSTR**||Chaîne qui retourne toujours **NULL**.|  
|**MEASURE_NAME_SQL_COLUMN_NAME**|**DBTYPE_WSTR**||Nom de la colonne de la requête SQL qui correspond au nom de la mesure.|  
|**MEASURE_UNQUALIFIED_CAPTION**|**DBTYPE_WSTR**||Nom de la mesure, non qualifié avec le nom du groupe de mesures.|  
|**MEASUREGROUP_NAME**|**DBTYPE_WSTR**||Nom du groupe de mesures auquel appartient cette mesure.|  
|**MEASURE_DISPLAY_FOLDER**|**DBTYPE_WSTR**||Chemin d'accès à utiliser pour l'affichage de la mesure dans l'interface utilisateur. Les noms de dossiers doivent être séparés par un point-virgule. Les dossiers imbriqués sont indiqués par une barre oblique inverse (\\).|  
|**DEFAULT_FORMAT_STRING**|**DBTYPE_WSTR**||Chaîne de format par défaut pour la mesure.|  
  
 L’ensemble de lignes est trié sur **CATALOG_NAME**, **nom_schéma**, **CUBE_NAME**, **MEASURE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_MEASURES** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEASURE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEASURE_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEASUREGROUP_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
|**MEASURE_VISIBILITY**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 Visible<br /><br /> 2 Non visible|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
