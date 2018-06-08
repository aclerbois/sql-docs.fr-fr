---
title: SchemaEnum | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- SchemaEnum
helpviewer_keywords:
- SchemaEnum enumeration [ADO]
ms.assetid: 21c97651-297f-469f-b5b5-c48af72b62a8
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5cdc032d770f587e8c78c4df0f195d9535688888
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="schemaenum"></a>SchemaEnum
Spécifie le type de schéma **Recordset** qui le [OpenSchema](../../../ado/reference/ado-api/openschema-method.md) récupère de la méthode.  
  
## <a name="remarks"></a>Notes  
 Des informations supplémentaires sur la fonction et les colonnes renvoyées pour chaque constante ADO permettre être trouvé dans les rubriques de [Appendix B: Schema Rowsets](http://msdn.microsoft.com/en-us/2b5fbf03-e50d-44ee-bc57-5a57666c55f1) de référence du programmeur OLE DB. Le nom de chaque rubrique figure entre parenthèses dans la section Description du tableau suivant.  
  
 Plus d’informations sur la fonction et les colonnes retournées pour chaque constante ADO MD sont accessibles dans les rubriques de [OLE DB pour les objets OLAP et d’ensembles de lignes de schéma](http://msdn.microsoft.com/en-us/d20bb2a6-68bd-423f-9ec8-eb930cd0c144) dans OLE DB pour la documentation de traitement analytique en ligne (OLAP). Le nom de chaque rubrique figure entre parenthèses dans la colonne Description du tableau suivant.  
  
 Vous pouvez traduire les types de données de colonnes dans la documentation OLE DB en types de données ADO en vous reportant à la colonne Description de ADO [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) rubrique. Par exemple, un type de données OLE DB de **DBTYPE_WSTR** équivaut à un type de données ADO **adWChar**.  
  
 ADO génère des résultats de type schéma pour les constantes, **adSchemaDBInfoKeywords** et **adSchemaDBInfoLiterals**. ADO crée un **Recordset**, puis remplit chaque ligne avec les valeurs retournées respectivement par le **IDBInfo::GetKeywords** et **IDBInfo::GetLiteralInfo** méthodes. Vous trouverez plus d’informations sur ces méthodes dans les [IDBInfo](http://msdn.microsoft.com/en-us/3f5ad97f-3fc6-4f21-b691-f6911e4007f3) section de référence du programmeur OLE DB.  
  
|Constante|Valeur| Description|Colonnes de contrainte|  
|--------------|-----------|-----------------|------------------------|  
|**adSchemaAsserts**|0|Retourne les assertions définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes ASSERTIONS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCatalogs**|1|Retourne les attributs physiques associés aux catalogues accessibles à partir du SGBD.<br /><br /> (Ensemble de lignes de catalogues)|CATALOG_NAME|  
|**adSchemaCharacterSets**|2|Retourne les jeux de caractères définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes CHARACTER_SETS)|CHARACTER_SET_CATALOG CHARACTER_SET_SCHEMA CHARACTER_SET_NAME|  
|**adSchemaCheckConstraints**|5|Retourne les contraintes check définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (CHECK_CONSTRAINTS) Ensemble de lignes)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCollations**|3|Retourne les classements de caractères définis dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes de classements)|COLLATION_CATALOG COLLATION_SCHEMA COLLATION_NAME|  
|**adSchemaColumnPrivileges**|13|Retourne les privilèges sur les colonnes des tables définies dans le catalogue qui sont disponibles pour ou accordés par un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMN_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|**adSchemaColumns**|4|Retourne les colonnes des tables (vues comprises) définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMNS)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaColumnsDomainUsage**|11|Retourne les colonnes définies dans le catalogue qui dépendent d’un domaine défini dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes COLUMN_DOMAIN_USAGE)|DOMAIN_CATALOG DOMAIN_SCHEMA NOM_DOMAINE COLUMN_NAME|  
|**adSchemaConstraintColumnUsage**|6|Retourne les colonnes utilisées par les contraintes référentielles, contraintes uniques, les contraintes de validation et les assertions, définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes CONSTRAINT_COLUMN_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaConstraintTableUsage**|7|Retourne les tables qui sont utilisées par les contraintes référentielles, contraintes uniques, les contraintes check et les assertions définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes CONSTRAINT_TABLE_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaCubes**|32|Retourne des informations sur les cubes disponibles dans un schéma (ou le catalogue, si le fournisseur ne prend pas en charge les schémas).<br /><br /> (CUBES ensemble de lignes *)|CATALOG_NAME SCHEMA_NAME CUBE_NAME|  
|**adSchemaDBInfoKeywords**|30|Retourne une liste de mots clés spécifiques au fournisseur.<br /><br /> (IDBInfo::GetKeywords)|\<Aucun >|  
|**adSchemaDBInfoLiterals**|31|Retourne une liste de littéraux spécifiques au fournisseur utilisé dans les commandes de texte.<br /><br /> (IDBInfo::GetLiteralInfo)|\<Aucun >|  
|**adSchemaDimensions**|33|Retourne des informations sur les dimensions d’un cube donné. Il possède une ligne pour chaque dimension.<br /><br /> (Ensemble de lignes de DIMENSIONS)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_NAME DIMENSION_UNIQUE_NAME|  
|**adSchemaForeignKeys**|27|Retourne les colonnes clés étrangères définies dans le catalogue par un utilisateur donné.<br /><br /> (Ensemble de lignes FOREIGN_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|**adSchemaHierarchies**|34|Retourne des informations sur les hiérarchies disponibles dans une dimension.<br /><br /> (Ensemble de lignes de hiérarchies)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_NAME HIERARCHY_UNIQUE_NAME|  
|**adSchemaIndexes**|12|Retourne les index définis dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes INDEXES)|TABLE_CATALOG TABLE_SCHEMA INDEX_NAME TYPE NOM_TABLE|  
|**adSchemaKeyColumnUsage**|8|Retourne les colonnes définies dans le catalogue qui sont limitées en tant que clés par un utilisateur donné.<br /><br /> (Ensemble de lignes KEY_COLUMN_USAGE)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA NOM_TABLE NOM_COLONNE|  
|**adSchemaLevels**|35|Retourne des informations sur les niveaux disponibles dans une dimension.<br /><br /> (Ensemble de lignes de niveaux)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME NOM_NIVEAU LEVEL_UNIQUE_NAME|  
|**adSchemaMeasures**|36|Retourne des informations sur les mesures disponibles.<br /><br /> (Ensemble de lignes de mesures)|CATALOG_NAME SCHEMA_NAME CUBE_NAME MEASURE_NAME MEASURE_UNIQUE_NAME|  
|**adSchemaMembers**|38|Retourne des informations sur les membres disponibles.<br /><br /> (Ensemble de lignes de membres)|Opérateur de catalog_name SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME LEVEL_NUMBER MEMBER_NAME MEMBER_UNIQUE_NAME MEMBER_CAPTION MEMBER_TYPE arborescence. Pour plus d’informations, consultez OLE DB pour OLAP Online Analytical Processing ().|  
|**adSchemaPrimaryKeys**|28|Retourne les colonnes de clés primaires définies dans le catalogue par un utilisateur donné.<br /><br /> (Ensemble de lignes PRIMARY_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME|  
|**adSchemaProcedureColumns**|29|Renvoie des informations sur les colonnes des ensembles de lignes retournées par les procédures.<br /><br /> (Ensemble de lignes PROCEDURE_COLUMNS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA NOM_PROCÉDURE COLUMN_NAME|  
|**adSchemaProcedureParameters**|26|Retourne des informations sur les paramètres et les codes de retour des procédures.<br /><br /> (Ensemble de lignes PROCEDURE_PARAMETERS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|**adSchemaProcedures**|16|Retourne les procédures définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes de procédures)|PROCEDURE_CATALOG PROCEDURE_SCHEMA NOM_PROCÉDURE PROCEDURE_TYPE|  
|**adSchemaProperties**|37|Retourne des informations sur les propriétés disponibles pour chaque niveau de la dimension.<br /><br /> (Ensemble de lignes de propriétés)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME MEMBER_UNIQUE_NAME PROPERTY_TYPE PROPERTY_NAME|  
|**adSchemaProviderSpecific**|-1|Utilisé si le fournisseur définit ses propres requêtes de schéma non standard.|\<Spécifique au fournisseur >|  
|**adSchemaProviderTypes**|22|Retourne les types de données (base) pris en charge par le fournisseur de données.<br /><br /> (Ensemble de lignes PROVIDER_TYPES)|DATA_TYPE BEST_MATCH|  
|**AdSchemaReferentialConstraints**|9|Retourne les contraintes référentielles définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes REFERENTIAL_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaSchemata**|17|Retourne les schémas (objets de base de données) qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes de schéma)|CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|**adSchemaSQLLanguages**|18|Retourne les niveaux de conformité, les options et les dialectes pris en charge par les données de traitement d’implémentation SQL définies dans le catalogue.<br /><br /> (Ensemble de lignes SQL_LANGUAGES)|\<Aucun >|  
|**adSchemaStatistics**|19|Retourne les statistiques définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes de statistiques)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaTableConstraints**|10|Retourne les contraintes de table définies dans le catalogue qui sont détenues par un utilisateur donné.<br /><br /> (Ensemble de lignes TABLE_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|**adSchemaTablePrivileges**|14|Retourne les privilèges sur les tables définies dans le catalogue qui sont disponibles pour ou accordés par un utilisateur donné.<br /><br /> (Ensemble de lignes TABLE_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|**adSchemaTables**|20|Retourne les tables (vues comprises) définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes de TABLES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|**adSchemaTranslations**|21|Retourne les traductions de caractères définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes de traductions)|TRANSLATION_CATALOG TRANSLATION_SCHEMA TRANSLATION_NAME|  
|**adSchemaTrustees**|39|Réservé pour un usage ultérieur.||  
|**adSchemaUsagePrivileges**|15|Retourne les privilèges USAGE sur les objets définis dans le catalogue et qui sont disponibles pour ou accordés par un utilisateur donné.<br /><br /> (Ensemble de lignes USAGE_PRIVILEGES)|OBJECT_CATALOG OBJECT_SCHEMA NOM_OBJET OBJECT_TYPE GRANTOR BÉNÉFICIAIRE|  
|**adSchemaViewColumnUsage**|24|Retourne les colonnes sur lesquelles les tables affichées, définies dans le catalogue et détenues par un utilisateur donné, sont dépendants.<br /><br /> (Ensemble de lignes VIEW_COLUMN_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
|**adSchemaViews**|23|Retourne les vues définies dans le catalogue qui sont accessibles à un utilisateur donné.<br /><br /> (Ensemble de lignes des vues)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaViewTableUsage**|25|Retourne les tables sur laquelle tables affichées, définies dans le catalogue et détenues par un utilisateur donné, sont dépendants.<br /><br /> (Ensemble de lignes VIEW_TABLE_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.Schema.ASSERTS|  
|AdoEnums.Schema.CATALOGS|  
|AdoEnums.Schema.CHARACTERSETS|  
|AdoEnums.Schema.CHECKCONSTRAINTS|  
|AdoEnums.Schema.COLLATIONS|  
|AdoEnums.Schema.COLUMNPRIVILEGES|  
|AdoEnums.Schema.COLUMNS|  
|AdoEnums.Schema.COLUMNSDOMAINUSAGE|  
|AdoEnums.Schema.CONSTRAINTCOLUMNUSAGE|  
|AdoEnums.Schema.CONSTRAINTTABLEUSAGE|  
|AdoEnums.Schema.CUBES|  
|AdoEnums.Schema.DBINFOKEYWORDS|  
|AdoEnums.Schema.DBINFOLITERALS|  
|AdoEnums.Schema.DIMENSIONS|  
|AdoEnums.Schema.FOREIGNKEYS|  
|AdoEnums.Schema.HIERARCHIES|  
|AdoEnums.Schema.INDEXES|  
|AdoEnums.Schema.KEYCOLUMNUSAGE|  
|AdoEnums.Schema.LEVELS|  
|AdoEnums.Schema.MEASURES|  
|AdoEnums.Schema.MEMBERS|  
|AdoEnums.Schema.PRIMARYKEYS|  
|AdoEnums.Schema.PROCEDURECOLUMNS|  
|AdoEnums.Schema.PROCEDUREPARAMETERS|  
|AdoEnums.Schema.PROCEDURES|  
|AdoEnums.Schema.PROPERTIES|  
|AdoEnums.Schema.PROVIDERSPECIFIC|  
|AdoEnums.Schema.PROVIDERTYPES|  
|AdoEnums.Schema.REFERENTIALCONTRAINTS|  
|AdoEnums.Schema.SCHEMATA|  
|AdoEnums.Schema.SQLLANGUAGES|  
|AdoEnums.Schema.STATISTICS|  
|AdoEnums.Schema.TABLECONSTRAINTS|  
|AdoEnums.Schema.TABLEPRIVILEGES|  
|AdoEnums.Schema.TABLES|  
|AdoEnums.Schema.TRANSLATIONS|  
|AdoEnums.Schema.TRUSTEES|  
|AdoEnums.Schema.USAGEPRIVILEGES|  
|AdoEnums.Schema.VIEWCOLUMNUSAGE|  
|AdoEnums.Schema.VIEWS|  
|AdoEnums.Schema.VIEWTABLEUSAGE|  
  
## <a name="applies-to"></a>S'applique à  
 [OpenSchema, méthode](../../../ado/reference/ado-api/openschema-method.md)
