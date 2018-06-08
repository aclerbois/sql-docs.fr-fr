---
title: Ensemble de lignes DISCOVER_LOCATIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d65e4ea55f956ce47632cd11a4e6dc5f8cfea377
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverlocations-rowset"></a>Ensemble de lignes DISCOVER_LOCATIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne des informations sur le contenu d'un fichier de sauvegarde. Vous devez disposer de l'autorisation requise pour accéder à l'emplacement du fichier de sauvegarde.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_LOCATIONS** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Restriction| Description|  
|-----------------|--------------------|-----------------|-----------------|  
|**LOCATION_BACKUP_FILE_PATHNAME**|**DBTYPE_WSTR**|Obligatoire, voir ci-dessous.|Emplacement du fichier de sauvegarde.|  
|**LOCATION_PARTITION_OBJECTPATH**|**DBTYPE_WSTR**||Chemin d'accès à la partition en fonction du dossier de données.|  
|**LOCATION_PARTITION_DATASOURCEID**|**DBTYPE_WSTR**||ID de source de données utilisé pour le traitement de la partition.|  
|**LOCATION_PARTITION_DATASOURCENAME**|**DBTYPE_WSTR**||Nom de la source de données utilisée pour le traitement.|  
|**LOCATION_PARTITION_NAME**|**DBTYPE_WSTR**||Nom de la partition.|  
|**LOCATION_PARTITION_SIZE**|**DBTYPE_WSTR**||Taille approximative de la partition.|  
|**LOCATION_CONNECTION_STRING**|**DBTYPE_WSTR**||Chaîne de connexion pour la source de données utilisée dans le traitement.|  
|**LOCATION_PARTITION_FOLDER**|**DBTYPE_WSTR**||Emplacement d'origine de cette partition lorsque le fichier de sauvegarde a été généré.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_LOCATIONS** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**LOCATION_BACKUP_FILE_PATHNAME**|**DBTYPE_WSTR**|Requis|  
|**LOCATION_PASSWORD PF_DBTYPE**|**DBTYPE_WSTR**|Obligatoire s'il a été spécifié pendant la sauvegarde. Cette restriction n'est pas utilisée pour limiter les lignes retournées. Elle est utilisée pour fournir le mot de passe en vue d'accéder à l'emplacement.|  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|a07ccd92-8148-11d0-87bb-00c04fc33942|  
|ADOMDNAME|Emplacements|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
