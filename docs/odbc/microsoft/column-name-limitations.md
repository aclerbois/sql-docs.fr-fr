---
title: Limites de nom de colonne | Documents Microsoft
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
- desktop database drivers [ODBC], column names
- ODBC desktop database drivers [ODBC], column names
ms.assetid: 5a339f61-c52f-40ad-8deb-d785f72753d4
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3f3f384b9a2080ab683c8148effe7c6a9a13fcd6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-name-limitations"></a>Limites de nom de colonne
Les noms de colonne peuvent contenir des caractères valides (par exemple, des espaces). Si les noms de colonnes contiennent des caractères à l’exception des lettres, des chiffres et des traits de soulignement, le nom doit être délimité en le plaçant entre guillemets précédent (').  
  
 Lorsque le pilote Microsoft Access ou Microsoft Excel est utilisé, les noms de colonne sont limités à 64 caractères et les noms plus génèrent une erreur. Lorsque le pilote Paradox est utilisé, le nom de colonne maximale est de 25 caractères. Lorsque le pilote de texte est utilisé, le nom de colonne maximale est de 64 caractères et les noms plus longs sont tronqués.  
  
 Lorsque le pilote dBASE est utilisé, les caractères avec une valeur ASCII supérieure à 127 sont convertis en traits de soulignement.  
  
 Lorsque le pilote Microsoft Excel est utilisé, si les noms de colonnes sont présents, ils doivent être dans la première ligne. Un nom qui, dans Microsoft Excel, utilisez le « ! » caractères doivent être encadrée de guillemets précédent ('). Le « ! » caractère est converti vers le caractère « $», car le « ! » caractère n’est pas valide dans un nom d’ODBC, même lorsque le nom est placé entre guillemets précédent. Tous les autres caractères valides de Microsoft Excel (à l’exception de la barre verticale (&#124;)) peut être utilisé dans un nom de colonne, y compris les espaces. Un identificateur délimité doit être utilisé pour un nom de colonne Microsoft Excel à inclure un espace. Noms de colonnes non spécifiée seront remplacés par le nom généré par le pilote, par exemple, « Col1 » pour la première colonne.  
  
 La barre verticale (&#124;) ne peut pas être utilisé dans un nom de colonne, si le nom est entouré de guillemets précédent ou non.  
  
 Lorsque le pilote de texte est utilisé, le pilote fournit un nom par défaut, si un nom de colonne n’est pas spécifié. Par exemple, le pilote appelle la première colonne (F1), la seconde colonne F2 et ainsi de suite.
