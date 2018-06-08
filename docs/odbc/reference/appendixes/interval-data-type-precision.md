---
title: Précision du Type de données de l’intervalle | Documents Microsoft
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
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- precision [ODBC]
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: eb73bd77-2e7e-4498-a266-4d7c990a0d56
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 44f00ddbbd577731d9485aedc1a63659dfe59414
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interval-data-type-precision"></a>Précision du Type de données intervalle
Précision pour un type de données d’intervalle inclut l’en-tête de précision, la précision de l’intervalle et précision en secondes de l’intervalle.  
  
 Le champ début d’un intervalle est une valeur numérique signée. Le nombre maximal de chiffres pour le champ de début est déterminé par un nombre appelé *précision, interval* qui fait partie de la déclaration de type de données. Par exemple, la déclaration : intervalle HOUR(5) MINUTE a un intervalle de début de précision de 5 ; le champ heure peut prendre les valeurs de –99999 et 99999. La précision interval est contenue dans le champ SQL_DESC_DATETIME_INTERVAL_PRECISION de l’enregistrement de descripteur.  
  
 La liste des champs de type de données d’intervalle est constitué d’est appelée *précision de l’intervalle*. Il n’est pas une valeur numérique, comme le terme « précision » peut impliquer. Par exemple, la précision de l’intervalle du type INTERVAL DAY TO est ensuite la liste par jour, heure, MINUTE, seconde. Il n’existe aucun champ de descripteur qui contient cette valeur ; la précision de l’intervalle peut toujours être déterminée par le type de données d’intervalle.  
  
 N’importe quel type de données d’intervalle qui a un deuxième champ a un *précision à la seconde*. Ceci est le nombre de chiffres autorisés dans la partie fractionnaire de la valeur des secondes. Cela est différent de celui d’autres types de données, où la précision indique le nombre de chiffres avant la virgule décimale. La précision d’un type de données d’intervalle en secondes est le nombre de chiffres après la virgule décimale. Par exemple, si la précision en secondes est définie sur 6, le 123456 dans le champ de fraction serait interprété comme.123456 et le numéro 1230 sont interprétés en tant que.001230. Pour les autres types de données, il est appelé mise à l’échelle. Précision de secondes d’intervalle est contenue dans le champ SQL_DESC_PRECISION du descripteur. Si la précision du composant fractions de secondes de la valeur d’intervalle SQL est supérieure à ce qui peuvent être contenu dans la structure d’intervalle C, il est définies par le pilote si la valeur de fraction de seconde dans l’intervalle SQL est arrondie ou tronquée lorsque converti à la structure d’intervalle C.  
  
 Lorsque le champ SQL_DESC_CONCISE_TYPE est défini pour un type de données d’intervalle, le champ SQL_DESC_TYPE a la valeur SQL_INTERVAL et la valeur SQL_DESC_DATETIME_INTERVAL_CODE est définie sur le code pour le type de données d’intervalle. Le champ SQL_DESC_DATETIME_INTERVAL_PRECISION est automatiquement défini sur la précision de début intervalle par défaut de 2, et le champ SQL_DESC_PRECISION est automatiquement défini sur la précision de secondes d’intervalle par défaut de 6. Si une de ces valeurs n’est pas approprié, l’application doit définir explicitement le champ de descripteur via un appel à **SQLSetDescField**.
