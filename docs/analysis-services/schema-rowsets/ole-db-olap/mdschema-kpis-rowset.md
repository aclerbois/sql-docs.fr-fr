---
title: Ensemble de lignes MDSCHEMA_KPIS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: be7d4519be7f113a3ddfd058f9e890ba71409b9d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemakpis-rowset"></a>Ensemble de lignes MDSCHEMA_KPIS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les indicateurs de performance clés (KPI) dans une base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_KPIS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Base de données source.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Non pris en charge.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Cube parent de l'indicateur de performance clé.|  
|**MEASUREGROUP_NAME**|**DBTYPE_WSTR**|Groupe de mesures associé pour l'indicateur de performance clé.<br /><br /> Vous pouvez utiliser cette colonne pour déterminer la dimensionnalité de l'indicateur de performance clé. Si «**\<NULL >**», l’indicateur de performance clé sera dimensionné par tous les groupes de mesures.<br /><br /> La valeur par défaut est «**\<NULL >**».|  
|**NOM_ICP**|**DBTYPE_WSTR**|Nom de l'indicateur de performance clé.|  
|**KPI_CAPTION**|**DBTYPE_WSTR**|Étiquette ou légende associée à l'indicateur de performance clé. Principalement utilisée à des fins d'affichage. Si une légende n’existe pas, **nom_icp** est retourné.|  
|**KPI_DESCRIPTION**|**DBTYPE_WSTR**|Description explicite de l'indicateur de performance clé.|  
|**KPI_DISPLAY_FOLDER**|**DBTYPE_WSTR**|Chaîne qui identifie le chemin d'accès du dossier d'affichage que l'application cliente utilise pour afficher le membre. Le séparateur de niveau de dossier est défini par l'application cliente. Pour les outils et les clients fournis par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], la barre oblique inverse (\\) est le séparateur de niveau. Pour fournir plusieurs dossiers d’affichage, utilisez un point-virgule ( ;) pour séparer les dossiers.|  
|**KPI_VALUE**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de mesures pour la valeur de l'indicateur de performance clé.|  
|**KPI_GOAL**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de mesures pour l'objectif de l'indicateur de performance clé.<br /><br /> Retourne **NULL** s’il n’existe aucun objectif défini.|  
|**KPI_STATUS**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de mesures pour l'état de l'indicateur de performance clé.<br /><br /> Retourne **NULL** s’il n’existe aucun état défini.|  
|**KPI_TREND**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de mesures pour la tendance de l'indicateur de performance clé.<br /><br /> Retourne **NULL** si tendance n’est pas défini.|  
|**KPI_STATUS_GRAPHIC**|**DBTYPE_WSTR**|Représentation graphique par défaut de l'indicateur de performance clé.|  
|**KPI_TREND_GRAPHIC**|**DBTYPE_WSTR**|Représentation graphique par défaut de l'indicateur de performance clé.|  
|**KPI_WEIGHT**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de mesures pour le poids de l'indicateur de performance clé.|  
|**KPI_CURRENT_TIME_MEMBER**|**DBTYPE_WSTR**|Nom unique du membre dans la dimension de temps qui définit le contexte temporel de l'indicateur de performance clé.<br /><br /> Retourne **NULL** si n’existe aucun membre de temps défini.|  
|**KPI_PARENT_KPI_NAME**|**DBTYPE_WSTR**|Nom de l'indicateur de performance clé parent.|  
|**ÉTENDUE**|**DBTYPE_I4**|Étendue de l'indicateur de performance clé. L'indicateur de performance clé peut être un indicateur de performance clé de session ou un indicateur de performance clé global.<br /><br /> Les valeurs possibles pour cette colonne sont les suivantes :<br /><br /> MDKPI_SCOPE_GLOBAL=1<br /><br /> MDKPI_SCOPE_SESSION=2|  
|**ANNOTATIONS**|**DBTYPE_WSTR**|(Facultatif) Jeu de notes, au format XML.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_KPIS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_ICP**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de **1**. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> **1** CUBE<br /><br /> **2** DIMENSION|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
