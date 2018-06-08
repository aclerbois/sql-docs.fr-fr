---
title: Ensembles de lignes de schéma OLE DB | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3e0e7563eb925e9974dcca5bd1d02c54b01f38d8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="ole-db-schema-rowsets"></a>Ensembles de lignes des schémas OLE DB
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les ensembles de lignes des schémas OLE DB suivants sont pris en charge par le fournisseur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] XML for Analysis (XMLA). Utilisez le **DISCOVER_ENUMERATORS** ensemble de lignes avec le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode permettant de vérifier si un fournisseur de source de données particulière prend en charge un ensemble de lignes.  
  
 Vous pouvez également obtenir des informations détaillées sur ces ensembles de lignes en recherchant la rubrique « Ensembles de lignes de schéma » dans la section Guide de référence du programmeur OLE DB de la Bibliothèque MSDN® sur le site Web [!INCLUDE[msCoName](../../../includes/msconame-md.md)].  
  
 Le tableau suivant décrit cet ensemble de lignes de schéma.  
  
|Ensemble de lignes| Description|  
|------------|-----------------|  
|**DBSCHEMA_ASSERTIONS**|Identifie les assertions qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|[Ensemble de lignes DBSCHEMA_CATALOGS](../../../analysis-services/schema-rowsets/ole-db/dbschema-catalogs-rowset.md) <sup>1</sup>|Identifie les attributs physiques associés aux catalogues qui sont accessibles à partir du système de gestion de base de données (SGBD). Parfois, pour certains systèmes (tels que [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Access), il n'existe qu'un seul catalogue. Pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], cet ensemble de lignes énumère tous les catalogues (bases de données) définis dans la base de données système.|  
|**DBSCHEMA_CHARACTER_SETS**|Identifie les jeux de caractères qui sont définis dans le catalogue et auxquels un utilisateur donné peut accéder.|  
|**DBSCHEMA_CHECK_CONSTRAINTS**|Identifie les contraintes de validation qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_CHECK_CONSTRAINTS_BY_TABLE**|Identifie les contraintes de validation d'une table donnée, définies dans un catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_COLLATIONS**|Identifie les classements de caractères qui sont définis dans le catalogue et auxquels un utilisateur donné peut accéder.|  
|**DBSCHEMA_COLUMN_DOMAIN_USAGE**|Identifie les colonnes définies dans le catalogue qui dépendent d'un domaine défini dans le catalogue et qui sont détenues par un utilisateur donné.|  
|**DBSCHEMA_COLUMN_PRIVILEGES**|Identifie les privilèges sur les colonnes de tables qui sont définis dans le catalogue et mis à disposition d'un utilisateur ou accordés par celui-ci.|  
|[Ensemble de lignes DBSCHEMA_COLUMNS](../../../analysis-services/schema-rowsets/ole-db/dbschema-columns-rowset.md) <sup>1</sup>|Fournit des informations de colonne pour toutes les colonnes qui répondent aux critères de restriction indiqués.|  
|**DBSCHEMA_CONSTRAINT_COLUMN_USAGE**|Identifie les colonnes utilisées par les contraintes référentielles, les contraintes uniques, les contraintes de validation et les assertions, et qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_CONSTRAINT_TABLE_USAGE**|Identifie les tables utilisées par les contraintes référentielles, les contraintes uniques, les contraintes de validation et les assertions, et qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_FOREIGN_KEYS**|Identifie les colonnes clés étrangères définies dans le catalogue par un utilisateur donné. Cet ensemble de lignes de schéma repose sur plusieurs vues de schémas ISO pour offrir davantage de facilité au programmeur non-SQL. Si la prise en charge, cet ensemble de lignes de schéma doit être synchronisé avec les vues ISO associées (**REFERENTIAL_CONSTRAINTS** et **CONSTRAINT_COLUMN_USAGE**).|  
|**DBSCHEMA_INDEXES**|Identifie les index qui sont définis dans le catalogue et détenus par un utilisateur donné.|  
|**DBSCHEMA_KEY_COLUMN_USAGE**|Identifie les colonnes qui sont définies dans le catalogue et contraintes en tant que clés par un utilisateur donné.|  
|**DBSCHEMA_PRIMARY_KEYS**|Identifie les colonnes clés primaires définies dans le catalogue par un utilisateur donné. Cet ensemble de lignes de schéma repose sur une vue de schéma ISO pour offrir davantage de facilité au programmeur non-SQL. Si la prise en charge, cet ensemble de lignes de schéma doit être synchronisé avec la vue ISO associée (**CONSTRAINT_COLUMN_USAGE**).|  
|**DBSCHEMA_PROCEDURE_COLUMNS**|Renvoie des informations sur les colonnes des ensembles de lignes retournées par les procédures.|  
|**DBSCHEMA_PROCEDURE_PARAMETERS**|Retourne des informations sur les paramètres et les codes de retour des procédures.|  
|**DBSCHEMA_PROCEDURES**|Identifie les procédures qui sont définies dans le catalogue et détenues par un utilisateur donné. Il s'agit d'une extension OLE DB.|  
|[Lignes de schéma DBSCHEMA_PROVIDER_TYPES](../../../analysis-services/schema-rowsets/ole-db/dbschema-provider-types-rowset.md) <sup>1</sup>|Identifie les types de données (de base) pris en charge par le fournisseur de données.|  
|**DBSCHEMA_REFERENTIAL_CONSTRAINTS**|Identifie les contraintes référentielles qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_SCHEMATA**|Identifie les schémas détenus par un utilisateur donné.|  
|**DBSCHEMA_SQL_LANGUAGES**|Identifie les dialectes, les options et les niveaux de conformité pris en charge par l'implémentation SQL traitant les données définies dans le catalogue.|  
|**DBSCHEMA_STATISTICS**|Identifie les statistiques qui sont définies dans le catalogue et détenues par un utilisateur donné.<br /><br /> Cette table n’est pas associée à la **TABLE_STATISTICS** ensemble de lignes.|  
|**DBSCHEMA_TABLE_CONSTRAINTS**|Identifie les contraintes de table qui sont définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_TABLE_PRIVILEGES**|Identifie les privilèges sur les tables qui sont définis dans le catalogue et mis à disposition d'un utilisateur ou accordés par celui-ci.|  
|**DBSCHEMA_TABLE_STATISTICS**|Décrit le jeu de statistiques disponible sur les tables du fournisseur.<br /><br /> Cet ensemble de lignes n’est pas liée à la **statistiques** ensemble de lignes.|  
|[Ensemble de lignes DBSCHEMA_TABLES](../../../analysis-services/schema-rowsets/ole-db/dbschema-tables-rowset.md) <sup>1</sup>|Identifie les groupes de mesures et les dimensions exposés en tant que tables dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].|  
|**DBSCHEMA_TABLES_INFO** <sup>1</sup>|Identifie les tables (vues comprises) qui sont définies dans le catalogue et auxquelles un utilisateur donné peut accéder.|  
|**DBSCHEMA_TRANSLATIONS**|Identifie les conversions de caractères qui sont définies dans le catalogue et auxquelles un utilisateur donné peut accéder.|  
|**DBSCHEMA_TRUSTEE**|Énumère les tiers de confiance d'une source de données.|  
|**DBSCHEMA_USAGE_PRIVILEGES**|Identifie la **utilisation** des privilèges sur les objets qui sont définies dans le catalogue et qui sont disponibles à ou accordés par un utilisateur donné.|  
|**DBSCHEMA_VIEW_COLUMN_USAGE**|Identifie les vues qui sont définies dans le catalogue et auxquelles un utilisateur donné peut accéder.|  
|**DBSCHEMA_VIEW_TABLE_USAGE**|Identifie les tables dont dépendent les tables affichées, définies dans le catalogue et détenues par un utilisateur donné.|  
|**DBSCHEMA_VIEWS**|Identifie les vues qui sont définies dans le catalogue et auxquelles un utilisateur donné peut accéder.|  
  
 <sup>1</sup> indique les ensembles de lignes de schéma pris en charge par le fournisseur de source de données MSOLAP pour le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] fournisseur XMLA.  
  
## <a name="see-also"></a>Voir aussi  
 [Ensemble de lignes DISCOVER_ENUMERATORS](../../../analysis-services/schema-rowsets/xml/discover-enumerators-rowset.md)   
 [Ensembles de lignes de schéma de Analysis Services](../../../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md)  
  
  
