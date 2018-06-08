---
title: Ensemble de lignes MDSCHEMA_FUNCTIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7e6786f53ddc48c457857f58128c3f16959420d5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemafunctions-rowset"></a>Ensemble de lignes MDSCHEMA_FUNCTIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les fonctions disponibles pour les applications clientes connectées à la base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_FUNCTIONS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**NOM DE LA FONCTION**|**DBTYPE_WSTR**|Nom de la fonction.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description de la fonction.|  
|**PARAMETER_LIST**|**DBTYPE_WSTR**|Liste séparée par des virgules de paramètres formatés en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic. Par exemple, un paramètre de nom peut avoir la forme d'une chaîne.|  
|**RETURN_TYPE**|**DBTYPE_I4**|Le **VARTYPE** du type de données de retour de la fonction.|  
|**ORIGINE**|**DBTYPE_I4**|Origine de la fonction :<br /><br /> 1 pour les fonctions MDX.<br /><br /> 2 pour les fonctions définies par l'utilisateur.|  
|**NOM_INTERFACE**|**DBTYPE_WSTR**|Nom de l'interface pour les fonctions définies par l'utilisateur.<br /><br /> Nom de groupe pour les fonctions MDX (Multidimensional Expressions).|  
|**NOM_LIBRAIRIE**|**DBTYPE_WSTR**|Nom de la bibliothèque de types pour les fonctions définies par l'utilisateur. **NULL** pour les fonctions MDX.|  
|**NOM_DLL**|**DBTYPE_WSTR**|(Facultatif) Nom de la classe dans l'assembly qui implémente la fonction définie par l'utilisateur.<br /><br /> Retourne **VT_NULL** pour les fonctions MDX.|  
|**HELP_FILE**|**DBTYPE_WSTR**|(Facultatif) Nom du fichier qui contient la documentation d'aide pour la fonction définie par l'utilisateur.<br /><br /> Retourne **VT_NULL** pour les fonctions MDX.|  
|**HELP_CONTEXT**|**DBTYPE_I4**|(Facultatif) Retourne l'ID du contexte d'aide pour cette fonction.|  
|**OBJECT**|**DBTYPE_WSTR**|(Facultatif) Nom générique de la classe de l'objet auquel s'applique une propriété. Par exemple, l’ensemble de lignes correspondant à l’élément < nom_niveau >. Membres de fonction retourne «**niveau**».<br /><br /> Retourne **VT_NULL** pour les fonctions définies par l’utilisateur, ou les fonctions MDX pas une propriété.|  
|**LÉGENDE**|**DBTYPE_WSTR**|Légende d'affichage pour la fonction.|  
  
 L’ensemble de lignes est trié sur **origine**, **nom_interface**, **nom_fonction**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_FUNCTIONS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**NOM_LIBRAIRIE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_INTERFACE**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM DE LA FONCTION**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**ORIGINE**|**DBTYPE_I4**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
