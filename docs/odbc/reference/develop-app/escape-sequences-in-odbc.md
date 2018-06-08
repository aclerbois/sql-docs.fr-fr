---
title: Séquences d’échappement dans ODBC | Documents Microsoft
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
- escape sequences [ODBC]
- SQL statements [ODBC], escape sequences
- escape sequences [ODBC], about escape sequences
ms.assetid: cf229f21-6c38-4b5b-aca8-f1be0dfeb3d0
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f2a62b9712801d2412385cc191b0649bae69be74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="escape-sequences-in-odbc"></a>Séquences d’échappement dans ODBC
Un nombre de fonctionnalités de langage, telles que les jointures externes et des appels de fonction scalaire, est généralement implémenté par le SGBD. Toutefois, les syntaxes pour ces fonctionnalités sont susceptibles d’être propres au SGBD, même lorsque les syntaxes standards sont définis par les organismes de normalisation différents. Pour cette raison, ODBC définit des séquences d’échappement qui contiennent des syntaxes standards pour les fonctionnalités de langage suivantes :  
  
-   Littéraux de date, time, timestamp et datetime l’intervalle  
  
-   Fonctions scalaires, par exemple, numérique, chaîne et fonctions de conversion de type de données  
  
-   COMME caractère d’échappement de prédicat  
  
-   Jointures externes  
  
-   Appels de procédure  
  
 La séquence d’échappement utilisée par ODBC est la suivante :  
  
```  
  
(extension)  
  
```  
  
## <a name="remarks"></a>Notes  
 La séquence d’échappement est reconnue et analysée par les pilotes qui remplacent les séquences d’échappement avec la grammaire des SGBD spécifiques. Pour plus d’informations sur la syntaxe de séquence d’échappement, consultez [séquences d’échappement ODBC](../../../odbc/reference/appendixes/odbc-escape-sequences.md) dans l’annexe c : SQL grammaire.  
  
> [!NOTE]  
>  Dans ODBC 2. *x*, il s’agit de la syntaxe standard de la séquence d’échappement : **--(\*fournisseur (***-nom du fournisseur***), produit (***-nom du produit***) *** extension*  **\*)--**  
>   
>  En plus de cette syntaxe, une syntaxe sténographique a été définie sous la forme : **{***extension***}**  
>   
>  Dans ODBC 3. *x*, la forme longue de la séquence d’échappement a été déconseillée et la forme abrégée est utilisée exclusivement.  
  
 Étant donné que les séquences d’échappement sont mappés par le pilote pour les syntaxes propres au SGBD, une application peut utiliser la séquence d’échappement ou une syntaxe spécifique au SGBD. Toutefois, les applications qui utilisent la syntaxe spécifique au SGBD ne seront pas interopérables. Lorsque vous utilisez la séquence d’échappement, applications devraient vous assurer que l’attribut d’instruction SQL_ATTR_NOSCAN est désactivée, ce qui est par défaut. Sinon, la séquence d’échappement est envoyée directement à la source de données, où elle entraîne généralement une erreur de syntaxe.  
  
 Pilotes prennent en charge uniquement les séquences d’échappement qui ils peuvent mapper aux fonctionnalités de langage sous-jacent. Par exemple, si la source de données ne prend pas en charge les jointures externes, ni sera le pilote. Pour déterminer les séquences d’échappement sont pris en charge, une application appelle **SQLGetTypeInfo** et **SQLGetInfo**. Pour plus d’informations, consultez la section suivante, [Date, Time et Timestamp littéraux](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Littéraux de date, d’heure et d’horodatage](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md)  
  
-   [Appels de fonctions scalaires](../../../odbc/reference/develop-app/scalar-function-calls.md)  
  
-   [Caractère d’échappement du prédicat LIKE](../../../odbc/reference/develop-app/like-predicate-escape-character.md)  
  
-   [Jointures externes](../../../odbc/reference/develop-app/outer-joins.md)  
  
-   [Appels de procédure](../../../odbc/reference/develop-app/procedure-calls.md)
