---
title: Appels de fonction scalaire | Documents Microsoft
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
- escape sequences [ODBC], scalar function calls
ms.assetid: 10cb4dcf-4cd8-4a56-8725-d080bd3ffe47
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee7070453f7d6303bdaebe15c7bbdd89710d5c28
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="scalar-function-calls"></a>Appels de fonction scalaire
Fonctions scalaires retournent une valeur pour chaque ligne. Par exemple, la fonction scalaire de valeur absolue prend une colonne numérique en tant qu’argument et retourne la valeur absolue de chaque valeur dans la colonne. Est de la séquence d’échappement pour appeler une fonction scalaire  
  
 **{fn***une fonction scalaire* **}**  
  
 où *une fonction scalaire* est une des fonctions répertoriées dans [annexe e : les fonctions scalaires](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md). Pour plus d’informations sur la séquence d’échappement de fonction scalaire, consultez [scalaire séquence d’échappement de fonction](../../../odbc/reference/appendixes/scalar-function-escape-sequence.md) dans l’annexe c : SQL grammaire.  
  
 Par exemple, les instructions SQL suivantes créent le même jeu de résultats du client en majuscules des noms. La première instruction utilise la syntaxe de la séquence d’échappement. La deuxième instruction utilise la syntaxe native pour Ingres pour OS/2 et n’est pas interopérable.  
  
```  
SELECT {fn UCASE(Name)} FROM Customers  
  
SELECT uppercase(Name) FROM Customers  
```  
  
 Une application peut combiner des appels aux fonctions scalaires qui utilisent la syntaxe natif et les appels aux fonctions scalaires qui utilisent la syntaxe ODBC. Par exemple, supposons que les noms dans la table des employés sont stockés en tant que nom de famille, une virgule et un prénom. L’instruction SQL suivante crée un jeu de résultats des noms d’employés dans la table Employee. L’instruction utilise la fonction scalaire ODBC **sous-chaîne** et la fonction scalaire SQL Server **CHARINDEX** et s’exécute correctement uniquement sur le serveur SQL Server.  
  
```  
SELECT {fn SUBSTRING(Name, 1, CHARINDEX(',', Name) – 1)} FROM Customers  
```  
  
 Pour une interopérabilité maximale, les applications doivent utiliser le **convertir** fonction scalaire pour vous assurer que la sortie d’une fonction scalaire est le type requis. Le **convertir** fonction convertit les données à partir d’un type de données SQL pour le type de données SQL spécifié. La syntaxe de la **convertir** fonction  
  
 **CONVERTIR (** *value_exp* **,** *data_type ***)**  
  
 où *value_exp* est un nom de colonne, le résultat d’une autre fonction scalaire ou une valeur littérale, et *data_type* est un mot clé qui correspond à la **#define** nom qui est utilisé par un identificateur de type de données SQL, tel que défini dans [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md). Par exemple, l’instruction SQL suivante utilise la **convertir** (fonction) pour vous assurer que la sortie de la **CURDATE** fonction est une date, au lieu d’un horodateur ou des caractères de données :  
  
```  
INSERT INTO Orders (OrderID, CustID, OpenDate, SalesPerson, Status)  
   VALUES (?, ?, {fn CONVERT({fn CURDATE()}, SQL_DATE)}, ?, ?)  
```  
  
 Pour déterminer les fonctions scalaires sont prises en charge par une source de données, une application appelle **SQLGetInfo** avec les options SQL_CONVERT_FUNCTIONS, SQL_NUMERIC_FUNCTIONS, SQL_STRING_FUNCTIONS, SQL_SYSTEM_FUNCTIONS et SQL_TIMEDATE_FUNCTIONS. Pour déterminer quelles opérations de conversion sont pris en charge par le **convertir** (fonction), une application appelle **SQLGetInfo** avec les options qui commencent par SQL_CONVERT.
