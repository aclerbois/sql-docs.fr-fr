---
title: Accès à un enregistrement | Documents Microsoft
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
- record jumping [ADO]
- jumping to record [ADO]
ms.assetid: 6caf6299-2eea-4d34-9b0e-b75aab07b740
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4364db29f8d4476c6f4238b970f0e56240ef0f06
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jumping-to-a-record"></a>Accès à un enregistrement
Le [déplacer](../../../ado/reference/ado-api/move-method-ado.md) méthode vous permet de faire avancer ou reculer dans le **Recordset** un nombre spécifié d’enregistrements à l’aide de la syntaxe suivante :  
  
```  
oRs.Move NumRecords, Start  
```  
  
## <a name="remarks"></a>Notes  
 Le **déplacer** méthode est prise en charge sur tous les **Recordset** objets.  
  
 Si le *NbEnregistrements* argument est supérieure à zéro, la position actuelle se déplace vers l’avant (vers la fin de la **Recordset**). Si *NbEnregistrements* est inférieur à zéro, la position actuelle se déplace vers l’arrière (vers le début de la **Recordset**).  
  
 Si le **déplacer** appel serait déplace la position actuelle à un point avant le premier enregistrement, ADO définit l’enregistrement actif à la position avant le premier enregistrement dans le **Recordset** (**BOF** est **True**). Une tentative de déplacement vers l’arrière lorsque la **BOF** propriété est déjà **True** génère une erreur.  
  
 Si le **déplacer** appel serait déplace la position actuelle vers un point après le dernier enregistrement, ADO définit l’enregistrement actif à la position après le dernier enregistrement dans le **Recordset** (**EOF** est **True**). Une tentative de déplacement vers l’avant lorsque la **EOF** propriété est déjà **True** génère une erreur.  
  
 Appel de la **déplacer** méthode vide **Recordset** objet génère une erreur.  
  
 Si vous passez un signet dans le *Démarrer* argument, le déplacement est relatif à l’enregistrement de ce signet, en supposant que le **Recordset** objet prend en charge les signets. Un signet est obtenu à l’aide de la [signet](../../../ado/reference/ado-api/bookmark-property-ado.md) propriété. Si non spécifié, le déplacement est relatif à l’enregistrement actif.  
  
 Si vous utilisez la **CacheSize** propriété à mettre en cache localement les enregistrements à partir du fournisseur, passant un *NbEnregistrements* argument qui déplace la position actuelle en dehors du groupe actuel d’enregistrements mis en cache force ADO à récupérer un nouveau groupe d’enregistrements, à partir de l’enregistrement de destination. Le **CacheSize** propriété détermine la taille du groupe qui vient d’être extrait et l’enregistrement de destination est le premier enregistrement récupéré.
