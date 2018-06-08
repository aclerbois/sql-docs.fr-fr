---
title: Prend en charge la méthode | Documents Microsoft
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
- Recordset15::raw_Supports
- Recordset15::Supports
helpviewer_keywords:
- Supports method [ADO]
ms.assetid: 298fc41c-0b55-42fc-b373-c5133b4da6a5
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f1a24ac211293847ffbb068055826abca3514abb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supports-method"></a>Prend en charge (méthode)
Détermine si un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet prend en charge un type particulier de fonctionnalité.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **booléenne** valeur qui indique si toutes les fonctionnalités identifié par le *CursorOptions* argument sont pris en charge par le fournisseur.  
  
#### <a name="parameters"></a>Paramètres  
 *CursorOptions*  
 A **Long** expression se compose d’un ou plusieurs [CursorOptionEnum](../../../ado/reference/ado-api/cursoroptionenum.md) valeurs.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **prend en charge** méthode pour déterminer les types de fonctionnalités un **Recordset** prend en charge de l’objet. Si le **Recordset** objet prend en charge les fonctionnalités dont les constantes correspondantes se trouvent dans *CursorOptions*, le **prend en charge** méthode retourne **True**. Sinon, elle retourne **False**.  
  
> [!NOTE]
>  Bien que le **prend en charge** méthode peut retourner **True** pour une fonctionnalité donnée, il ne pas garantit que le fournisseur peut rendre la fonctionnalité disponible en toutes circonstances. Le **prend en charge** méthode retourne simplement si le fournisseur peut prendre en charge la fonctionnalité spécifiée, en supposant que certaines conditions sont remplies. Par exemple, le **prend en charge** méthode peut indiquer qu’un **Recordset** objet prend en charge les mises à jour même si le curseur est basé sur une jointure de plusieurs tables, dont certaines colonnes ne sont pas modifiables.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode prend en charge (VB)](../../../ado/reference/ado-api/supports-method-example-vb.md)   
 [Exemple de méthode prend en charge (VC ++)](../../../ado/reference/ado-api/supports-method-example-vc.md)   
 [CursorType, propriété (ADO)](../../../ado/reference/ado-api/cursortype-property-ado.md)
