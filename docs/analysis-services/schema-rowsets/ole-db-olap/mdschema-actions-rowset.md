---
title: Ensemble de lignes MDSCHEMA_ACTIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0b2e2967b1b01c56801235b88220ea13be3b8e65
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemaactions-rowset"></a>Ensemble de lignes MDSCHEMA_ACTIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les actions qui peuvent être disponibles pour les applications clientes.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_ACTIONS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**||Nom de la base de données.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**||Non pris en charge. Contient toujours **VT_NULL**.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Nom du cube auquel appartient cette action.|  
|**ACTION_NAME**|**DBTYPE_WSTR**||Nom de cette action.|  
|**ACTION_TYPE**|**DBTYPE_I4**||Bitmap utilisé pour spécifier la méthode de déclenchement de l'action. Le fichier Msmd.h définit les constantes de valeur binaire suivantes pour cette image bitmap :<br /><br /> **MDACTION_TYPE_URL** (**0 x 01**)<br /><br /> **MDACTION_TYPE_HTML** (**0 x 02**)<br /><br /> **MDACTION_TYPE_STATEMENT** (**0 x 04**)<br /><br /> **MDACTION_TYPE_DATASET** (**0 x 08**)<br /><br /> **MDACTION_TYPE_ROWSET** (**0 x 10**)<br /><br /> **MDACTION_TYPE_COMMANDLINE** (**0 x 20**)<br /><br /> **MDACTION_TYPE_PROPRIETARY** (**0 x 40**)<br /><br /> **MDACTION_TYPE_REPORT** (**0 x 80**)<br /><br /> **MDACTION_TYPE_DRILLTHROUGH** (**0 x 100**)|  
|**COORDONNÉES**|**DBTYPE_WSTR**||Expression MDX (Multidimensional Expressions) qui spécifie un objet ou une coordonnée dans l'espace multidimensionnel dans lequel l'action est effectuée. L'application cliente est chargée de fournir la veleur de cette colonne de restriction.<br /><br /> Le **CORDINATE** doit correspondre à l’objet spécifié dans **COORDINATE_TYPE**.|  
|**COORDINATE_TYPE**|**DBTYPE_I4**||Une image bitmap qui spécifie comment la **coordonner** colonne de restriction est interprété. Le fichier Msmd.h définit les constantes de valeur binaire suivantes pour cette image bitmap :<br /><br /> **MDACTION_COORDINATE_CUBE** (**1**)<br /><br /> **MDACTION_COORDINATE_DIMENSION** (**2**) : fait référence aux hiérarchies de dimensions.<br /><br /> **MDACTION_COORDINATE_LEVEL** (**3**)<br /><br /> **MDACTION_COORDINATE_MEMBER** (**4**)<br /><br /> **MDACTION_COORDINATE_SET** (**5**)<br /><br /> **MDACTION_COORDINATE_CELL** (**6**)|  
|**ACTION_CAPTION**|**DBTYPE_WSTR**||Nom de l'action si aucune légende n'a été spécifiée et si aucune traduction n'a été spécifiée dans le DDL.<br /><br /> Si une légende ou des traductions ont été spécifiées, et **CaptionIsMDX** a la valeur false, une des chaînes suivantes :<br /><br /> -Traduction pour la langue appropriée.<br /><br /> -Légende spécifiée si aucune traduction n’a été trouvée pour la langue spécifiée.<br /><br /> -Le nom de l’action si aucune traduction n’a été trouvée et la légende n’a pas été spécifiée dans le DDL.<br /><br /> Si une légende ou des traductions ont été spécifiées, et **CaptionIsMDX** est true, la chaîne qui résulte de la recherche de la traduction appropriée pour la langue spécifiée ou la traduction spécifiée dans la légende DDL et le calcul de la formule pour créer la chaîne.<br /><br /> Si l'action a été spécifiée dans un script MDX, il n'y a pas de traductions et la légende est toujours traitée comme une expression MDX.|  
|**DESCRIPTION**|**DBTYPE_WSTR**||Description conviviale de l'action.|  
|**CONTENT**|**DBTYPE_WSTR**||Expression ou contenu de l'action qui sera exécutée.|  
|**APPLICATION**|**DBTYPE_WSTR**||Nom de l'application qui sera utilisée pour exécuter l'action.|  
|**APPEL**|**DBTYPE_I4**||Informations sur la manière dont l'action doit être appelée :<br /><br /> **MDACTION_INVOCATION_INTERACTIVE** (**1**) indique une action régulière utilisée pendant les opérations normales. Il s'agit de la valeur par défaut pour cette colonne.<br /><br /> **MDACTION_INVOCATION_ON_OPEN** (**2**) indique que l’action doit être effectuée lors de la première ouverture du cube.<br /><br /> **MDACTION_INVOCATION_BATCH** (**4**) indique que l’action est effectuée dans le cadre d’une opération de traitement par lots ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] tâche.<br /><br /> <br /><br /> Notez que ces valeurs d’énumération sont définies dans le fichier, Msmd.h.|  
  
 L’ensemble de lignes est trié sur **CATALOG_NAME**, **nom_schéma**, **CUBE_NAME**, **ACTION_NAME**.  
  
> [!NOTE]  
>  Actions de **MDACTION_TYPE_PROPRIETARY** type doit fournir une valeur pour le **APPLICATION** colonne.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_ACTIONS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Obligatoire|  
|**ACTION_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif|  
|**ACTION_TYPE**|**DBTYPE_I4**|Ce paramètre est facultatif|  
|**COORDONNÉES**|**DBTYPE_WSTR**|Obligatoire|  
|**COORDINATE_TYPE**|**DBTYPE_I4**|Obligatoire|  
|**APPEL**|**DBTYPE_I4**|(Facultatif) Le **INVOCATION** la valeur de colonne de restriction par défaut **MDACTION_INVOCATION_INTERACTIVE**. Pour récupérer toutes les actions, utilisez la **MDACTION_INVOCATION_ALL** valeur dans le **INVOCATION** colonne de restriction.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
  
> [!IMPORTANT]  
>  Le **INVOCATION** colonne de restriction a la valeur par défaut **MDACTION_INVOCATION_INTERACTIVE**. Tout ensemble de lignes de schéma qui ne spécifie pas explicitement une valeur pour cette colonne contient uniquement des lignes avec cette valeur. Si vous souhaitez que l’ensemble de lignes pour contenir l’ensemble des actions, utilisez la **MDACTION_INVOCATION_ALL** constante dans le **INVOCATION** colonne de restriction.  
  
 Les applications clientes peuvent définir plusieurs **ACTION_TYPE** à l’aide de l’opérateur OR.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant répertorie les valide **coordonner** et **COORDINATE_TYPE** combinaisons.  
  
|Type d'objet COORDINATE|COORDINATE_TYPE|  
|----------------------------|----------------------|  
|**Cube**|**MDACTION_COORDINATE_CUBE**|  
|**Dimension**|**MDACTION_COORDINATE_DIMENSION**<br /><br /> **MDACTION_COORDINATE_LEVEL**<br /><br /> **MDACTION_COORDINATE_MEMBER**<br /><br /> **MDACTION_COORDINATE_SET**<br /><br /> **MDACTION_COORDINATE_CELL**|  
|**Hiérarchie**|**MDACTION_COORDINATE_DIMENSION**|  
|**Level**|**MDACTION_COORDINATE_LEVEL**|  
|**Membre**|**MDACTION_COORDINATE_MEMBER**|  
|**Ensemble**|**MDACTION_COORDINATE_SET**|  
|**Cellule**|**MDACTION_COORDINATE_CELL**|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
