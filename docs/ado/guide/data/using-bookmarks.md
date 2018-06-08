---
title: À l’aide de signets | Documents Microsoft
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
helpviewer_keywords:
- bookmarks [ADO]
- Recordset object [ADO]
ms.assetid: cca244e6-84f8-4394-bca9-f7a819b8f4df
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 46ea739c431005f8409b2c2680f15e55b077c086
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-bookmarks"></a>À l’aide de signets
Il est souvent utile de pouvoir revenir directement à un enregistrement spécifique après avoir déplacé dans le **Recordset** sans avoir à parcourir tous les enregistrements et comparer des valeurs. Par exemple, si vous essayez de rechercher un enregistrement à l’aide de la **trouver** méthode, mais la recherche ne retourne aucun enregistrement, vous accédez automatiquement à chaque extrémité de la **Recordset**. Si votre fournisseur prend en charge les, les signets peuvent être utilisés pour marquer votre position avant d’utiliser le **trouver** méthode afin de pouvoir revenir à votre emplacement. Un signet est un **Variant** type valeur qui identifie de façon unique un enregistrement dans un **Recordset** objet.  
  
 Vous pouvez également utiliser un tableau de type variant de signets avec le **filtre de jeu d’enregistrements** méthode pour filtrer un jeu d’enregistrements sélectionné. Pour plus d’informations sur cette technique, consultez le filtrage des résultats dans la rubrique [utilisation des jeux d’enregistrements](../../../ado/guide/data/working-with-recordsets.md), plus loin dans cette section.  
  
 Vous pouvez utiliser la **signet** propriété à obtenir un signet pour un enregistrement ou définir l’enregistrement actif dans un **Recordset** objet l’enregistrement identifié par un signet valid. Le code suivant utilise la **signet** propriété pour définir un signet et retourner à l’enregistrement de signet après avoir accédé à d’autres enregistrements. Pour déterminer si votre **Recordset** prend en charge les signets, utilisez la **prend en charge** (méthode).  
  
```  
'BeginBookmarkEg  
Dim varBookmark As Variant  
Dim blnCanBkmrk As Boolean  
  
objRs.Open strSQL, strConnStr, adOpenStatic, adLockOptimistic, adCmdText  
  
If objRs.RecordCount > 4 Then  
    objRs.Move 4                       ' move to the fifth record  
    blnCanBkmrk = objRs.Supports(adBookmark)  
    If blnCanBkmrk = True Then  
        varBookmark = objRs.Bookmark   ' record the bookmark  
        objRs.MoveLast                 ' move to a different record  
        objRs.Bookmark = varBookmark   ' return to the bookmarked (sixth) record  
    End If  
End If  
'EndBookmarkEg  
```  
  
 Le [prend en charge](../../../ado/reference/ado-api/supports-method.md) (méthode) est couvert plus en détail ultérieurement.  
  
 Sauf dans le cas de cloné **jeux d’enregistrements**, les signets sont spécifiques à la **Recordset** dans lequel ils ont été créés, même si la même commande est utilisée. Cela signifie que vous ne pouvez pas utiliser un **signet** obtenu à partir d’un **Recordset** pour déplacer vers le même enregistrement dans un second **Recordset** ouvert avec la même commande.
