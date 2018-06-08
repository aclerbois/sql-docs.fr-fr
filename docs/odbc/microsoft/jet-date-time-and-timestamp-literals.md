---
title: 'Jet : Date, heure et horodatage littéraux | Documents Microsoft'
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
- literals [ODBC], SQL grammar
- SQL grammar [ODBC], literals
- date literals [ODBC]
- timestamp literals [ODBC]
- time literals [ODBC]
ms.assetid: 37db1ae1-ca4e-4cd8-9b47-7f1a38e7fcad
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 25064ec8edc48f99c1e68cd7df7a728ffa7673de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jet-date-time-and-timestamp-literals"></a>Jet : Date, heure et horodatage littéraux
Pour une interopérabilité maximale, les applications doivent passer des littéraux de date au format canonique ODBC à l’aide de la syntaxe de la clause d’échappement :  
  
-   Pour les littéraux de date, {d '*valeur*'}, où *eurs*e est sous la forme « aaaa-mm-jj »  
  
-   Pour les littéraux d’heure, {t '*valeur*'}, où *eurs*e est sous la forme « hh : mm : »  
  
 Pour les littéraux d’horodatage {ts'*valeur*'}, où *eurs*e est sous la forme « aaaa-mm-jj hh : mm : [. f...] ».
