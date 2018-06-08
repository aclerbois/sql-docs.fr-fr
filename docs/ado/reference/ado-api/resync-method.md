---
title: Resync, méthode | Documents Microsoft
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
- Recordset20::raw_Resync
- Fields::Resync
- Recordset20::Resync
- Fields::raw_Resync
helpviewer_keywords:
- Resync method [ADO]
ms.assetid: 73b355d4-a4c0-434b-bfc4-039b1c76b32e
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 187f1397cbdb4e6ccdfc39b573f301fce1a957ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="resync-method"></a>Resync, méthode
Actualise les données en cours [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet, ou [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection d’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet, à partir de la base de données sous-jacente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Recordset.Resync AffectRecords, ResyncValues Record.Fields.Resync ResyncValues  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Ce paramètre est facultatif. Un [AffectEnum](../../../ado/reference/ado-api/affectenum.md) valeur qui détermine le nombre d’enregistrements le **Resync** méthode affectera. La valeur par défaut est **adAffectAll**. Cette valeur n’est pas disponible avec la **Resync** méthode de la **champs** collection d’un **enregistrement** objet.  
  
 *ResyncValues*  
 Ce paramètre est facultatif. A [ResyncEnum](../../../ado/reference/ado-api/resyncenum.md) valeur qui indique si les valeurs sous-jacentes sont remplacées. La valeur par défaut est **adResyncAllValues**.  
  
## <a name="remarks"></a>Notes  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Utilisez le **Resync** méthode pour resynchroniser des enregistrements en cours **Recordset** avec la base de données sous-jacente. Cela est utile si vous utilisez un curseur statique ou avant uniquement, mais que vous souhaitez voir les modifications apportées dans la base de données sous-jacente.  
  
 Si vous définissez la [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété **adUseClient**, **Resync** est disponible uniquement pour non-en lecture seule **Recordset** objets.  
  
 Contrairement à la [Requery](../../../ado/reference/ado-api/requery-method.md) (méthode), la **Resync** méthode ne réexécute pas la **Recordset** commande sous-jacent de l’objet. Nouveaux enregistrements dans la base de données sous-jacente ne seront pas visibles.  
  
 Si la tentative de resynchronisation échoue en raison d’un conflit avec les données sous-jacentes (par exemple, un enregistrement a été supprimé par un autre utilisateur), le fournisseur retourne des avertissements dans le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection et une erreur d’exécution se produit. Utilisez le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété (**adFilterConflictingRecords**) et le [état](../../../ado/reference/ado-api/status-property-ado-recordset.md) propriété pour localiser les enregistrements en conflit.  
  
 Si le [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) et [Resync Command](../../../ado/reference/ado-api/resync-command-property-dynamic-ado.md) propriétés dynamiques sont définies et la **Recordset** est le résultat de l’exécution d’une opération de jointure de plusieurs tables, puis le  **Resync** méthode exécute la commande donnée dans le **Resync Command** propriété uniquement sur la table désignée dans la **Unique Table** propriété.  
  
## <a name="fields"></a>Champs  
 Utilisez le **Resync** méthode pour resynchroniser les valeurs de la **champs** collection d’un **enregistrement** objet avec la source de données sous-jacente. Le [nombre](../../../ado/reference/ado-api/count-property-ado.md) propriété n’est pas affectée par cette méthode.  
  
 Si *ResyncValues* a la valeur **adResyncAllValues** (la valeur par défaut), le [UnderlyingValue](../../../ado/reference/ado-api/underlyingvalue-property.md), [valeur](../../../ado/reference/ado-api/value-property-ado.md), et [ OriginalValue](../../../ado/reference/ado-api/originalvalue-property-ado.md) propriétés de [champ](../../../ado/reference/ado-api/field-object.md) les objets dans la collection sont synchronisés. Si *ResyncValues* a la valeur **adResyncUnderlyingValues**, seule la **UnderlyingValue** propriété est synchronisée.  
  
 La valeur de la **état** propriété pour chaque **champ** objet au moment de l’appel affecte également le comportement de **Resync**. Pour **champ** objets ayant **état** les valeurs de **adFieldPendingUnknown** ou **adFieldPendingInsert**, **Resync**  n’a aucun effet. Pour **état** les valeurs de **égales à adFieldPendingChange** ou **adFieldPendingDelete**, **Resync** synchronise les valeurs de données pour les champs qui existent toujours dans la source de données.  
  
 **Resync** ne modifiera pas **état** les valeurs de **champ** objets, sauf si une erreur se produit lorsque **Resync** est appelée. Par exemple, si le champ n’existe plus, le fournisseur retournera appropriée **état** valeur pour le **champ** de l’objet, tel que **adFieldDoesNotExist**. Retourné **état** valeurs peuvent être combinées de manière logique dans la valeur de la **état** propriété.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)|[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Resync, méthode-exemple (VB)](../../../ado/reference/ado-api/resync-method-example-vb.md)   
 [Resync, méthode-exemple (VC ++)](../../../ado/reference/ado-api/resync-method-example-vc.md)   
 [Clear (méthode) (ADO)](../../../ado/reference/ado-api/clear-method-ado.md)   
 [UnderlyingValue, propriété](../../../ado/reference/ado-api/underlyingvalue-property.md)
