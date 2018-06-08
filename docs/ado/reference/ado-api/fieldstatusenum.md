---
title: FieldStatusEnum | Documents Microsoft
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
- FieldStatusEnum
helpviewer_keywords:
- FieldStatusEnum enumeration [ADO]
ms.assetid: e06da1e2-303f-41b2-a3b0-61e233da152c
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c4a64c48018b0a8012da631af5b4af0259f0b8e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fieldstatusenum"></a>FieldStatusEnum
Spécifie le [état](../../../ado/reference/ado-api/status-property-ado-field.md) d’un [objet Field](../../../ado/reference/ado-api/field-object.md).  
  
 Le **adFieldPending\***  valeurs indiquent l’opération qui a provoqué l’état et peut être combinée avec d’autres valeurs d’état.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adFieldAlreadyExists**|26|Indique que le champ spécifié existe déjà.|  
|**adFieldBadStatus**|12|Indique qu’une valeur d’état non valide a été envoyée à partir d’ADO pour le fournisseur OLE DB. Les causes possibles incluent un OLE DB 1.0 ou 1.1 fournisseur ou une combinaison incorrecte de [valeur](../../../ado/reference/ado-api/value-property-ado.md) et [état](../../../ado/reference/ado-api/status-property-ado-field.md).|  
|**adFieldCannotComplete**|20|Indique que le serveur de l’URL spécifiée par [Source](../../../ado/reference/ado-api/source-property-ado-record.md) ne peut pas terminer l’opération.|  
|**adFieldCannotDeleteSource**|23|Indique que pendant une opération de déplacement, un arbre ou sous-arbre a été déplacé vers un nouvel emplacement, mais la source n’a pas pu être supprimée.|  
|**adFieldCantConvertValue**|2|Indique que le champ ne peut pas être extraites ou stocké sans perte de données.|  
|**adFieldCantCreate**|7|Indique que le champ ne peut pas ajouté, car le fournisseur a dépassé une limite (par exemple, le nombre de champs autorisé).|  
|**adFieldDataOverflow**|6|Indique que les données renvoyées par le fournisseur a dépassé le type de données du champ.|  
|**adFieldDefault**|13|Indique que la valeur par défaut pour le champ a été utilisée lors de la définition de données.|  
|**adFieldDoesNotExist**|16|Indique que le champ spécifié n’existe pas.|  
|**adFieldIgnore**|15|Indique que ce champ a été ignoré lorsque les valeurs de données de paramètre dans la source. Aucune valeur dans le fournisseur.|  
|**adFieldIntegrityViolation**|10|Indique que le champ ne peut pas être modifié car il s’agit d’une entité calculée ou dérivée.|  
|**adFieldInvalidURL**|17|Indique que l’URL de source de données contient des caractères non valides.|  
|**adFieldIsNull**|3|Indique que le fournisseur a retourné une valeur de type VARIANT de type VT_NULL et que le champ n’est pas vide.|  
|**adFieldOK**|0|Valeur par défaut. Indique que le champ a été correctement ajouté ou supprimé.|  
|**adFieldOutOfSpace**|22|Indique que le fournisseur est impossible d’obtenir un espace de stockage suffisant pour terminer un déplacement ou de l’opération de copie.|  
|**adFieldPendingChange**|0x40000|Indique soit qui le champ a été supprimé puis rajouté, peut-être avec un type de données différent, ou que la valeur du champ état connu de **adFieldOK** a changé. La forme finale du champ modifiera la [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection après le [mise à jour](../../../ado/reference/ado-api/update-method.md) méthode est appelée.|  
|**adFieldPendingDelete**|0x20000|Indique que le **supprimer** l’opération a provoqué l’état à définir. Le champ a été marqué pour suppression à partir de la **champs** collection après le **mise à jour** méthode est appelée.|  
|**adFieldPendingInsert**|0x10000|Indique que le **Append** l’opération a provoqué l’état à définir. Le **champ** a été marqué pour être ajouté à la **champs** collection après le **mise à jour** méthode est appelée.|  
|**adFieldPendingUnknown**|0x80000|Indique que le fournisseur ne peut pas déterminer le statut de champ d’opération a entraîné à définir.|  
|**adFieldPendingUnknownDelete**|0x100000|Indique que le fournisseur ne peut pas déterminer l’opération qui a défini l’état champ, et que le champ sera supprimé le **champs** collection après le **mise à jour** méthode est appelée.|  
|**adFieldPermissionDenied**|9|Indique que le champ ne peut pas être modifié car il est défini comme étant en lecture seule.|  
|**adFieldReadOnly**|24|Indique que le champ dans la source de données est défini en lecture seule.|  
|**adFieldResourceExists**|19|Indique que le fournisseur n’a pas pu effectuer l’opération car l’objet existe déjà à l’URL de destination, et il n’est pas en mesure de remplacer l’objet.|  
|**adFieldResourceLocked**|18|Indique que le fournisseur n’a pas pu effectuer l’opération car la source de données est verrouillée par un ou plusieurs autres applications ou les processus.|  
|**adFieldResourceOutOfScope**|25|Indique qu’une URL de la source ou de destination est en dehors de l’étendue de l’enregistrement actif.|  
|**adFieldSchemaViolation**|11|Indique que la valeur a violé la contrainte de schéma de source de données pour le champ.|  
|**adFieldSignMismatch**|5|Indique que la valeur des données retournée par le fournisseur a été signée mais le type de données de la valeur du champ ADO n’était pas signé.|  
|**adFieldTruncated**|4|Indique que les données de longueur variable ont été tronquées lors de la lecture à partir de la source de données.|  
|**adFieldUnavailable**|8|Indique que le fournisseur ne peut pas déterminer la valeur lors de la lecture à partir de la source de données. Par exemple, la ligne vient d’être créée, la valeur par défaut pour la colonne n’était pas disponible, et une nouvelle valeur n’avait pas été spécifiée.|  
|**adFieldVolumeNotFound**|21|Indique que le fournisseur est impossible de localiser le volume de stockage indiqué par l’URL.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Status, propriété (objet Field ADO)](../../../ado/reference/ado-api/status-property-ado-field.md)
