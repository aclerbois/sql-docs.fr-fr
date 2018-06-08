---
title: DELETE, méthode (jeu d’enregistrements ADO) | Documents Microsoft
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
- Recordset15::raw_Delete
- Recordset15::Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 1eb9209c-602c-4507-b0c2-6527a599b67d
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: afb1071d48cb6c4c1652cc5caab96de97beefae4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="delete-method-ado-recordset"></a>DELETE, méthode (jeu d’enregistrements ADO)
Supprime l’enregistrement actif ou un groupe d’enregistrements.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Delete AffectRecords  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Un [AffectEnum](../../../ado/reference/ado-api/affectenum.md) valeur qui détermine le nombre d’enregistrements le **supprimer** méthode affectera. La valeur par défaut est **adAffectCurrent**.  
  
> [!NOTE]
>  **adAffectAll** et **adAffectAllChapters** ne sont pas des arguments valides **supprimer**.  
  
## <a name="remarks"></a>Notes  
 À l’aide de la **supprimer** méthode marque l’enregistrement actif ou un groupe d’enregistrements dans une [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet pour la suppression. Si le **Recordset** objet n’autorise pas suppression de l’enregistrement, une erreur se produit. Si vous êtes en mode de mise à jour immédiate, suppressions se produisent immédiatement dans la base de données. Si un enregistrement ne peut pas être supprimé avec succès (en raison de violations d’intégrité de base de données, par exemple), l’enregistrement reste en mode édition après l’appel à [mise à jour](../../../ado/reference/ado-api/update-method.md). Cela signifie que vous devez annuler la mise à jour avec [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) avant de quitter l’enregistrement actif (par exemple, avec [fermer](../../../ado/reference/ado-api/close-method-ado.md), [déplacer](../../../ado/reference/ado-api/move-method-ado.md), ou [ NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)).  
  
 Si vous êtes en mode de mise à jour par lot, les enregistrements sont marqués pour suppression à partir du cache et la suppression effective intervient lorsque vous appelez le [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) (méthode). Utilisez le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété pour afficher les enregistrements supprimés.  
  
 Extraction des valeurs de champ de l’enregistrement supprimé génère une erreur. Après la suppression de l’enregistrement en cours, l’enregistrement supprimé reste actif jusqu'à ce que vous déplacez vers un autre enregistrement. Une fois vous déplacer hors de l’enregistrement supprimé, il n’est plus accessible.  
  
 Si vous imbriquez des suppressions dans une transaction, vous pouvez récupérer les enregistrements supprimés avec le [RollbackTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) (méthode). Si vous êtes en mode de mise à jour par lot, vous pouvez annuler une suppression en attente ou un groupe de suppressions en attente avec le [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) (méthode).  
  
 Si la tentative de suppression des enregistrements échoue en raison d’un conflit avec les données sous-jacentes (par exemple, lorsqu’un enregistrement a déjà été supprimé par un autre utilisateur), le fournisseur retourne des avertissements dans le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection mais n’interrompt ne pas le programme exécution. Une erreur d’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés.  
  
 Si le [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) propriétés dynamiques sont définie et la **Recordset** est le résultat de l’exécution d’une opération de jointure sur plusieurs tables, puis le **supprimer** méthode supprime uniquement lignes de la table nommée dans la [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) propriété.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer l’exemple de méthode (VB)](../../../ado/reference/ado-api/delete-method-example-vb.md)   
 [DELETE, méthode-exemple (VBScript)](../../../ado/reference/ado-api/delete-method-example-vbscript.md)   
 [Supprimer l’exemple de méthode (VC ++)](../../../ado/reference/ado-api/delete-method-example-vc.md)   
 [DELETE, méthode (Collection de champs ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [DELETE, méthode (Collection de paramètres ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [DeleteRecord, méthode (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
