---
title: Champs de la Collection (ADO) | Documents Microsoft
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
- Fields
- Recordset15::Fields
- _Record::Fields
helpviewer_keywords:
- Fields collection [ADO]
ms.assetid: 7c371474-b88f-4730-afa5-44163a0488d5
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4eebfb3b3e401585829446872545063448ec87d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fields-collection-ado"></a>Collection de champs (ADO)
Contient tous les [champ](../../../ado/reference/ado-api/field-object.md) les objets d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ou [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet.  
  
## <a name="remarks"></a>Notes  
 A **Recordset** objet a un **champs** collection composée de **champ** objets. Chaque **champ** objet correspond à une colonne dans la **Recordset**. Vous pouvez remplir le **champs** collection avant d’ouvrir le **Recordset** en appelant le [Actualiser](../../../ado/reference/ado-api/refresh-method-ado.md) méthode sur la collection.  
  
> [!NOTE]
>  Consultez le **champ** rubrique d’objet pour une explication plus détaillée de l’utilisation de **champ** objets.  
  
 Le **champs** comporte une [Append](../../../ado/reference/ado-api/append-method-ado.md) (méthode), qui crée et ajoute provisoirement un **champ** objet à la collection et un **demettreàjour**(méthode), qui finalise les ajouts et les suppressions.  
  
 A **enregistrement** objet possède deux champs spéciaux qui peuvent être indexés avec [FieldEnum](../../../ado/reference/ado-api/fieldenum.md) constantes. Une des constantes accède à un champ qui contient le flux de valeur par défaut pour le **enregistrement**, et l’autre accède à un champ qui contient la chaîne d’URL absolue pour le **enregistrement**.  
  
 Certains fournisseurs (par exemple, le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)) peut remplir le **champs** collection avec un sous-ensemble des champs disponibles pour le **enregistrement** ou **Recordset**. Autres champs ne seront pas ajoutés à la collection jusqu'à ce qu’ils sont tout d’abord référencées par son nom ou indexés par votre code.  
  
 Si vous tentez de référencer un champ inexistant par son nom, un nouveau **champ** objet sera ajouté à la **champs** collection avec une [état](../../../ado/reference/ado-api/status-property-ado-field.md) de  **adFieldPendingInsert**. Lorsque vous appelez [mise à jour](../../../ado/reference/ado-api/update-method.md), ADO crée un nouveau champ dans votre source de données si autorisé par votre fournisseur.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés de Collection de champs, méthodes et événements](../../../ado/reference/ado-api/fields-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Field, objet](../../../ado/reference/ado-api/field-object.md)
