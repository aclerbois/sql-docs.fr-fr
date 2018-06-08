---
title: Ensemble de lignes MDSCHEMA_MEMBERS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ffe79581b338941ab4fcd379d7da48ce33028633
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemamembers-rowset"></a>Ensemble de lignes MDSCHEMA_MEMBERS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les membres d'une base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_MEMBERS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**||Nom de la base de données à laquelle ce membre appartient.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**||Nom du schéma auquel ce membre appartient.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Nom du cube auquel ce membre appartient.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique de la dimension à laquelle ce membre appartient.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique de la hiérarchie à laquelle ce membre appartient.|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du niveau auquel ce membre appartient.|  
|**LEVEL_NUMBER**|**DBTYPE_UI4**||Distance du membre par rapport à la racine de la hiérarchie. Le niveau de la racine est égal à zéro (0).|  
|**MEMBER_ORDINAL**|**DBTYPE_UI4**||(Déconseillée) Retourne toujours **0**.|  
|**MEMBER_NAME**|**DBTYPE_WSTR**||Nom du membre.|  
|**MEMBER_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du membre.|  
|**MEMBER_TYPE**|**DBTYPE_I4**||Type du membre :<br /><br /> **MDMEMBER_TYPE_UNKNOWN** (**0**)<br /><br /> **MDMEMBER_TYPE_REGULAR** (**1**)<br /><br /> **MDMEMBER_TYPE_ALL** (**2**)<br /><br /> **MDMEMBER_TYPE_MEASURE** (**3**)<br /><br /> **MDMEMBER_TYPE_FORMULA** (**4**)<br /><br /> <br /><br /> Notez que <br />                    **MDMEMBER_TYPE_FORMULA**est prioritaire sur **MDMEMBER_TYPE_MEASURE**. Par exemple, s’il existe un membre de formule (calculé) sur la dimension de mesures, il est répertorié comme étant **MDMEMBER_TYPE_FORMULA**.|  
|**MEMBER_GUID**|**DBTYPE_GUID**||GUID du membre. **NULL** si n’existe aucun GUID.|  
|**MEMBER_CAPTION**|**DBTYPE_WSTR**||Étiquette ou légende associée au membre. Principalement utilisée à des fins d'affichage. Si une légende n’existe pas, **MEMBER_NAME** est retourné.|  
|**CHILDREN_CARDINALITY**|**DBTYPE_UI4**||Nombre d'enfants de ce membre. Ce nombre peut être une estimation ; il ne doit donc pas être considéré comme le nombre exact. Les fournisseurs doivent renvoyer la meilleure estimation possible.|  
|**PARENT_LEVEL**|**DBTYPE_UI4**||Distance du parent du membre par rapport au niveau racine de la hiérarchie. Le niveau de la racine est égal à zéro (0).|  
|**PARENT_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique du parent du membre. **NULL** est retournée pour tout membre situé au niveau de la racine.|  
|**PARENT_COUNT**|**DBTYPE_UI4**||Nombre de parents de ce membre.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Cette colonne retourne toujours un **NULL** valeur.<br /><br /> Cette colonne existe pour la compatibilité descendante|  
|**EXPRESSION**|**DBTYPE_WSTR**||L’expression pour les calculs, si le membre est de type **MDMEMBER_TYPE_FORMULA**.|  
|**MEMBER_KEY**|**DBTYPE_WSTR**||Valeur de la colonne clé du membre. Retourne **NULL** si le membre a une clé composite.|  
|**IS_PLACEHOLDERMEMBER**|**DBTYPE_BOOL**||Valeur booléenne qui indique si un membre est un membre d'espace réservé pour une position vide dans une hiérarchie de dimension.<br /><br /> Il n’est valide uniquement si le **MDX Compatibility** propriété a été définie à 2.|  
|**IS_DATAMEMBER**|**DBTYPE_BOOL**||Valeur booléenne indiquant si le membre est un membre de données.<br /><br /> Retourne True si le membre est un membre de données.|  
|**ÉTENDUE**|**DBTYPE_I4**||Étendue du membre. Le membre peut être un membre calculé de session ou un membre calculé global. La colonne retourne **NULL** pour les membres non calculés. Les valeurs possibles pour cette colonne sont les suivantes :<br /><br /> MDMEMBER_SCOPE_GLOBAL=1<br /><br /> MDMEMBER_SCOPE_SESSION=2|  
|**Zéro ou plusieurs des colonnes supplémentaires**|**DBTYPE_UI2**||Aucune propriété n'est retournée si les membres peuvent être retournés de plusieurs niveaux. Par exemple, si l’opérateur d’arborescence est **PARENT** et **SELF** pour une hiérarchie non parent enfant, aucune propriété de membre n’est retournées.<br /><br /> Ceci s'applique aux hiérarchies déséquilibrées où les opérateurs d'arborescence peuvent retourner des membres de niveaux différents (par exemple, si le niveau précédent contient des trous et qu'un parent sur les membres est demandé).|  
  
 L’ensemble de lignes est trié sur **CATALOG_NAME**, **nom_schéma**, **CUBE_NAME**, **DIMENSION_UNIQUE_NAME**, **HIERARCHY_UNIQUE_NAME**, **LEVEL_UNIQUE_NAME**, **LEVEL_NUMBER**, **MEMBER_ORDINAL**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_MEMBERS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**LEVEL_NUMBER**|**DBTYPE_UI4**|Ce paramètre est facultatif.|  
|**MEMBER_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEMBER_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEMBER_CAPTION**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEMBER_TYPE**|**DBTYPE_I4**|Ce paramètre est facultatif.|  
|**TREE_OP**|**DBTYPE_I4**|(Facultatif) S'applique uniquement à un membre unique :<br /><br /> **MDTREEOP_ANCESTORS** (**0 x 20**) renvoie tous les ancêtres.<br /><br /> **MDTREEOP_CHILDREN** (**0 x 01**) retourne uniquement les enfants immédiats.<br /><br /> **MDTREEOP_SIBLINGS** (**0 x 02**) renvoie les membres du même niveau.<br /><br /> **MDTREEOP_PARENT** (**0 x 04**) retourne uniquement le parent immédiat.<br /><br /> **MDTREEOP_SELF** (**0 x 08**) se retourne lui-même dans la liste des lignes retournées.<br /><br /> **MDTREEOP_DESCENDANTS** (**0 x 10**) retourne tous les descendants.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
