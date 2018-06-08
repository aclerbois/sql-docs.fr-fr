---
title: Règles pour les Conversions | Documents Microsoft
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
- numeric data type [ODBC], literals
- conversions with numeric literals [ODBC]
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: 89f846a3-001d-496a-9843-ac9c38dc1762
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d8020afb215724e05201ac0a2a23ff0f39f1642a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rules-for-conversions"></a>Règles pour les Conversions
Les règles de cette section s’appliquent pour les conversions de littéraux numériques. Dans le cadre de ces règles, les termes suivants sont définis :  
  
-   *Stocker l’affectation :* lors de l’envoi des données dans une colonne de table dans une base de données. Cela se produit lors des appels à **SQLExecute**, **SQLExecDirect**, et **SQLSetPos**. Lors de l’attribution de magasin « target » fait référence à une colonne de base de données, et « source » fait référence aux données dans les mémoires tampon d’application.  
  
-   *Affectation de récupération :* lors de la récupération des données à partir de la base de données dans des tampons de l’application. Cela se produit lors des appels à **SQLFetch**, **SQLGetData**, **SQLFetchScroll**, et **SQLSetPos**. Lors de l’affectation de la récupération, « target » fait référence aux mémoires tampons de l’application et « source » fait référence à la colonne de base de données.  
  
-   *CS :* la valeur de la source du caractère.  
  
-   *NT :* la valeur dans la cible numérique.  
  
-   *NS :* la valeur de la source de type numérique.  
  
-   *CT :* la valeur dans la cible de caractère.  
  
-   Précision d’un littéral numérique exact : le nombre de chiffres qu’il contient.  
  
-   L’échelle d’un littéral numérique exact : le nombre de chiffres à droite de la période expresse ou implicite.  
  
-   La précision d’un littéral numérique approximatif : la précision de la mantisse.  
  
## <a name="character-source-to-numeric-target"></a>Source du caractère à la cible numérique  
 Voici les règles de conversion d’une source de caractère (CS) vers une cible numérique (NT) :  
  
1.  Remplacez CS avec la valeur obtenue en supprimant les espaces de début ou de fin dans CS. Si CS n’est pas valide *littéral numérique*, valeur SQLSTATE 22018 (valeur de caractère non valide pour la spécification de la casse) est retournée.  
  
2.  Remplacez CS avec la valeur obtenue en supprimant les zéros non significatifs avant la virgule décimale, les zéros de fin après la virgule décimale, ou les deux.  
  
3.  Convertir CS NT. Si la conversion entraîne une perte de chiffres significatifs, SQLSTATE 22003 (valeur numérique hors limites) est retournée. Si la conversion entraîne la perte de chiffres non significatives, SQLSTATE 01 s 07 (troncation fractionnelle) est retournée.  
  
## <a name="numeric-source-to-character-target"></a>Source de type numérique à caractère cible  
 Voici les règles de conversion d’une source de type numérique (NS) pour une cible de caractère (CT) :  
  
1.  Soit la longueur en caractères de CT LT Pour l’attribution de la récupération, LT est égale à la longueur de la mémoire tampon en caractères moins le nombre d’octets dans le caractère de fin de la valeur null pour ce jeu de caractères.  
  
2.  Cas :  
  
    -   Si NS est un type numérique exact, puis laissez YP égale à la chaîne de caractères le plus court qui est conforme à la définition de *littéral numérique exact* telle que l’échelle de YP est identique à l’échelle de NS, et la valeur interprétée de YP est la valeur absolue de NS.  
  
    -   Si NS est un type numérique approximatif, faites-le YP être une chaîne de caractères comme suit :  
  
         Cas :  
  
         Si NS est égal à 0, YP est 0.  
  
         Soit la chaîne de caractères le plus court qui est conforme à la définition exacte-YSN*littéral numérique* et interprétée dont la valeur est la valeur absolue de NS. Si la longueur de YSN est inférieur à (*précision* + 1) du type de données NS, puis laissez YP égal YSN.  
  
         Dans le cas contraire, YP est la chaîne de caractères le plus court qui est conforme à la définition de *littéral numérique approximatif* interprétée dont la valeur est la valeur absolue de NS et dont *mantisse* se compose d’un seul *chiffre* c'est-à-dire pas « 0 », suivi d’un *période* et un *entier non signé*.  
  
3.  Cas :  
  
    -   Si NS est inférieur à 0, puis permettent d’Y être le résultat de :  
  
         '-' &AMP;#124; &AMP;#124; YP  
  
         où «&#124;&#124;» est l’opérateur de concaténation de chaîne.  
  
         Sinon, laissez le Y égal YP.  
  
4.  Soit la longueur en caractères de Y EP.  
  
5.  Cas :  
  
    -   Si EP est égal à LT, CT a la valeur Y.  
  
    -   Si EP est inférieure à LT, CT est définie à Y étendu à droite par un nombre approprié d’espaces.  
  
         Dans le cas contraire (EP > LT), copiez les premiers caractères LT y CT  
  
         Cas :  
  
         S’il s’agit d’une attribution de magasin, retournent l’erreur SQLSTATE 22001 (chaîne de données tronquée à droite).  
  
         S’il s’agit d’affectation de la récupération, retourner l’avertissement SQLSTATE 01004 (chaîne de données tronquée à droite). Lors de la copie entraîne la perte de chiffres fractionnaires (autres que les zéros à droite), elle est définie par le pilote si une des actions suivantes se produit :  
  
         (1) le pilote tronque la chaîne de Y à une échelle appropriée (qui peut être pas également de zéro) et écrit le résultat dans CT  
  
         (2) le pilote arrondit la chaîne dans Y à une échelle appropriée (qui peut être pas également de zéro) et écrit le résultat dans CT  
  
         (3) le pilote ni tronque ni arrondit, mais copie tout simplement les premiers caractères LT de Y dans CT
