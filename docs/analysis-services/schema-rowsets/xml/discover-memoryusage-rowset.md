---
title: Ensemble de lignes DISCOVER_MEMORYUSAGE | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 37600d1c99353cd31c1b88bfc6a2893ab968376e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discovermemoryusage-rowset"></a>DISCOVER_MEMORYUSAGE, ensemble de lignes
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne les statistiques DISCOVER_MEMORYUSAGE pour plusieurs objets alloués par le serveur.  
  
> [!WARNING]  
>  Cet ensemble de lignes peut produire des jeux de résultats très volumineux. Si les résultats ne peuvent pas être affichés car ils nécessitent davantage de mémoire d'affichage que ne le permet SQL Server Management Studio, ils sont écrits dans un fichier temporaire, dans l'emplacement par défaut suivant :  
>   
>  «\<lecteur > : \Users\\< nom d’utilisateur\>\AppData\Local\Temp\\< fileID\>.xml ».  
  
 **S'applique à :** modèles tabulaires, modèles multidimensionnels  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DISCOVER_MEMORYUSAGE** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Restriction| Description|  
|-----------------|--------------------|-----------------|-----------------|  
|**MemoryID**|**DBTYPE_UI8**||Numéro identifiant la mémoire.|  
|**MemoryName**|**DBTYPE_WSTR**||Nom de l'objet qui possède la mémoire.|  
|**SPID**|**DBTYPE_UI4**|Oui|Session qui a alloué la mémoire. Zéro indique la mémoire non liée à une session spécifique.|  
|**CreationTime**|**DBTYPE_DBTIMESTAMP**||Soit l'« heure de création de l'objet », soit l'« l'heure d'allocation de la mémoire ».|  
|**BaseObjectType**|**DBTYPE_UI4**|Oui|Numéro qui décrit le type de l'objet. Les objets avec le même BaseObjectType sont de même type.|  
|**MemoryUsed**|**DBTYPE_UI8**|Oui|Il s'agit de la taille actuelle de l'objet, qui peut être inférieure à la mémoire allouée à l'utilisation de l'objet.|  
|**MemoryAllocated**|**DBTYPE_UI8**||Quantité de mémoire allouée à l'utilisation de l'objet, qui peut être supérieure à la quantité de mémoire réellement utilisée par l'objet.|  
|**MemoryAllocBase**|**DBTYPE_UI8**||Octets initialement alloués pour l'objet lui-même (à l'exception des allocations supplémentaires pour le contenu de l'objet).|  
|**MemoryAllocFromAlloc**|**DBTYPE_UI8**||Mémoire allouée au contenu de cet objet.|  
|**valeur d’elementCount**|**DBTYPE_UI4**||Pour un objet conteneur, il s'agit du nombre d'objets contenus par cet objet.|  
|**Réductible**|**DBTYPE_BOOL**|Oui|Valeur booléenne qui indique si la mémoire est réductible (peut être supprimée en cas de sollicitation de la mémoire). Si la valeur est true, la mémoire est réductible ; si la valeur est false, la mémoire n'est pas réductible.|  
|**ObjectParentPath**|**DBTYPE_WSTR**||Chaîne identifiant le chemin complet de cet objet.|  
|**ObjectID**|**DBTYPE_WSTR**||Chaîne identifiant l'objet. Le chemin d’accès complet de cet objet est représenté par la chaîne : (ObjectParentPath + '.' + ObjectId).|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|A07CCD21-8148-11D0-87BB-00C04FC33942|  
|ADOMDNAME|MemoryUsage|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
