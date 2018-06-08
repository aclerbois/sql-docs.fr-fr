---
title: GetRows, méthode (ADO) | Documents Microsoft
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
- Recordset15::GetRows
- Recordset15::raw_GetRows
helpviewer_keywords:
- Getrows method [ADO]
ms.assetid: 14b92860-4171-47d9-a413-dd60dd6a8880
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: efe7a21a26e99d089c64fcd3a627c693c5f7de09
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getrows-method-ado"></a>GetRows, méthode (ADO)
Récupère plusieurs enregistrements d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet dans un tableau.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array = recordset.GetRows(Rows, Start, Fields )  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **Variant** dont la valeur est un tableau à deux dimensions.  
  
#### <a name="parameters"></a>Paramètres  
 *Lignes*  
 Ce paramètre est facultatif. A [GetRowsOptionEnum](../../../ado/reference/ado-api/getrowsoptionenum.md) valeur qui indique le nombre d’enregistrements à récupérer. La valeur par défaut est **adGetRowsRest**.  
  
 *Démarrer*  
 Ce paramètre est facultatif. A **chaîne** valeur ou **Variant** qui correspond au signet de l’enregistrement à partir de laquelle le **GetRows** opération doit commencer. Vous pouvez également utiliser un [BookmarkEnum](../../../ado/reference/ado-api/bookmarkenum.md) valeur.  
  
 *Fields*  
 Ce paramètre est facultatif. A **Variant** qui représente un seul nom de champ ou position ordinale ou un tableau de noms de champs ou de positions ordinales. ADO retourne uniquement les données de ces champs.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **GetRows** méthode pour copier des enregistrements d’un **Recordset** dans un tableau à deux dimensions. Le premier indice identifie le champ et le second identifie le numéro d’enregistrement. Le *tableau* variable est automatiquement dimensionnée à la bonne taille lors la **GetRows** méthode retourne les données.  
  
 Si vous ne spécifiez pas une valeur pour le *lignes* argument, la **GetRows** méthode récupère automatiquement tous les enregistrements dans la **Recordset** objet. Si vous demandez plus d’enregistrements sont disponibles, **GetRows** retourne uniquement le nombre d’enregistrements disponibles.  
  
 Si le **Recordset** objet prend en charge les signets, vous pouvez spécifier à quel enregistrement la **GetRows** méthode doit commencer la récupération de données en passant la valeur de cet enregistrement [signet](../../../ado/reference/ado-api/bookmark-property-ado.md)propriété dans le *Démarrer* argument.  
  
 Si vous souhaitez limiter les champs qui le **GetRows** appel retourne, vous pouvez passer un seul nom/numéro ou un tableau de noms/numéros de champ dans le *champs* argument.  
  
 Après avoir appelé **GetRows**, l’enregistrement suivant non lu devient l’enregistrement en cours, ou le [EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) est définie sur **True** s’il n’y a plus aucun enregistrement.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode GetRows (VB)](../../../ado/reference/ado-api/getrows-method-example-vb.md)   
 [GetRows, exemple de méthode (VC++)](../../../ado/reference/ado-api/getrows-method-example-vc.md)   
