---
title: Autres méthodes de déplacement d’un objet Recordset | Documents Microsoft
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
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- Recordset object [ADO], moving
- MovePrevious method [ADO]
ms.assetid: 9f8cf1b2-3def-453f-a0ff-4646c5f15262
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 21c9c99d9e168d73a333fe001b56dcdf845339f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="more-ways-to-move-in-a-recordset"></a>Autres méthodes de déplacement dans un jeu d’enregistrements
Les quatre méthodes suivantes servent à déplacer, ou faites défiler, dans le **Recordset**: [MoveFirst, MoveLast, MoveNext et MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md). (Certaines de ces méthodes ne sont pas disponibles sur les curseurs avant uniquement.)  
  
 **MoveFirst** modifie la position actuelle vers le premier enregistrement dans le **Recordset**. **MoveLast** modifications position de l’enregistrement actif au dernier enregistrement dans le **Recordset**. Pour utiliser **MoveFirst** ou **MoveLast**, le **Recordset** objet doit prendre en charge les signets ou le déplacement du curseur arrière ; sinon, l’appel de méthode génère une erreur.  
  
 **MoveNext** déplace l’enregistrement actif positionner un seul endroit vers l’avant. Si vous êtes sur le dernier enregistrement lorsque vous appelez **MoveNext**, **EOF** a la valeur **True**. **MovePrevious** déplace l’enregistrement actif positionner un seul endroit vers l’arrière. Si vous êtes sur le premier enregistrement lorsque vous appelez **MovePrevious**, **BOF** a la valeur **True**. Il est conseillé de vérifier la **EOF** et **BOF** propriétés lors de l’utilisation de ces méthodes et remettre en place le curseur à une position d’enregistrement actif valide si vous dépassez des extrémités de la **Recordset**, comme illustré ici :  
  
```  
. . .  
oRs.MoveNext  
If oRs.EOF Then oRs.MoveLast  
. . .   
```  
  
 Ou, dans le cas de la **MovePrevious** méthode :  
  
```  
. . .   
oRs.MovePrevious  
If oRs.BOF Then oRs.MoveFirst  
. . .  
```  
  
 Dans le cas où la **Recordset** a été filtrée ou triée et les données de l’enregistrement actif sont modifiées, la position peut également changer. Dans ce cas le **MoveNext** méthode fonctionne normalement, mais sachez que la position est déplacée d’un enregistrement vers l’avant à partir de la nouvelle position, pas de l’ancien. Par exemple, remplacer les données dans l’enregistrement actif, telles que l’enregistrement est déplacé à la fin de la liste triée **Recordset**, signifie que l’appel **MoveNext** entraîne ADO de définition de l’enregistrement en cours sur le position après le dernier enregistrement de la **Recordset** (**EOF** = **True**).  
  
 Le comportement des diverses méthodes de déplacement de la **Recordset** dépend de l’objet, dans une certaine mesure, sur les données dans le **Recordset**. Nouveaux enregistrements ajoutés à la **Recordset** sont initialement ajoutés dans un ordre particulier, ce qui est défini par la source de données et peut-être dépendre, implicitement ou explicitement des données dans le nouvel enregistrement. Par exemple, si un tri ou une jointure est effectuée dans la requête qui remplit la **Recordset**, le nouvel enregistrement est inséré dans l’emplacement approprié dans le **Recordset**. Si le classement n’est pas explicitement spécifié lors de la création du **Recordset**, les modifications dans l’implémentation de source de données peuvent provoquer l’ordre des lignes renvoyées modifier par inadvertance. De plus, le tri, le filtrage, fonctions d’édition et de la **Recordset** peuvent affecter l’ordre et éventuellement les lignes dans le jeu d’enregistrements qui sont visibles.  
  
 Par conséquent, **MoveNext**, **MovePrevious**, **MoveFirst**, **MoveLast**, et **déplacer** sont tous les sensible aux autres opérations effectuées sur le même **Recordset**. ADO tente toujours mettre à jour la position actuelle jusqu'à ce que vous la déplacez explicitement, mais dans certains cas, les modifications intervenues rendent difficile à comprendre les effets d’un déplacement suivant. Par exemple, si vous appelez **MoveFirst** à la position sur la première ligne d’une liste triée **Recordset** et que vous remplacez le tri croissant à l’ordre décroissant, vous êtes toujours sur la même ligne, mais il est maintenant la dernière ligne dans le **Recordset**. **MoveFirst** vous conduira à une autre (la dernière ligne).  
  
 Autre exemple, si vous êtes positionné sur une ligne particulière au milieu d’un **Recordset** et que vous appelez **supprimer** , puis appelez **MoveNext**, vous êtes maintenant sur l’enregistrement immédiatement après l’enregistrement supprimé. Contrairement à l’appel **MovePrevious** rend l’enregistrement précédant celle que vous avez supprimé l’enregistrement en cours, car l’enregistrement supprimé n’est plus comptabilisé dans l’abonnement actif de la **Recordset**.  
  
 Il est particulièrement difficile de définir une sémantique de déplacement cohérente entre tous les fournisseurs pour les méthodes qui passent par rapport à l’enregistrement actif : **MovePrevious**, **MoveNext**, et **déplacer** : en cas de modification de données dans l’enregistrement actif. Par exemple, si vous travaillez avec un triées, filtrées **Recordset**et vous modifiez les données dans l’enregistrement actif afin qu’elle précède toutes les autres enregistrements, mais vos données modifiées également n’est plus correspond au filtre, il est difficile de déterminer où un **MoveNext** opération devrait vous prendre. La conclusion plus sûre est mouvement relatif au sein d’un **Recordset** est plus de risques que le déplacement des absolu (telles que l’utilisation **MoveFirst** ou **MoveLast**) lorsque les données sont la modification pendant que les enregistrements modifiés, ajoutés ou supprimés. Tri et filtrage doivent reposer sur une clé primaire ou l’ID, car vous ne devez pas modifier ce type de valeur.
