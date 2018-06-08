---
title: Méthode UpdateBatch | Documents Microsoft
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
- Recordset15::UpdateBatch
- Recordset15::raw_UpdateBatch
helpviewer_keywords:
- UpdateBatch method [ADO]
ms.assetid: 23f9314c-b027-4a51-aeae-50caa2977740
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 75fa802b85b1bdb9f2dcd97af8c244a41f7ec37b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updatebatch-method"></a>Méthode UpdateBatch
Écrit toutes les mises à jour par lot en attente sur le disque.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.UpdateBatch AffectRecords, PreserveStatus  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Ce paramètre est facultatif. Un [AffectEnum](../../../ado/reference/ado-api/affectenum.md) valeur qui indique le nombre d’enregistrements le **UpdateBatch** méthode affectera.  
  
 *PreserveStatus*  
 Ce paramètre est facultatif. A **booléenne** valeur qui spécifie si les modifications locales, comme indiqué par le [état](../../../ado/reference/ado-api/status-property-ado-recordset.md) propriété, doivent être validées. Si cette valeur est définie sur **True**, le **état** propriété de chaque enregistrement reste inchangée après la mise à jour est terminée.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **UpdateBatch** méthode lorsque vous modifiez un **Recordset** objet en mode de mise à jour par lot pour transmettre toutes les modifications apportées dans un **Recordset** objet à la base de données sous-jacente.  
  
 Si le **Recordset** objet prend en charge la mise à jour par lot, vous pouvez mettre en cache plusieurs modifications apportées à un ou plusieurs enregistrements localement jusqu'à ce que vous appeliez la **UpdateBatch** (méthode). Si vous modifiez l’enregistrement actif ou ajoutez un nouvel enregistrement lorsque vous appelez le **UpdateBatch** (méthode), ADO appelle automatiquement la [mise à jour](../../../ado/reference/ado-api/update-method.md) méthode pour enregistrer les modifications en attente à l’enregistrement actif avant transmettre les modifications par lot au fournisseur. Vous devez utiliser les commandes de mise à jour avec un jeu de clés ou un curseur statique uniquement.  
  
> [!NOTE]
>  Spécification de **adAffectGroup** la valeur de ce paramètre entraînera une erreur lorsqu’il existe des enregistrements visibles en cours **Recordset** (par exemple, un filtre pour lequel aucun enregistrement correspondant).  
  
 Si la transmission des modifications échoue pour un ou tous les enregistrements en raison d’un conflit avec les données sous-jacentes (par exemple, lorsqu’un enregistrement a déjà été supprimé par un autre utilisateur), le fournisseur retourne des avertissements dans le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection et un Erreur d’exécution se produit. Utilisez le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété (**adFilterAffectedRecords**) et le [état](../../../ado/reference/ado-api/status-property-ado-recordset.md) propriété pour localiser les enregistrements en conflit.  
  
 Pour annuler tout en attente de mises à jour par lots, utilisez le [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) (méthode).  
  
 Si le [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) et [Update Resync](../../../ado/reference/ado-api/update-resync-property-dynamic-ado.md) propriétés dynamiques sont définies et la **Recordset** est le résultat de l’exécution d’une opération de jointure sur plusieurs tables, puis le l’exécution de la **UpdateBatch** méthode est implicitement suivie par la [Resync](../../../ado/reference/ado-api/resync-method.md) (méthode), en fonction des paramètres de la [Update Resync](../../../ado/reference/ado-api/update-resync-property-dynamic-ado.md) propriété.  
  
 L’ordre dans lequel les mises à jour individuelles d’un lot sont effectuées sur la source de données n’est pas nécessairement le même que l’ordre dans lequel ils ont été effectuées sur l’ordinateur local **Recordset**. Ordre de mise à jour dépend du fournisseur. Prendre en compte lors du codage des mises à jour qui sont liés entre eux, telles que les contraintes de clé étrangère sur une insertion ou une mise à jour.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [UpdateBatch et CancelBatch, méthodes-exemple (VB)](../../../ado/reference/ado-api/updatebatch-and-cancelbatch-methods-example-vb.md)   
 [UpdateBatch et CancelBatch, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/updatebatch-and-cancelbatch-methods-example-vc.md)   
 [CancelBatch, méthode (ADO)](../../../ado/reference/ado-api/cancelbatch-method-ado.md)   
 [Clear (méthode) (ADO)](../../../ado/reference/ado-api/clear-method-ado.md)   
 [LockType, propriété (ADO)](../../../ado/reference/ado-api/locktype-property-ado.md)   
 [Update, méthode](../../../ado/reference/ado-api/update-method.md)
