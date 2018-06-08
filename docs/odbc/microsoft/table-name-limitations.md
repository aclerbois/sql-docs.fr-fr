---
title: Limitations de nom de table | Documents Microsoft
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
- ODBC SQL grammar, table name limitations
- table name limitations [ODBC]
- Excel driver [ODBC], table name limitations
ms.assetid: d9843e4b-1d05-4d5a-9dc3-ee9ec59edb97
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3c33928107e3094e0e2116170b79352268ec9964
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-name-limitations"></a>Limites de nom de table
Les noms de table peuvent contenir des caractères valides (par exemple, des espaces). Si les noms de tables contiennent des caractères à l’exception des lettres, des chiffres et des traits de soulignement, le nom doit être délimité en le plaçant entre guillemets précédent (').  
  
 Lorsque le pilote Microsoft Excel est utilisé, et un nom de table n’est pas qualifié par une référence de base de données, la base de données par défaut est implicite. Si un nom dans Microsoft Excel inclut le « ! » caractères, il est automatiquement converti vers le caractère « $» à la place.  
  
 Le nom de table Microsoft Excel qui fait référence à \<filename > est pris en charge pour Microsoft Excel 3.0 et 4.0 fichiers. Le nom de table Microsoft Excel qui fait référence à \<classeur-name > est pris en charge pour les fichiers Microsoft Excel 5.0, 7.0 ou 97.  
  
 Lorsque le pilote dBASE est utilisé, les caractères avec une valeur ASCII supérieure à 127 sont convertis en traits de soulignement.  
  
 Lorsque le pilote Microsoft Access est utilisé, le nom de table est limité à 64 caractères.  
  
 Lorsque dBASE, le pilote Microsoft Excel 3.0 ou 4.0, Paradox, ou du texte est utilisé, MS-DOS des mots clés spéciaux CON, AUX, LPT1, LPT2 et ne doivent pas être utilisés comme noms de table.
