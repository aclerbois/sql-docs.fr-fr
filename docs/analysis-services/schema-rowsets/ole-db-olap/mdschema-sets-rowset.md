---
title: Ensemble de lignes MDSCHEMA_SETS | Documents Microsoft
ms.custom: 
ms.date: 03/06/2017
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: 
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
apiname: MDSCHEMA_SETS
apitype: NA
applies_to: SQL Server 2016 Preview
helpviewer_keywords: MDSCHEMA_SETS rowset
ms.assetid: abb00dc0-2b83-48d6-b2ba-6615c1488d06
caps.latest.revision: "37"
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: d861c23e40464535fbde4b666c2e1429f65c772c
ms.sourcegitcommit: f486d12078a45c87b0fcf52270b904ca7b0c7fc8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
---
# <a name="mdschemasets-rowset"></a>Ensemble de lignes MDSCHEMA_SETS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]Décrit les ensembles actuellement définis dans une base de données, y compris les ensembles de niveau session.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_SETS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Description|  
|-----------------|--------------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Nom de la base de données.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Non pris en charge.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Nom du cube.|  
|**SET_NAME**|**DBTYPE_WSTR**|Le nom du jeu de données, comme spécifié dans le **CREATE SET** instruction.|  
|**ÉTENDUE**|**DBTYPE_I4**|Étendue de l'ensemble :<br /><br /> **MDSET_SCOPE_GLOBAL** (**1**)<br /><br /> **MDSET_SCOPE_SESSION** (**2**)|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Non pris en charge.|  
|**EXPRESSION**|**DBTYPE_WSTR**|Expression pour l'ensemble.|  
|**DIMENSIONS**|**DBTYPE_WSTR**|Liste des hiérarchies incluses dans l'ensemble, délimitée par des virgules.|  
|**SET_CAPTION**|**DBTYPE_WSTR**|Étiquette ou légende associée à l'ensemble. L'étiquette ou la légende est utilisée principalement à des fins d'affichage.|  
|**SET_DISPLAY_FOLDER**|**DBTYPE_WSTR**|Chaîne qui identifie le chemin d'accès du dossier d'affichage que l'application cliente utilise pour afficher le jeu. Le séparateur de niveau de dossier est défini par l'application cliente. Pour les outils et les clients fournis par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], la barre oblique inverse (\\) est le séparateur de niveau. Pour fournir plusieurs dossiers d’affichage, utilisez un point-virgule ( ;) pour séparer les dossiers.|  
|**SET_EVALUATION_CONTEXT**|**DBTYPE_I4**|Contexte de l'ensemble. L'ensemble peut être statique ou dynamique. Les valeurs possibles pour cette colonne sont les suivantes :<br /><br /> MDSET_RESOLUTION_STATIC=1<br /><br /> MDSET_RESOLUTION_DYNAMIC=2|  
  
 L'ensemble de lignes est trié selon **CATALOG_NAME**, **SCHEMA_NAME**, **CUBE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_SETS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Facultatif.|  
|**SET_NAME**|**DBTYPE_WSTR**|Facultatif.|  
|**ÉTENDUE**|**DBTYPE_I4**|Facultatif.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**|Facultatif.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|Facultatif.<br /><br /> Remarque : Une seule hiérarchie peut être incluse, et seuls les jeux nommés dont les hiérarchies correspondent exactement à la restriction sont retournés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma OLE DB pour OLAP](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
