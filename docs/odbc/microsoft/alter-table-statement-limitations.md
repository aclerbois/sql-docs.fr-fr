---
title: Limitations d’instruction ALTER TABLE | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC SQL grammar, ALTER TABLE statement limitations
- ALTER TABLE statement limitations [ODBC]
ms.assetid: f3e88f85-edf4-47cd-a822-292b106ddb34
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: be0319a29c0193d460e9ce54616897de3d940443
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-table-statement-limitations"></a>Limitations d’instruction ALTER TABLE
Lorsque le pilote dBASE ou Paradox est utilisé, une fois qu’un index a été créé et ajouté à un nouvel enregistrement, la structure de la table ne peut pas être modifiée par l’instruction ALTER TABLE, sauf si l’index est supprimé et le contenu de la table est supprimé.  
  
 Les instructions ALTER TABLE ne sont pas pris en charge pour les pilotes Microsoft Excel ou texte.  
  
> [!NOTE]  
>  Lorsque vous utilisez le pilote Paradox sans implémenter le moteur de base de données Borland, les instructions ALTER TABLE ne sont pas pris en charge ; uniquement lire et ajoutez les instructions sont autorisées.
