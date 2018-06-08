---
title: Mise à jour et persistance des données | Documents Microsoft
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
- updating data [ADO]
- data updates [ADO]
- ADO, updating data
ms.assetid: 8dc27274-4f96-43d1-913c-4ff7d01b9a27
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 40e92067564f56eb7bc30739c0abd0d758a3b9c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-and-persisting-data"></a>Mise à jour et persistance des données
Les chapitres précédents ont décrit comment utiliser ADO pour accéder aux données d’une source de données, pour vous déplacer dans les données et même comment modifier les données. Bien entendu, si votre application vise à permettre aux utilisateurs d’apporter des modifications aux données, vous devez comprendre comment enregistrer ces modifications. Vous pouvez soit conserver le **Recordset** modifications apportées à un fichier en utilisant le **enregistrer** (méthode), ou vous pouvez renvoyer les modifications à la source de données pour le stockage à l’aide la **mise à jour** ou  **UpdateBatch** méthodes.  
  
 Dans les chapitres précédents, vous avez modifié les données dans plusieurs lignes de la **Recordset**. ADO prend en charge deux notions de base relatives à l’ajout, la suppression et modification des lignes de données.  
  
 La notion de première est que les modifications ne sont pas directement appliquées à la **Recordset**; au lieu de cela, ils sont apportées à une liste interne *tampon de copie*. Si vous décidez de ne pas les modifications, les modifications dans la mémoire tampon de copie sont ignorées. Si vous décidez de conserver les modifications, les modifications dans la mémoire tampon de copie sont appliquées à la **Recordset**.  
  
 La notion de deuxième est que les modifications sont soit propagées à la source de données dès que vous déclarez que le travail sur une ligne complète (autrement dit, *immédiate* mode), ou toutes les modifications apportées à un ensemble de lignes sont collectées jusqu'à ce que vous déclarez le travail pour le jeu complète (autrement dit, *lot* mode). Le **LockType** propriété détermine quand les modifications sont apportées à la source de données sous-jacente. **adLockOptimistic** ou **adLockPessimistic** Spécifie le mode d’exécution, tandis que **adLockBatchOptimistic** Spécifie le mode batch. Le **CursorLocation** qui peuvent affecter les propriétés **LockType** paramètres sont disponibles. Par exemple, le **adLockPessimistic** paramètre n’est pas pris en charge si le **CursorLocation** est définie sur **adUseClient**.  
  
 Dans ce mode, chaque appel de la **mise à jour** méthode propage les modifications apportées à la source de données. En mode batch, chaque appel de **mise à jour** ou déplacement de la position de ligne actuelle enregistre les modifications dans la mémoire tampon de copie, mais uniquement le **UpdateBatch** méthode propage les modifications apportées à la source de données.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Mise à jour des données](../../../ado/guide/data/updating-data.md)  
  
-   [Persistance des données](../../../ado/guide/data/persisting-data.md)
