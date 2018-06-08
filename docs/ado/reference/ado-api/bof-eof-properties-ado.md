---
title: Début de fichier EOF, propriétés (ADO) | Documents Microsoft
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
- Recordset15::BOF
- Recordset15::EOF
helpviewer_keywords:
- EOF property [ADO]
- BOF property [ADO]
ms.assetid: 36c31ab2-f3b6-4281-89b6-db7e04e38fd2
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 369d6a3b4d069ed67ccc4c4d217aa257c20c79b1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bof-eof-properties-ado"></a>Début de fichier EOF, propriétés (ADO)
-   **BOF** indique que la position actuelle se trouve avant le premier enregistrement dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
-   **EOF** indique que la position actuelle se trouve après le dernier enregistrement dans un **Recordset** objet.  
  
## <a name="return-value"></a>Valeur retournée  
 Le **BOF** et **EOF** propriétés retour **booléenne** valeurs.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **BOF** et **EOF** propriétés afin de déterminer si un **Recordset** objet contient des enregistrements ou si vous avez dépassé les limites d’un **Recordset**  lorsque vous déplacez d’un enregistrement à l’objet.  
  
 Le **BOF** propriété renvoie **True** (-1) si la position actuelle est avant le premier enregistrement et **False** (0) si la position actuelle est l’ou après le premier enregistrement.  
  
 Le **EOF** propriété renvoie **True** si la position actuelle est après le dernier enregistrement et **False** si la position actuelle est l’ou avant le dernier enregistrement.  
  
 Si le **BOF** ou **EOF** propriété **True**, il n’existe aucun enregistrement actif.  
  
 Si vous ouvrez un **Recordset** objet ne contenant aucun enregistrement, la **BOF** et **EOF** propriétés sont définies sur **True** (voir la [ RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) propriété pour plus d’informations sur cet état d’un **Recordset**). Lorsque vous ouvrez un **Recordset** objet qui contient au moins un enregistrement, le premier enregistrement est l’enregistrement actif et la **BOF** et **EOF** propriétés sont **False** .  
  
 Si vous supprimez le dernier enregistrement restant dans le **Recordset** objet, le **BOF** et **EOF** propriétés peuvent rester **False** jusqu'à ce que vous tentative d’enregistrement.  
  
 Le tableau ci-dessous montre ce qui **déplacer** les méthodes sont autorisées avec les différentes combinaisons de le **BOF** et **EOF** propriétés.  
  
||MoveFirst,<br /><br /> MoveLast|MovePrevious,<br /><br /> Déplacer < 0|Déplacer 0|MoveNext,<br /><br /> Déplacer > 0|  
|------|-----------------------------|---------------------------------|------------|-----------------------------|  
|**BOF**=**True**, **EOF**=**False**|Autorisé|Erreur|Erreur|Autorisé|  
|**BOF**=**False**, **EOF**=**True**|Autorisé|Autorisé|Erreur|Erreur|  
|Les deux **True**|Erreur|Erreur|Erreur|Erreur|  
|Les deux **False**|Autorisé|Autorisé|Autorisé|Autorisé|  
  
 Ce qui permet une **déplacer** méthode ne garantit pas que la méthode parviendra à localiser un enregistrement ; cela signifie uniquement que l’appel spécifié **déplacer** méthode ne génère pas d’une erreur.  
  
 Le tableau suivant montre ce qui arrive à la **BOF** et **EOF** les paramètres de propriété lorsque vous appelez différentes **déplacer** méthodes sont mais Impossible de trouver un enregistrement.  
  
||BOF|EOF|  
|------|---------|---------|  
|**MoveFirst**, **MoveLast**|La valeur **True**|La valeur **True**|  
|**Déplacer** 0|Aucun changement|Aucun changement|  
|**MovePrevious**, **déplacer** < 0|La valeur **True**|Aucun changement|  
|**MoveNext**, **déplacer** > 0|Aucun changement|La valeur **True**|  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BOF, EOF et Bookmark, propriétés-exemple (VB)](../../../ado/reference/ado-api/bof-eof-and-bookmark-properties-example-vb.md)   
 [BOF, EOF et Bookmark, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/bof-eof-and-bookmark-properties-example-vc.md)   
