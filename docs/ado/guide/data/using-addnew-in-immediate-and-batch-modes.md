---
title: À l’aide de AddNew dans l’immédiat et les Modes de traitement par lots | Documents Microsoft
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
- AddNew method [ADO]
- ADO, editing data
- ADO, adding data
- editing data [ADO], AddNew method
ms.assetid: ed314bb9-e188-4658-a68c-a2abc49610be
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3c66b250780e3b12ae9590796a062426f89920de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-addnew-in-immediate-and-batch-modes"></a>À l’aide de AddNew dans l’immédiat et les Modes de traitement par lots
Le comportement de la **AddNew** méthode varie selon le mode de mise à jour de la **Recordset** objet et si vous passez le *liste de champs* et *valeurs*arguments.  
  
 En mode de mise à jour immédiate (dans lequel le fournisseur écrit les modifications à la source de données sous-jacente lorsque vous appelez le **mettre à jour** méthode), l’appel le **AddNew** méthode sans arguments affecte le  **EditMode** propriété **adEditAdd.** Le fournisseur met en cache localement les modifications des valeurs de champ. Appel de la **mise à jour** méthode publie le nouvel enregistrement dans la base de données et réinitialise le **EditMode** propriété **adEditNone.** Si vous passez le *liste de champs* et *valeurs* arguments, ADO publie immédiatement le nouvel enregistrement dans la base de données (aucun **mise à jour** appel n’est nécessaire) ; le **EditMode**  valeur de propriété ne change pas (**adEditNone**).  
  
 En mode de mise à jour par lots, appelant le **AddNew** méthode sans arguments affecte le **EditMode** propriété **adEditAdd**. Le fournisseur met en cache localement les modifications des valeurs de champ. Appel de la **mise à jour** méthode ajoute le nouvel enregistrement actuel **Recordset** et réinitialise le **EditMode** propriété **adEditNone**, mais le fournisseur ne valide pas les modifications apportées à la base de données jusqu'à ce que vous appeliez la **UpdateBatch** (méthode). Si vous passez le *liste de champs* et *valeurs* arguments, ADO envoie le nouvel enregistrement dans le fournisseur de stockage dans un cache ; vous devez appeler la **UpdateBatch** pour valider la nouvelle méthode Enregistrez la base de données sous-jacente. Pour plus d’informations sur **mise à jour** et **UpdateBatch**, consultez [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).
