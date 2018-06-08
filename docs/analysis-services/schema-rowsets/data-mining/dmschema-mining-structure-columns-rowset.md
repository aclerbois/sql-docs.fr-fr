---
title: Ensemble de lignes DMSCHEMA_MINING_STRUCTURE_COLUMNS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 722adc702e40df68c4a137fb15120c81bcdef90d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dmschemaminingstructurecolumns-rowset"></a>Ensemble de lignes DMSCHEMA_MINING_STRUCTURE_COLUMNS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les colonnes de toutes les structures d’exploration de données déployées sur un serveur qui est en cours d’exécution [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DMSCHEMA_MINING_STRUCTURE_COLUMNS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**STRUCTURE_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue.|  
|**STRUCTURE_SCHEMA**|**DBTYPE_WSTR**||Nom de schéma non qualifié. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ne prend pas en charge les schémas, par conséquent, cette colonne est toujours **NULL**.|  
|**NOM_STRUCTURE**|**DBTYPE_WSTR**||Nom de la structure. Cette colonne ne peut pas contenir un **NULL**.|  
|**COLUMN_NAME**|**DBTYPE_WSTR**||Nom de la colonne. L'unicité est garantie uniquement au sein des colonnes qui partagent le même modèle. Par exemple, deux colonnes imbriquées peuvent porter le même nom si elles appartiennent à deux tables imbriquées distinctes dans la même structure.|  
|**COLUMN_GUID**|**DBTYPE_GUID**||GUID de la colonne. Les fournisseurs qui n’utilisent pas de GUID pour identifier les colonnes doivent retourner **NULL** dans cette colonne.|  
|**COLUMN_PROPID**|**DBTYPE_UI4**||ID de propriété de la colonne. Les fournisseurs qui n’associent aucun ID de propriété avec des colonnes doivent retourner **NULL** dans cette colonne. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] Retourne **NULL** pour cette colonne.|  
|**ORDINAL_POSITION**|**DBTYPE_UI4**||Valeur ordinale de la colonne. Les colonnes sont numérotées à partir de 1. **NULL** s’il n’existe aucune valeur ordinale stable pour la colonne.|  
|**COLUMN_HASDEFAULT**|**DBTYPE_BOOL**||Valeur booléenne qui indique si cette colonne possède une valeur par défaut.<br /><br /> **TRUE** si la colonne a une valeur par défaut.<br /><br /> **FALSE** si la colonne n’a pas de valeur par défaut ou si elle est inconnue, indique si la colonne a une valeur par défaut.|  
|**COLUMN_DEFAULT**|**DBTYPE_WSTR**||La valeur par défaut de la colonne. Un fournisseur peut exposer **DBCOLUMN_DEFAULTVALUE** mais pas **DBCOLUMN_HASDEFAULT** (pour les tables de la norme ISO) dans l’ensemble de lignes retourné par **IColumnsRowset::GetColumnsRowset**.<br /><br /> Si la valeur par défaut est **NULL**, **COLUMN_HASDEFAULT** est **TRUE** et **COLUMN_DEFAULT** colonne est un **NULL** valeur.|  
|**COLUMN_FLAGS**|**DBTYPE_UI4**||Masque de bits qui décrit les caractéristiques de la colonne. Le **DBCOLUMNFLAGS** type énuméré spécifie les bits du masque de bits. Cette colonne ne peut pas contenir un **NULL** valeur. Les valeurs valides sont les suivantes :<br /><br /> **DBCOLUMNFLAGS_ISNULLABLE** (**0 x 20**)<br /><br /> **DBCOLUMNFLAGS_MAYBENULL** (**0 x 40**)<br /><br /> **DBCOLUMNFLAGS_ISLONG** (**0 x 80**)|  
|**IS_NULLABLE**|**DBTYPE_BOOL**||Valeur booléenne qui indique si cette colonne possède une valeur par défaut.<br /><br /> **TRUE** si la colonne peut contenir **NULL**; **FALSE**, dans le cas contraire.|  
|**DATA_TYPE**|**DBTYPE_UI2**||Indicateur du type de données de la colonne. Par exemple :<br /><br /> «**TABLE**» = **DBTYPE_HCHAPTER**<br /><br /> «**TEXTE**» = **DBTYPE_WCHAR**<br /><br /> «**LONG**» = **DBTYPE_I8**<br /><br /> «**DOUBLE**» = **DBTYPE_R8**<br /><br /> «**DATE**» = **DBTYPE_DATE**|  
|**TYPE_GUID**|**DBTYPE_GUID**||GUID du type de données de la colonne. Les fournisseurs qui n’utilisent pas de GUID pour identifier les types de données doivent retourner **NULL** dans cette colonne.|  
|**CHARACTER_MAXIMUM_LENGTH**|**DBTYPE_UI4**||Longueur maximale possible pour une valeur de la colonne. Pour les colonnes de type character, binary ou bit, il s'agit de l'une des valeurs suivantes :<br /><br /> Longueur maximale de la colonne en caractères, octets ou bits, respectivement, si la longueur est définie. Par exemple, la longueur maximale d'une colonne `CHAR(5)` dans une table SQL est 5.<br /><br /> Longueur maximale du type de données en caractères, octets ou bits, respectivement, si la longueur de la colonne n'est pas définie.<br /><br /> Zéro (0) si la longueur maximale n'est définie ni pour la colonne, ni pour le type de données.<br /><br /> **NULL** pour tous les autres types de colonnes.|  
|**CHARACTER_OCTET_LENGTH**|**DBTYPE_UI4**||Longueur maximale en octets de la colonne, si la colonne est de type character ou binary. La valeur zéro (0) signifie que la colonne ne possède pas de longueur maximale. **NULL** pour tous les autres types de colonnes.|  
|**NUMERIC_PRECISION**|**DBTYPE_UI2**||La précision maximale de la colonne si le type de données de la colonne est un type de données numérique autre que **VARNUMERIC**; **NULL** si le type de données de la colonne n’est pas numérique ou est **VARNUMERIC**.<br /><br /> La précision des colonnes avec un type de données **DBTYPE_DECIMAL** ou **DBTYPE_NUM**ERIC dépend de la définition de la colonne.|  
|**NUMERIC_SCALE**|**DBTYPE_I2**||Le nombre de chiffres à droite de la virgule décimale si l’indicateur de type de la colonne est **DBTYPE_DECIMAL**, **DBTYPE_NUMERIC**, ou **DBTYPE_VARNUMERIC**. Sinon, c’est **NULL**.|  
|**DATETIME_PRECISION**|**DBTYPE_UI4**||Précision DateTime (nombre de chiffres dans la partie des fractions de secondes) de la colonne si cette dernière est de type datetime ou interval. Si le type de données de la colonne n’est pas datetime, il s’agit **NULL**.|  
|**CHARACTER_SET_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue dans lequel le jeu de caractères est défini. **NULL** si le fournisseur ne prend pas en charge les catalogues ou plusieurs jeux de caractères.|  
|**CHARACTER_SET_SCHEMA**|**DBTYPE_WSTR**||Le nom de schéma non qualifié dans lequel le jeu de caractères est défini. **NULL** si le fournisseur ne prend pas en charge les schémas ou plusieurs jeux de caractères.|  
|**CHARACTER_SET_NAME**|**DBTYPE_WSTR**||Nom du jeu de caractères. **NULL** si le fournisseur ne prend pas en charge plusieurs jeux de caractères.|  
|**COLLATION_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue dans lequel le classement est défini. **NULL** si le fournisseur ne prend pas en charge les catalogues ou plusieurs classements.|  
|**COLLATION_SCHEMA**|**DBTYPE_WSTR**||Nom de schéma non qualifié dans lequel le classement est défini. **NULL** si le fournisseur ne prend pas en charge les schémas ou des classements différents.|  
|**COLLATION_NAME**|**DBTYPE_WSTR**||Nom du classement. **NULL** si le fournisseur ne prend pas en charge des classements différents.|  
|**DOMAIN_CATALOG**|**DBTYPE_WSTR**||Nom du catalogue dans lequel le domaine est défini. **NULL** si le fournisseur ne prend pas en charge les catalogues ou les domaines.|  
|**DOMAIN_SCHEMA**|**DBTYPE_WSTR**||Nom du schéma non qualifié dans lequel le domaine est défini. **NULL** si le fournisseur ne prend pas en charge les schémas ou les domaines.|  
|**NOM_DOMAINE**|**DBTYPE_WSTR**||Le nom de domaine. **NULL** si le fournisseur ne prend pas en charge les domaines.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Description explicite de la colonne. **NULL** s’il n’existe aucune description associée à la colonne.|  
|**DISTRIBUTION_FLAG**|**DBTYPE_WSTR**||Distribution de la colonne de structure d'exploration de données :<br /><br /> «**NORMAL**»<br /><br /> «**LOG_NORM**AL »<br /><br /> «**UNIFORME**»|  
|**TYPE_CONTENU**|**DBTYPE_WSTR**||Type de contenu de la colonne de structure d'exploration de données :<br /><br /> «**CLÉ**»<br /><br /> «**DISCRÈTE**»<br /><br /> «**CONTINU**»<br /><br /> «**DISCRETIZED (**[args]**)**»<br /><br /> «**COMMANDÉ**»<br /><br /> «**SEQUENCE_TIME**»<br /><br /> «**CYCLIQUE**»<br /><br /> «**PROBABILITÉ**»<br /><br /> «**VARIANCE**»<br /><br /> «**STDEV**»<br /><br /> «**PRISE EN CHARGE**»<br /><br /> «**PROBABILITY_VARIANCE**»<br /><br /> «**PROBABILITY_STDEV**»|  
|**MODELING_FLAG**|**DBTYPE_WSTR**||Liste d'indicateurs de modélisation délimitée par des virgules. Le seul indicateur pris en charge pour une colonne de structure est «**non NULL**».|  
|**IS_RELATED_TO_KEY**|**DBTYPE_BOOL**||Valeur booléenne qui indique si cette colonne est associée à la clé.<br /><br /> **VARIANT_TRUE** si cette colonne est associée à la clé. **VARIANT_FALSE** dans le cas contraire. Si la clé est une colonne unique, la **RELATED_ATTRIBUTE** champ peut éventuellement contenir son nom de colonne.|  
|**RELATED_ATTRIBUTE**|**DBTYPE_WSTR**||Nom de la colonne cible à laquelle la colonne actuelle est associée ou pour laquelle elle constitue une propriété spéciale.|  
|**CONTAINING_COLUMN**|**DBTYPE_WSTR**||Le nom de la **TABLE** colonne contenant cette colonne. **NULL** si aucune table ne contient la colonne.|  
|**IS_POPULATED**|**DBTYPE_BOOL**||Valeur booléenne qui indique si cette colonne a appris un jeu de valeurs possibles.<br /><br /> **TRUE** si la colonne a appris un jeu de valeurs possibles ; **FALSE**, dans le cas contraire.|  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DMSCHEMA_MINING_STRUCTURE_COLUMNS** ensemble de lignes peut être restreint sur les colonnes dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**STRUCTURE_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**STRUCTURE_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_STRUCTURE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**COLUMN_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de données d’exploration de données](../../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
