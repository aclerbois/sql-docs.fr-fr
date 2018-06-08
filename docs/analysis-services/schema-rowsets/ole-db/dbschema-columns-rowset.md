---
title: Ensemble de lignes DBSCHEMA_COLUMNS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2bb1fec6a1633f545f0c65feda93c7e19c649a6e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dbschemacolumns-rowset"></a>Ensemble de lignes DBSCHEMA_COLUMNS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations de colonne pour toutes les colonnes qui répondent aux critères de restriction indiqués.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DBSCHEMA_COLUMNS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**TABLE_CATALOG**|**DBTYPE_WSTR**||Nom de la base de données.|  
|**TABLE_SCHEMA**|**DBTYPE_WSTR**||Non pris en charge.|  
|**NOM_TABLE**|**DBTYPE_WSTR**||Nom du cube.|  
|**COLUMN_NAME**|**DBTYPE_WSTR**||Nom de la mesure ou de la hiérarchie d'attribut.|  
|**COLUMN_GUID**|**DBTYPE_GUID**||Non pris en charge.|  
|**COLUMN_PROPID**|**DBTYPE_UI4**||Non pris en charge.|  
|**ORDINAL_POSITION**|**DBTYPE_UI4**||Position de la colonne, à partir de 1.|  
|**COLUMN_HAS_DEFAULT**|**DBTYPE_BOOL**||Non pris en charge.|  
|**COLUMN_DEFAULT**|**DBTYPE_WSTR**||Non pris en charge.|  
|**COLUMN_FLAGS**|**DBTYPE_UI4**||A **DBCOLUMNFLAGS** masque de bits indiquant les propriétés de la colonne. Consultez « Type d’énuméré DBCOLUMNFLAGS » dans [IColumnsInfo::GetColumnInfo](http://msdn2.microsoft.com/library/ms722704.aspx)|  
|**IS_NULLABLE**|**DBTYPE_BOOL**||Retourne toujours **false**.|  
|**DATA_TYPE**|**DBTYPE_WSTR**<br /><br /> **DBTYPE_VARIANT**||Type de données de la colonne. Retourne une chaîne pour les colonnes de dimension et une variante pour les mesures.|  
|**TYPE_GUID**|**DBTYPE_GUID**||Non pris en charge.|  
|**CHARACTER_MAXIMUM_LENGTH**|**DBTYPE_UI4**||Longueur maximale possible d'une valeur de la colonne.<br /><br /> Cette valeur est extraite à partir de la **DataSize** propriété dans le **DataItem**.|  
|**CHARACTER_OCTET_LENGTH**|**DBTYPE_UI4**||Longueur maximale possible d'une valeur de la colonne, en octets, pour les colonnes de type character ou binary.<br /><br /> Une valeur de zéro (0) indique que la colonne ne possède pas de longueur maximale.<br /><br /> **NULL** s’affichera pour les colonnes qui ne retournent pas de types de données binaire ou caractère.|  
|**NUMERIC_PRECISION**|**DBTYPE_UI2**||Les types autres que la précision maximale de la colonne pour les données numériques **DBTYPE_VARNUMERIC**.|  
|**NUMERIC_SCALE**|**DBTYPE_I2**||Le nombre de chiffres à droite de la virgule décimale pour **DBTYPE_DECIMAL**, **DBTYPE_NUMERIC**, **DBTYPE_VARNUMERIC**. Sinon, c’est **NULL**.|  
|**DATETIME_PRECISION**|**DBTYPE_UI4**||Non pris en charge.|  
|**CHARACTER_SET_CATALOG**|**DBTYPE_WSTR**||Non pris en charge.|  
|**CHARACTER_SET_SCHEMA**|**DBTYPE_WSTR**||Non pris en charge.|  
|**CHARACTER_SET_NAME**|**DBTYPE_WSTR**||Non pris en charge.|  
|**COLLATION_CATALOG**|**DBTYPE_WSTR**||Non pris en charge.|  
|**COLLATION_SCHEMA**|**DBTYPE_WSTR**||Non pris en charge.|  
|**COLLATION_NAME**|**DBTYPE_WSTR**||Non pris en charge.|  
|**DOMAIN_CATALOG**|**DBTYPE_WSTR**||Non pris en charge.|  
|**DOMAIN_SCHEMA**|**DBTYPE_WSTR**||Non pris en charge.|  
|**NOM_DOMAINE**|**DBTYPE_WSTR**||Non pris en charge.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Non pris en charge.|  
|**COLUMN_OLAP_TYPE**|**DBTYPE_WSTR**||Type OLAP de l'objet.<br /><br /> **MESURE** indique l’objet est une mesure.<br /><br /> **ATTRIBUT** indique l’objet est un attribut de dimension.<br /><br /> **SCHÉMA** indique l’objet est une colonne dans un schéma.|  
  
 L’ensemble de lignes est trié sur **TABLE_CATALOG**, **TABLE_SCHEMA**, **TABLE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DBSCHEMA_COLUMNS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**TABLE_CATALOG**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**TABLE_SCHEMA**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**NOM_TABLE**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**COLUMN_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**COLUMN_OLAP_TYPE**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma OLE DB](../../../analysis-services/schema-rowsets/ole-db/ole-db-schema-rowsets.md)  
  
  
