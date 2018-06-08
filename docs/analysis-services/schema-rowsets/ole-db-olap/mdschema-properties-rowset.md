---
title: Ensemble de lignes MDSCHEMA_PROPERTIES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7cccd5d28d3f4d2675229929a2bf3d35b383a032
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemaproperties-rowset"></a>Ensemble de lignes MDSCHEMA_PROPERTIES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les propriétés des membres d'une base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_PROPERTIES** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**||Nom de la base de données.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**||Nom du schéma auquel appartient cette propriété. **NULL** si le fournisseur ne prend pas en charge les schémas.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Nom du cube.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**||Le nom unique de la dimension. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique de la hiérarchie. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du niveau auquel appartient cette propriété. Si le fournisseur ne prend pas en charge les niveaux nommés, elle doit retourner la **DIMENSION_UNIQUE_NAME** valeur pour ce champ. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**MEMBER_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du membre auquel appartient cette propriété. Utilisé pour les magasins de données qui ne prennent pas en charge les niveaux nommés ou qui ont des propriétés sur une base membre-par-membre. Si la propriété s’applique à tous les membres d’un niveau, cette colonne est **NULL**. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**PROPERTY_TYPE**|**DBTYPE_I2**||Bitmap qui spécifie le type de la propriété :<br /><br /> **MDPROP_MEMBER** (**1**) identifie une propriété d’un membre. Cette propriété peut être utilisée dans la clause DIMENSION PROPERTIES de l'instruction SELECT.<br /><br /> **MDPROP_CELL** (**2**) identifie une propriété d’une cellule. Cette propriété peut être utilisée dans la clause CELL PROPERTIES qui apparaît à la fin de l'instruction SELECT.<br /><br /> **MDPROP_SYSTEM** (**4**) identifie une propriété interne.<br /><br /> **MDPROP_BLOB** (**8**) identifie une propriété qui contient un objet binaire volumineux (blob).|  
|**PROPERTY_NAME**|**DBTYPE_WSTR**||Nom de la propriété. Si la clé pour la propriété est le même que le nom de la propriété, **PROPERTY_NAME** sera vide.|  
|**PROPERTY_CAPTION**|**DBTYPE_WSTR**||Étiquette ou légende associée à la propriété, essentiellement à des fins d’affichage. Retourne **PROPERTY_NAME** si une légende n’existe pas.|  
|**DATA_TYPE**|**DBTYPE_UI2**||Type de données de la propriété.|  
|**CHARACTER_MAXIMUM_LENGTH**|**DBTYPE_UI4**||Longueur maximale possible de la propriété, s'il s'agit d'un caractère, d'une donnée binaire ou de type bit.<br /><br /> Zéro indique qu'il n'existe pas de longueur maximale définie.<br /><br /> Retourne **NULL** pour tous les autres types de données.|  
|**CHARACTER_OCTET_LENGTH**|**DBTYPE_UI4**||Longueur maximale possible (en octets) de la propriété, s'il s'agit d'un caractère ou d'une donnée binaire.<br /><br /> Zéro indique qu'il n'existe pas de longueur maximale définie.<br /><br /> Retourne **NULL** pour tous les autres types de données.|  
|**NUMERIC_PRECISION**|**DBTYPE_UI2**||Précision maximale de la propriété, s'il s'agit d'un type de données numérique.<br /><br /> Retourne **NULL** pour tous les autres types de données.|  
|**NUMERIC_SCALE**|**DBTYPE_I2**||Le nombre de chiffres à droite de la virgule décimale, s’il s’agit un **DBTYPE_NUMERIC** ou **DBTYPE_DECIMAL** type.<br /><br /> Retourne **NULL** pour tous les autres types de données.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Description explicite de la propriété. **NULL** si aucune description n’existe.|  
|**PROPERTY_CONTENT_TYPE**|**DBTYPE_I2**||Le type de la propriété. Il peut s'agir de l'une des énumérations suivantes :<br /><br /> **MD_PROPTYPE_REGULAR** (**0 x 00**)<br /><br /> **MD_PROPTYPE_ID** (**0 x 01**)<br /><br /> **MD_PROPTYPE_RELATION_TO_PARENT** (**0 x 02**)<br /><br /> **MD_PROPTYPE_ROLLUP_OPERATOR** (**0 x 03**)<br /><br /> **MD_PROPTYPE_ORG_TITLE** (**0 x 11**)<br /><br /> **MD_PROPTYPE_CAPTION** (**0 x 21**)<br /><br /> **MD_PROPTYPE_CAPTION_SHORT** (**0 x 22**)<br /><br /> **MD_PROPTYPE_CAPTION_DESCRIPTION** (**0 x 23**)<br /><br /> **MD_PROPTYPE_CAPTION_ABREVIATION** (**0 x 24**)<br /><br /> **MD_PROPTYPE_WEB_URL** (**0 x 31**)<br /><br /> **MD_PROPTYPE_WEB_HTML** (**0 x 32**)<br /><br /> **MD_PROPTYPE_WEB_XML_OR_XSL** (**0 x 33**)<br /><br /> **MD_PROPTYPE_WEB_MAIL_ALIAS** (**0 x 34**)<br /><br /> **MD_PROPTYPE_ADDRESS** (**0 x 41**)<br /><br /> **MD_PROPTYPE_ADDRESS_STREET** (**0 x 42**)<br /><br /> **MD_PROPTYPE_ADDRESS_HOUSE** (**0 x 43**)<br /><br /> **MD_PROPTYPE_ADDRESS_CITY** (**0 x 44**)<br /><br /> **MD_PROPTYPE_ADDRESS_STATE_OR_PROVINCE** (**0 x 45**)<br /><br /> **MD_PROPTYPE_ADDRESS_ZIP** (**0 x 46**)<br /><br /> **MD_PROPTYPE_ADDRESS_QUARTER** (**0 x 47**)<br /><br /> **MD_PROPTYPE_ADDRESS_COUNTRY** (**0 x 48**)<br /><br /> **MD_PROPTYPE_ADDRESS_BUILDING** (**0 x 49**)<br /><br /> **MD_PROPTYPE_ADDRESS_ROOM** (**0x4A**)<br /><br /> **MD_PROPTYPE_ADDRESS_FLOOR** (**0x4B**)<br /><br /> **MD_PROPTYPE_ADDRESS_FAX** (**0x4C**)<br /><br /> **MD_PROPTYPE_ADDRESS_PHONE** (**0x4D**)<br /><br /> **MD_PROPTYPE_GEO_CENTROID_X** (**0 x 61**)<br /><br /> **MD_PROPTYPE_GEO_CENTROID_Y** (**0x62**)<br /><br /> **MD_PROPTYPE_GEO_CENTROID_Z** (**0 x 63**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_TOP** (**0 x 64**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_LEFT** (**0 x 65**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_BOTTOM** (**0x66**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_RIGHT** (**0 x 67**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_FRONT** (**0x68**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_REAR** (**0x69**)<br /><br /> **MD_PROPTYPE_GEO_BOUNDARY_POLYGON** (**0x6A**)<br /><br /> **MD_PROPTYPE_PHYSICAL_SIZE** (**0x71**)<br /><br /> **MD_PROPTYPE_PHYSICAL_COLOR** (**0 x 72**)<br /><br /> **MD_PROPTYPE_PHYSICAL_WEIGHT** (**0 x 73**)<br /><br /> **MD_PROPTYPE_PHYSICAL_HEIGHT** (**0 x 74 pour**)<br /><br /> **MD_PROPTYPE_PHYSICAL_WIDTH** (**0 x 75**)<br /><br /> **MD_PROPTYPE_PHYSICAL_DEPTH** (**0x76**)<br /><br /> **MD_PROPTYPE_PHYSICAL_VOLUME** (**0 x 77**)<br /><br /> **MD_PROPTYPE_PHYSICAL_DENSITY** (**0 x 78**)<br /><br /> **MD_PROPTYPE_PERSON_FULL_NAME** (**0 x 82**)<br /><br /> **MD_PROPTYPE_PERSON_FIRST_NAME** (**0 x 83**)<br /><br /> **MD_PROPTYPE_PERSON_LAST_NAME** (**0 x 84**)<br /><br /> **MD_PROPTYPE_PERSON_MIDDLE_NAME** (**0 x 85**)<br /><br /> **MD_PROPTYPE_PERSON_DEMOGRAPHIC** (**0x86**)<br /><br /> **MD_PROPTYPE_PERSON_CONTACT** (**0 x 87**)<br /><br /> **MD_PROPTYPE_QTY_RANGE_LOW** (**0 x 91**)<br /><br /> **MD_PROPTYPE_QTY_RANGE_HIGH** (**0 x 92**)<br /><br /> **MD_PROPTYPE_FORMATTING_COLOR** (**0xA1**)<br /><br /> **MD_PROPTYPE_FORMATTING_ORDER** (**0xA2**)<br /><br /> **MD_PROPTYPE_FORMATTING_FONT** (**0xA3**)<br /><br /> **MD_PROPTYPE_FORMATTING_FONT_EFFECTS** (**0xA4**)<br /><br /> **MD_PROPTYPE_FORMATTING_FONT_SIZE** (**0xA5**)<br /><br /> **MD_PROPTYPE_FORMATTING_SUB_TOTAL** (**0xA6**)<br /><br /> **MD_PROPTYPE_DATE** (**0xB1**)<br /><br /> **MD_PROPTYPE_DATE_START** (**0xB2**)<br /><br /> **MD_PROPTYPE_DATE_ENDED** (**0xB3**)<br /><br /> **MD_PROPTYPE_DATE_CANCELED** (**0xB4**)<br /><br /> **MD_PROPTYPE_DATE_MODIFIED** (**0xB5**)<br /><br /> **MD_PROPTYPE_DATE_DURATION** (**0xB6**)<br /><br /> **MD_PROPTYPE_VERSION** (**0xC1**)|  
|**SQL_COLUMN_NAME**|**DBTYPE_WSTR**||Nom de la propriété utilisé dans les requêtes SQL de la dimension du cube ou de la base de données.|  
|**LANGUAGE**|**DBTYPE_UI2**||La traduction exprimée sous la forme un **LCID**. Valide uniquement pour les traductions de propriété.|  
|**PROPERTY_ORIGIN**|**DBTYPE_UI2**||Identifie le type de hiérarchie auquel s'applique la propriété :<br /><br /> **MD_USER_DEFINED** (**1**) indique que la propriété est sur une hiérarchie définie par l’utilisateur<br /><br /> **MD_SYSTEM_ENABLED** (**2**) indique que la propriété est sur une hiérarchie d’attribut<br /><br /> **MD_SYSTEM_DISABLED** (**4**) indique que la propriété est sur une hiérarchie d’attribut qui n’est pas activée.|  
|**PROPERTY_ATTRIBUTE_HIERARCHY_NAME**|**DBTYPE_WSTR**||Nom de la hiérarchie d'attributs fournissant la source de la propriété.|  
|**PROPERTY_CARDINALITY**|**DBTYPE_WSTR**||Cardinalité de la propriété. Elle peut prendre les valeurs de chaîne suivantes :<br /><br /> **UNE**<br /><br /> **NOMBREUX**|  
|**MIME_TYPE**|**DBTYPE_WSTR**||Type MIME pour les objets BLOB.|  
|**PROPERTY_IS_VISIBLE**|**DBTYPE_BOOL**||Valeur booléenne qui indique si la propriété est visible.<br /><br /> **TRUE** si la propriété est visible ; sinon, **FALSE**.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_PROPERTIES** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Obligatoire|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**MEMBER_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**PROPERTY_TYPE**|**DBTYPE_I2**|Ce paramètre est facultatif|  
|**PROPERTY_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**PROPERTY_CONTENT_TYPE**|**DBTYPE_I2**|(Facultatif) Une restriction par défaut est en place sur **MDPROP_MEMBER** ou **MDPROP_CELL**.|  
|**PROPERTY_ORIGIN**|**DBTYPE_UI2**|(Facultatif) Une restriction par défaut est en place sur **MD_USER_DEFINED** ou **MD_SYSTEM_ENABLED**.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1.  Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
|**PROPERTY_VISIBILITY**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 Visible<br /><br /> 2 non visible|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
