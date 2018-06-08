---
title: Littéraux de l’intervalle | Documents Microsoft
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
- interval literals [ODBC]
- interval data type [ODBC], literals
ms.assetid: f9e6c3c7-4f98-483f-89d8-ebc5680f021b
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e86666c4b956b0f926644f0470d7bdf6e8d8dbff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interval-literals"></a>Littéraux de l’intervalle
ODBC requiert que tous les pilotes prennent en charge la conversion du type de données SQL_CHAR ou SQL_VARCHAR à tous les types de données d’intervalle C. Si la source de données sous-jacente ne prend pas en charge les types de données interval, toutefois, le pilote doit connaître le format correct de la valeur dans le champ SQL_CHAR pour prendre en charge ces conversions. De même, ODBC requiert que n’importe quel type être convertible en SQL_CHAR ou SQL_VARCHAR, par conséquent, un pilote doit savoir quel format un intervalle stocké dans le champ de caractère de ODBC C doit avoir. Cette section décrit la syntaxe des littéraux de l’intervalle, le writer de pilote doit utiliser pour valider les champs SQL_CHAR lors de la conversion vers ou depuis des types d’intervalle C.  
  
> [!NOTE]  
>  La syntaxe complète de la notation BNF de littéraux de l’intervalle est indiquée dans la section [syntaxe de littéral d’intervalle](../../../odbc/reference/appendixes/interval-literal-syntax.md) dans l’annexe c : SQL grammaire.  
  
 Pour passer de littéraux de l’intervalle dans le cadre d’une instruction SQL, une syntaxe de clause d’échappement est définie pour les littéraux de l’intervalle. Pour plus d’informations, consultez [Date, Time et Timestamp littéraux](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 Un littéral d’intervalle est de la forme :  
  
```  
INTERVAL[<sign>] 'value' <interval qualifier>  
```  
  
 où « Intervalle » indique que le littéral de caractère est un intervalle. La connexion peut être plus ou moins ; Il est en dehors de la chaîne de l’intervalle et est facultatif.  
  
 Le qualificateur de l’intervalle peut être un champ de date/heure unique ou être composé de deux champs datetime, sous la forme : \< *champ début*> à \< *fin champ*>.  
  
-   Lorsque l’intervalle est composé d’un champ, le seul champ peut être un champ de seconde non qui peut être accompagné d’une précision de début facultative entre parenthèses. Le champ de date/heure unique peut être également un deuxième champ qui peut être accompagné de la précision facultatif de début, la précision en fractions de seconde facultative entre parenthèses ou les deux. Si une précision de début et une précision en fractions de seconde sont présentes pour un champ de secondes, ils sont séparés par des virgules. Si le champ des secondes a une précision en fractions de seconde, il doit également avoir une précision au début.  
  
-   Lors de l’intervalle est composée de champs de début et de fin, le champ Début est un champ en dehors du second qui peut être accompagné de l’intervalle de précision de champ d’en-tête entre parenthèses. Le champ de fin peut être un champ en dehors du second ou un deuxième champ qui peut être accompagné d’une précision de fractions de secondes d’intervalle entre parenthèses.  
  
 La chaîne de l’intervalle dans *valeur* est placée entre guillemets simples. Il peut être un littéral de l’année-mois ou un littéral d’heure de la journée. Le format de la chaîne de *valeur* est déterminée par les règles suivantes :  
  
-   La chaîne contient une valeur décimale pour chaque champ qui est impliquée par le \< *intervalle* *qualificateur*>.  
  
-   Si la précision de l’intervalle inclut les champs année et mois, les valeurs de ces champs sont séparés par un signe moins.  
  
-   Si la précision de l’intervalle inclut les champs à jour et l’heure, les valeurs de ces champs sont séparés par un espace.  
  
-   Si la précision de l’intervalle inclut les champs heure et l’ordre inférieur (MINUTE et seconde), les valeurs de ces champs sont séparés par un signe deux-points.  
  
-   Si la précision de l’intervalle comprend un deuxième champ et la précision en secondes expresse ou implicite est différente de zéro, le caractère immédiatement avant le premier chiffre de la partie fractionnaire de la deuxième est un point.  
  
-   Aucun champ ne peut être plus de deux chiffres, à l’exception :  
  
    -   La valeur du champ Début peut être aussi longue que la précision expresse ou implicite interval.  
  
    -   La partie fractionnaire du deuxième champ peut être aussi longue que la précision en secondes expresse ou implicite.  
  
    -   Les champs fin suivent les contraintes habituelles du calendrier grégorien. (Consultez [contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md).)  
  
 Le tableau suivant répertorie des exemples de littéraux d’intervalle valide comme étant incluses dans la clause d’échappement ODBC pour les intervalles. La syntaxe de la clause d’échappement est comme suit :  
  
> [!NOTE]  
>  *{INTERVALLE signe intervalle-intervalle-qualificateur de chaîne}*  
  
|Littéral clause escape|Intervalle spécifié|  
|---------------------------|------------------------|  
|{YEAR(4) INTERVALLE '326'}|Spécifie l’intervalle d’années 326. L’intervalle de précision d’en-tête est 4.|  
|{MONTH(3) INTERVALLE '326'}|Spécifie l’intervalle de mois 326. La précision interval est 3.|  
|{DAY(4) INTERVALLE '3261'}|Spécifie un intervalle de jours 3261. L’intervalle de précision d’en-tête est 4.|  
|{HOUR(3) INTERVALLE '163'}|Spécifie un intervalle de jours 163. La précision interval est 3.|  
|{MINUTE(3) INTERVALLE '163'}|Spécifie un intervalle de 163 minutes. La précision interval est 3.|  
|{SECOND(3,2) INTERVALLE '223.16'}|Spécifie un intervalle de secondes 223.16. La précision de début d’intervalle est 3, et la précision en secondes est 2.|  
|{INTERVALLE ' 163-11' ANNÉE (3) MOIS}|Spécifie un intervalle de 163 années et 11 mois. La précision interval est 3.|  
|{INTERVALLE 163 ' 12' DAY (3) À L’HEURE}|Spécifie un intervalle de 163 jours et 12 heures. La précision interval est 3.|  
|{INTERVALLE ' 163 12:39 ' DAY (3) À LA MINUTE}|Spécifie un intervalle de 163 jours, 12 heures et 39 minutes. La précision interval est 3.|  
|{INTERVALLE '163 12:39:59.163' DAY (3) À SECOND(3)}|Spécifie un intervalle de 163 jours, 12 heures, 39 minutes et 59.163 secondes. La précision de début d’intervalle est 3, et la précision en secondes est 3.|  
|{INTERVALLE '163:39' HOUR(3) MINUTE}|Spécifie un intervalle de 163 heures et 39 minutes. La précision interval est 3.|  
|{INTERVALLE '163:39:59.163' HOUR(3) À SECOND(4)}|Spécifie un intervalle de 163 heures, 39 minutes et 59.163 secondes. La précision de début d’intervalle est 3, et la précision en secondes est 4.|  
|{INTERVALLE '163:59.163' MINUTE(3) À SECOND(5)}|Spécifie un intervalle de 163 minutes et 59.163 secondes. La précision de début d’intervalle est 3, et la précision en secondes est 5.|  
|{INTERVALLE - JOUR '16 23:39:56.23' À LA SECONDE}|Spécifie un intervalle de moins de 16 jours, 23 heures, 39 minutes et 56.23 secondes. La précision de début implicite est 2, et la précision en secondes implicite est 6.|  
  
 Le tableau suivant répertorie des exemples de littéraux de l’intervalle non valide :  
  
|Littéral clause escape|Raison pourquoi non valide|  
|---------------------------|------------------------|  
|{HOUR(2) INTERVALLE '163'}|La précision de début d’intervalle est 2, mais la valeur du champ de début est 163.|  
|{SECOND(2,2) INTERVALLE '223.16'}<br /><br /> {SECOND(3,1) INTERVALLE '223.16'}|Dans le premier exemple, la précision de début est trop petite, et dans le deuxième exemple, la précision en secondes est trop petite.|  
|{INTERVALLE '223.16' DEUXIÈME}<br /><br /> {INTERVALLE '223' YEAR}|Étant donné que la précision de début n’est pas spécifiée, la valeur par défaut 2, qui est trop petit pour contenir le littéral spécifié.|  
|{INTERVALLE '22.1234567' DEUXIÈME}|La précision en secondes n’est pas spécifiée, afin de la valeur par défaut est 6. Le littéral a sept chiffres après la virgule décimale.|  
|{INTERVALLE ' 163-13' ANNÉE (3) MOIS}<br /><br /> {INTERVALLE 163 ' DAY (3) À L’HEURE DE 65'}<br /><br /> {INTERVALLE '163 62:39' DAY (3) À LA MINUTE}<br /><br /> {INTERVALLE '163 12:125:59.163' DAY (3) À SECOND(3)}<br /><br /> {INTERVALLE '163:144' HOUR(3) MINUTE}<br /><br /> {INTERVALLE '163:567:234.163' HOUR(3) À SECOND(4)}<br /><br /> {INTERVALLE '163:591.163' MINUTE(3) À SECOND(5)}|Le champ de fin ne suit pas les règles du calendrier grégorien.|
