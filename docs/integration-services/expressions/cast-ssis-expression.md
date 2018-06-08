---
title: Cast (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- CAST function
- cast operator
- converting data types [Integration Services]
- data types [Integration Services], expressions
- data types [Integration Services], converting
ms.assetid: d4e915cc-1c7b-4b2e-93b0-13a8b0cb9242
caps.latest.revision: 61
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 629c4e06a41ad7f15dd1420ca3e7828512a1cf0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cast-ssis-expression"></a>Cast (expression SSIS)
  Convertit explicitement une expression d'un type de données vers un autre. L'opérateur de conversion peut également fonctionner comme opérateur de troncation.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
(type_spec) expression  
  
```  
  
## <a name="arguments"></a>Arguments  
 *type_spec*  
 Type de données [!INCLUDE[ssIS](../../includes/ssis-md.md)] valide.  
  
 *expression*  
 Expression valide.  
  
## <a name="result-types"></a>Types des résultats  
 Type de données de *type_spec*. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="remarks"></a>Notes   
 Le schéma suivant montre les opérations de conversion valides.  
  
 ![Conversions légales et illégales entre types de données](../../integration-services/expressions/media/data-conversion.gif "Conversions légales et illégales entre types de données")  
  
 La conversion vers certains types de données nécessite des paramètres. Le tableau suivant décrit ces types de données et leurs paramètres.  
  
|Type de données|Paramètre| Exemple|  
|---------------|---------------|-------------|  
|DT_STR|*charcount*<br /><br /> *codepage*|L'expression (DT_STR,30,1252) convertit 30 octets, ou 30 caractères codés sur un octet, vers le type de données DT_STR à l'aide de la page de codes 1252.|  
|DT_WSTR|*Charcount*|L'expression (DT_WSTR,20) convertit 20 paires d'octets, ou 20 caractères Unicode, vers le type de données DT_WSTR.|  
|DT_BYTES|*Bytecount*|L'expression (DT_BYTES,50) convertit 50 octets vers le type de données DT_BYTES.|  
|DT_DECIMAL|*Échelle*|L'expression (DT_DECIMAL,2) convertit une valeur numérique dans le type de données DT_DECIMAL avec une échelle égale à 2.|  
|DT_NUMERIC|*Précision*<br /><br /> *Échelle*|L'expression (DT_NUMERIC,10,3) convertit une valeur numérique dans le type de données DT_NUMERIC avec une précision de 10 et une échelle de 3.|  
|DT_TEXT|*Codepage*|L'expression (DT_TEXT,1252) convertit une valeur vers le type de données DT_TEXT à l'aide de la page de codes 1252.|  
  
 Lorsque vous convertissez une chaîne vers un type de données DT_DATE ou vice versa, les paramètres régionaux de la transformation sont utilisés. Toutefois, la date se présente dans le format ISO AAAA-MM-JJJ, que les préférences des paramètres régionaux utilisent ou non le format ISO.  
  
> [!NOTE]  
>  Pour convertir une chaîne en un type de données date autre que DT_DATE, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Si la page de codes est une page de codes de caractères multi-octets, le nombre d'octets et de caractères peut différer. La conversion d’un type de données (transtypage) DT_WSTR en DT_STR avec la même valeur *charcount* peut provoquer la troncation des caractères finaux dans la chaîne convertie. Si l’espace de stockage disponible est suffisant dans la colonne de la table de destination, définissez la valeur du paramètre *charcount* de façon à refléter le nombre d’octets nécessaires à la page de codes multioctets. Par exemple, si vous convertissez des données de type caractère en un type de données DT_STR en utilisant la page de codes 936, vous devez définir le paramètre *charcount* sur une valeur jusqu’à deux fois supérieure au nombre de caractères attendus dans les données. Si vous convertissez des données de type caractère en utilisant la page de codes UTF-8, vous devez attribuer au paramètre *charcount* une valeur jusqu’à quatre fois supérieure.  
  
 Pour plus d’informations sur la structure des types de données date, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="ssis-expression-examples"></a>Exemples d'expressions SSIS  
 L'exemple suivant convertit une valeur numérique en un entier.  
  
```  
(DT_I4) 3.57  
```  
  
 L'exemple suivant convertit un entier en une chaîne de caractères à l'aide de la page de codes 1252.  
  
```  
(DT_STR,1,1252)5  
```  
  
 L'exemple suivant convertit une chaîne de trois caractères en caractères codés sur deux octets.  
  
```  
(DT_WSTR,3)"Cat"  
```  
  
 L'exemple suivant convertit un entier en un nombre décimal avec une échelle de deux.  
  
```  
(DT_DECIMAl,2)500  
```  
  
 L'exemple suivant convertit un entier en une valeur numérique avec une précision de sept et une échelle de trois.  
  
```  
(DT_NUMERIC,7,3)4000  
```  
  
 L’exemple suivant convertit les valeurs de la colonne **FirstName** , définie avec un type de données **nvarchar** et une longueur de 50, en une chaîne de caractères en utilisant la page de codes 1252.  
  
```  
(DT_STR,50,1252)FirstName  
```  
  
 L’exemple suivant convertit les valeurs de la colonne **DateFirstPurchase** de type DT_DBDATE, en une chaîne de caractères Unicode avec une longueur de 20.  
  
```  
(DT_WSTR,20)DateFirstPurchase  
```  
  
 L'exemple suivant convertit le littéral de chaîne "True" en une valeur booléenne.  
  
```  
(DT_BOOL)"True"  
```  
  
 Cet exemple convertit un littéral de chaîne en DT_DBDATE.  
  
```  
(DT_DBDATE) "1999-10-11"  
```  
  
 Cet exemple convertit un littéral de chaîne en type de données DT_DBTIME2 qui utilise 5 chiffres pour les fractions de seconde. (Le type de données DT_DBTIME2 peut avoir entre 0 et 7 chiffres spécifiés pour les fractions de seconde.)  
  
```  
(DT_DBTIME2, 5) "16:34:52.12345"  
```  
  
 Cet exemple convertit un littéral de chaîne en type de données DT_DBTIMESTAMP2 qui utilise 4 chiffres pour les fractions de seconde. (Le type de données DT_DBTIMESTAMP2 peut avoir entre 0 et 7 chiffres spécifiés pour les fractions de seconde.)  
  
```  
(DT_DBTIMESTAMP2, 4) "1999-10-11 16:34:52.1234"  
```  
  
 Cet exemple convertit un littéral de chaîne en type de données DT_DBTIMESTAMPOFFSET qui utilise 7 chiffres pour les fractions de seconde. (Le type de données DT_DBTIMESTAMPOFFSET peut avoir entre 0 et 7 chiffres spécifiés pour les fractions de seconde.)  
  
```  
(DT_DBTIMESTAMPOFFSET, 7) "1999-10-11 16:34:52.1234567 + 5:35"  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)   
 [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md)  
  
  
