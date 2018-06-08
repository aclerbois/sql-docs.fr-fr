---
title: Syntaxe de littéral d’intervalle | Documents Microsoft
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
- literals [ODBC], interval
- interval literals [ODBC]
- ODBC literals [ODBC], interval
ms.assetid: 2f2d22c1-51d6-4055-9f5a-53bc31e9fea0
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 016a4fa307e9bda697dde5eec81ce606ad07a19b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interval-literal-syntax"></a>Syntaxe de littéral d’intervalle
La syntaxe suivante est utilisée pour les littéraux de l’intervalle dans ODBC.  
  
 *intervalle de littéral :: = intervalle* [+*&#124;*-] *intervalle-qualificateur de chaîne de l’intervalle*  
  
 *chaîne de l’intervalle* :: = *devis* { *année-mois-literal* &#124; *littéral d’heure jour* } *devis*  
  
 *année-mois-literal* :: = *-valeur en années* &#124; [*-valeur en années* -] *-valeur en mois*  
  
 *littéral d’heure jour* :: = *jours d’intervalle de temps* &#124; *intervalle de temps*  
  
 *intervalle de temps jour* :: = *-valeur en jours* [*-valeur en heures* [ :*-valeur en minutes*[ :*-valeur des secondes*]]]  
  
 *intervalle de temps* :: = *-valeur en heures* [ :*-valeur en minutes* [ :*-valeur des secondes* ]]  
  
 &#124;*-valeur en minutes* [ :*-valeur des secondes* ]  
  
 &#124;*valeur des secondes*  
  
 *valeur des années* :: = *-valeur datetime*  
  
 *valeur de mois* :: = *-valeur datetime*  
  
 *valeur de jours* :: = *-valeur datetime*  
  
 *valeur des heures* :: = *-valeur datetime*  
  
 *valeur des minutes* :: = *-valeur datetime*  
  
 *valeur des secondes* :: = *secondes entier* [. [ *fraction de seconde*]]  
  
 *valeur de nombre entier de secondes* :: = *entier non signé*  
  
 *fraction de seconde* :: = *entier non signé*  
  
 *valeur de date/heure* :: = *entier non signé*  
  
 *qualificateur de l’intervalle* :: = *début champ* à *fin-champ* &#124; *champ de date/heure unique*  
  
 *champ de début* :: = *non-seconde--champ datetime* [(*intervalle début champ précision* )]  
  
 *champ de fin* :: = *non-seconde--champ datetime* &#124; deuxième [(*intervalle--secondes-précision fractionnaire*)]  
  
 *champ de date/heure unique* :: = *non-seconde--champ datetime* [(*intervalle début champ précision*)] &#124; deuxième [(*intervalle début champ précision*  [, (*intervalle--secondes-précision fractionnaire*)]  
  
 *champ de date/heure* :: = *non-seconde--champ datetime* &#124; deuxième  
  
 *non-seconde--champ datetime* :: = année &#124; mois &#124; jour &#124; heure &#124; MINUTE  
  
 *intervalle de fractions de seconde-secondes-précision* :: = *entier non signé*  
  
 *intervalle de début-champ précision* :: = *entier non signé*  
  
 *guillemet* :: = '  
  
 *entier non signé* :: = *chiffre...*
