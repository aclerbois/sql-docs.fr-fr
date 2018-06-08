---
title: Ensemble de lignes DMSCHEMA_MINING_COLUMNS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1f5d5dcc4385634b24cadf9e08c2298cc7bacc26
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingcolumns-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_COLUMNS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les colonnes de tous les modèles d’exploration de données dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Cet ensemble de lignes est limité au catalogue actuel.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DMSCHEMA_MINING_COLUMNS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**|Nom du catalogue. Défini d'après le nom de la base de données dont le modèle est membre.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**|Nom de schéma non qualifié. Cette colonne n’est pas pris en charge par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**MODEL_NAME**|**DBTYPE_WSTR**|Le nom du modèle d’exploration de données. Cette colonne contient le nom du modèle d'exploration de données auquel une colonne est associée ; elle n'est jamais vide.|  
|**COLUMN_NAME**|**DBTYPE_WSTR**|Nom de la colonne.|  
|**COLUMN_GUID**|**DBTYPE_GUID**|GUID de la colonne. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**COLUMN_PROPID**|**DBTYPE_UI4**|ID de propriété de la colonne. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**ORDINAL_POSITION**|**DBTYPE_UI4**|La position ordinale de la colonne. Les colonnes sont numérotées à partir de 1. Cette colonne contient **NULL** s’il n’existe aucune valeur ordinale stable pour la colonne.|  
|**COLUMN_HAS_DEFAULT**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne possède une valeur par défaut.<br /><br /> **TRUE** si la colonne a une valeur par défaut, sinon **FALSE**.|  
|**COLUMN_DEFAULT**|**DBTYPE_WSTR**|La valeur par défaut de la colonne.<br /><br /> Si la valeur par défaut est la **NULL** valeur, **COLUMN_HASDEFAULT** contient **TRUE**, et cette colonne contient **NULL**.|  
|**COLUMN_FLAGS**|**DBTYPE_UI4**|Masque de bits qui décrit les caractéristiques de la colonne. Le **DBCOLUMNFLAGS** type énuméré spécifie les bits du masque de bits. Cette colonne n'est jamais vide.|  
|**IS_NULLABLE**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne autorise la valeur Null.<br /><br /> **FALSE** si la colonne ne connaît ne pas être nullable ; sinon, **TRUE**.|  
|**DATA_TYPE**|**DBTYPE_UI2**|Indicateur du type de données de la colonne. La liste suivante contient des exemples de types d'indicateurs retournés :<br /><br /> «**TABLE**» retournerait **DBTYPE_HCHAPTER**.<br /><br /> «**Texte**» retournerait **DBTYPE_WCHAR**.<br /><br /> «**LONG**» retournerait **DBTYPE_I8**.<br /><br /> «**DOUBLE**» retournerait **DBTYPE_R8**.<br /><br /> «**DATE**» retournerait **DBTYPE_DATE**.|  
|**TYPE_GUID**|**DBTYPE_GUID**|GUID du type de données de la colonne. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **VT_NULL**.|  
|**CHARACTER_MAXIMUM_LENGTH**|**DBTYPE_UI4**|Longueur maximale possible pour une valeur de la colonne. Pour les colonnes de type character, binary ou bit, il s'agit de l'une des valeurs suivantes :<br /><br /> Longueur maximale de la colonne en caractères, octets ou bits (selon le type de la colonne), si une longueur est définie. Par exemple, un **char (5)** colonne dans une table SQL a une longueur maximale de 5.<br /><br /> Longueur maximale du type de données en caractères, octets ou bits (selon le type de la colonne), si la longueur de la colonne n'est pas définie.<br /><br /> Zéro (0) si la longueur maximale n'est définie ni pour la colonne, ni pour le type de données.<br /><br /> **NULL** pour tous les autres types de colonnes|  
|**CHARACTER_OCTET_LENGTH**|**DBTYPE_UI4**|Longueur maximale en octets de la colonne, si la colonne est de type character ou binary. La valeur zéro (0) signifie que la colonne ne possède pas de longueur maximale. Cette colonne contient **NULL** pour tous les autres types de colonnes.|  
|**NUMERIC_PRECISION**|**DBTYPE_UI2**|La précision maximale de la colonne si le type de données de la colonne est un type de données numérique autre que **VARNUMERIC**.<br /><br /> **NULL** si le type de données de la colonne n’est pas numérique ou est **VARNUMERIC**.<br /><br /> La précision des colonnes avec un type de données **DBTYPE_DECIMAL** ou **DBTYPE_NUMERIC** dépend de la définition de colonne.|  
|**NUMERIC_SCALE**|**DBTYPE_I2**|Le nombre de chiffres à droite de la virgule décimale si l’indicateur de type de la colonne est **DBTYPE_DECIMAL**, **DBTYPE_NUMERIC**, ou **DBTYPE_VARNUMERIC**. Sinon, cette colonne contient **VT_NULL**.|  
|**DATETIME_PRECISION**|**DBTYPE_UI4**|La précision de date/heure (nombre de chiffres dans la partie fractions de secondes) de la colonne si le type de données de colonne est un type DateTime ou interval ; dans le cas contraire, **NULL**.|  
|**CHARACTER_SET_CATALOG**|**DBTYPE_WSTR**|Nom du catalogue dans lequel le jeu de caractères est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**CHARACTER_SET_SCHEMA**|**DBTYPE_WSTR**|Nom de schéma non qualifié dans lequel le jeu de caractères est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**CHARACTER_SET_NAME**|**DBTYPE_WSTR**|Nom du jeu de caractères. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**COLLATION_CATALOG**|**DBTYPE_WSTR**|Nom du catalogue dans lequel le classement est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**COLLATION_SCHEMA**|**DBTYPE_WSTR**|Un nom de schéma non qualifié dans lequel le classement est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**COLLATION_NAME**|**DBTYPE_WSTR**|Nom du classement. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**DOMAIN_CATALOG**|**DBTYPE_WSTR**|Nom du catalogue dans lequel le domaine est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**DOMAIN_SCHEMA**|**DBTYPE_WSTR**|Nom du schéma non qualifié dans lequel le domaine est défini. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**NOM_DOMAINE**|**DBTYPE_WSTR**|Le nom de domaine. Cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description conviviale de la colonne de cette colonne n’est pas pris en charge par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]; il contient toujours **NULL**.|  
|**DISTRIBUTION_FLAG**|**DBTYPE_WSTR**|Description de la distribution statistique de la colonne. Cette colonne contient l'une des valeurs suivantes :<br /><br /> «**NORMAL**»<br /><br /> «**LOG_NORMAL**»<br /><br /> «**UNIFORME**»|  
|**TYPE_CONTENU**|**DBTYPE_WSTR**|Description du contenu de la colonne. Cette colonne contient l'une des valeurs suivantes :<br /><br /> «**CLÉ**»<br /><br /> «**DISCRÈTE**»<br /><br /> «**CONTINU**»<br /><br /> «**DISCRETIZED (**[arguments]**)**»<br /><br /> «**COMMANDÉ**»<br /><br /> «**TEMPS CLÉ**»<br /><br /> «**CYCLIQUE**»<br /><br /> «**PROBABILITÉ**»<br /><br /> «**VARIANCE**»<br /><br /> «**STDEV**»<br /><br /> «**PRISE EN CHARGE**»<br /><br /> «**PROBABILITY_VARIANCE**»<br /><br /> «**PROBABILITY_STDEV**»<br /><br /> **« SÉQUENCE CLÉ**»|  
|**MODELING_FLAG**|**DBTYPE_WSTR**|Liste d'indicateurs délimitée par des virgules. Les indicateurs définis sont les suivants :<br /><br /> «**MODEL_EXISTENCE_ONLY**»<br /><br /> «**RÉGRESSEUR**»<br /><br /> Des indicateurs de modélisation spécifiques des algorithmes peuvent également être contenus dans cette colonne.|  
|**IS_RELATED_TO_KEY**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne est associée à la clé.<br /><br /> **TRUE** si cette colonne est associée à la clé. Si la clé est une colonne unique, la **RELATED_ATTRIBUTE** champ peut éventuellement contenir son nom de colonne.|  
|**RELATED_ATTRIBUTE**|**DBTYPE_WSTR**|Le nom de la colonne cible à laquelle la colonne actuelle est liée ou est une propriété spéciale.|  
|**IS_INPUT**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne est une colonne d'entrée.<br /><br /> **VARIANT_TRUE** s’il s’agit d’une colonne d’entrée.|  
|**IS_PREDICTABLE**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne est prédictible.<br /><br /> **TRUE** si la colonne est prédictible.|  
|**CONTAINING_COLUMN**|**DBTYPE_WSTR**|Le nom de la **TABLE** colonne qui contient cette colonne. Cette colonne contient **NULL** si la colonne n’est pas contenue dans une autre colonne.|  
|**PREDICTION_SCALAR_FUNCTIONS**|**DBTYPE_WSTR**|Liste, délimitée par des virgules, de fonctions scalaires pouvant être exécutées sur la colonne.|  
|**PREDICTION_TABLE_FUNCTIONS**|**DBTYPE_WSTR**|Liste, délimitée par des virgules, de fonctions pouvant être appliquées à la colonne. Les fonctions doivent retourner une table. La liste est au format suivant :<br /><br /> `<function name>(<column1> [, <column2>], ...)`<br /><br /> Le format permet à l'application cliente de déterminer la signature (liste de paramètres) de la fonction correspondante.|  
|**IS_POPULATED**|**DBTYPE_BOOL**|Valeur booléenne qui indique si la colonne a fait l'objet d'un apprentissage d'un jeu de valeurs possibles.<br /><br /> **TRUE** si la colonne a été effectuée avec un ensemble de valeurs possibles.<br /><br /> Contient **FALSE** si la colonne n’est pas remplie.|  
|**PREDICTION_SCORE**|**DBTYPE_R8**|Score du modèle lors de la prédiction de la colonne. Le score est utilisé pour mesurer l'exactitude d'un modèle.|  
|**SOURCE_COLUMN**|**DBTYPE_WSTR**|Nom de la colonne de structure d'exploration de données source correspondant à la colonne d'exploration de données actuelle.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DMSCHEMA_MINING_COLUMNS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**MODEL_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MODEL_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**COLUMN_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
