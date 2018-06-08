---
title: Littéraux (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
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
- string literals
- numeric literals [Integration Services]
- Boolean literals
- expressions [Integration Services], literals
- literals [Integration Services]
- mapping literals [Integration Services]
ms.assetid: a980cd52-54ef-4b9c-b00c-e6807cf8e01f
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 34382ce333db1d7dda8e72b4a7eeb6a2b8196e74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="numeric-string-and-boolean-literals"></a>Littéraux numériques, booléens et de chaîne
 Les expressions peuvent contenir des littéraux numériques, booléens et de chaîne. L'évaluateur d'expression prend en charge divers littéraux numériques tels que les entiers, les décimaux et les constantes en virgule flottante. L'évaluateur d'expression prend également en charge les suffixes longs et flottants, qui spécifient comment il doit gérer les valeurs, ainsi que la notation scientifique dans les littéraux numériques.  
  
## <a name="numeric-literals"></a>Littéraux numériques  
 L'évaluateur d'expression prend en charge les types de données numériques intégraux et non intégraux. Il prend également en charge les identificateurs de lignage (identificateurs numériques uniques des éléments de package). Les identificateurs de lignage sont des nombres, mais vous ne pouvez pas les utiliser dans des opérations mathématiques.  
  
 L'évaluateur d'expression prend en charge les suffixes, qui vous permettent d'indiquer comment celui-ci doit traiter le littéral numérique. Par exemple, vous pouvez indiquer que l'entier 37 doit être traité en tant que type de données integer long en écrivant « 37L » ou « 37l ».  
  
 Le tableau suivant décrit les suffixes des littéraux numériques.  
  
|Suffixe|Description|  
|------------|-----------------|  
|L ou l|Littéral numérique long.|  
|U ou u|Littéral numérique non signé.|  
|E ou e|Exposant en notation scientifique|  
  
 Le tableau suivant décrit les éléments des expressions numériques et leurs expressions régulières.  
  
|Élément d'expression|Expression régulière|Description|  
|------------------------|------------------------|-----------------|  
|Chiffres exprimés par la lettre « D ».|[0-9]|Tout chiffre.|  
|Notation scientifique exprimée par la lettre « E ».|[Ee][+-]?{D}+|« e » majuscule ou minuscule, éventuellement le signe « + » ou « - », et un ou plusieurs chiffres tels que définis pour « D ».|  
|Suffixe entier exprimé par « IS ».|(([lL]?[uU]?)&#124;([uU]?[lL]?))|Facultativement, « u » et « l » majuscules ou minuscules ou une combinaison de « u » et de « l ». « U » ou « u » indique une valeur non signée. « L » ou « l » indique une valeur longue.|  
|Suffixe flottant exprimé par « FS ».|([f&#124;F]&#124;[l&#124;L])|« f » ou « l » majuscule ou minuscule. « F » ou « f » indique une valeur flottante (type de données DT_R4). « L » ou « l » indique une valeur longue (type de données DT_R8).|  
|Chiffre hexadécimal exprimé par la lettre « H ».|[a-fA-F0-9]|Tout chiffre hexadécimal.|  
  
 Le tableau suivant décrit les littéraux numériques valides dans le langage d'expression régulière.  
  
|Expression régulière|Description|  
|------------------------|-----------------|  
|{D}+{IS}|Littéral numérique intégral avec au moins un chiffre (D) et éventuellement le suffixe long et/ou le suffixe non signé (IS).  Exemples : 457, 785u, 986L et 7945ul.|  
|{D}+{E}{FS}|Littéral numérique non intégral avec au moins un chiffre (D), la notation scientifique et le suffixe long ou flottant.  Exemples : 4E8l, 13e-2f et 5E+L.|  
|{D}*"."{D}+{E}?{FS}|Littéral numérique non intégral avec une décimale, une fraction décimale d'au moins un chiffre (D), un exposant facultatif (E) et un identificateur flottant ou long (FS). Ce littéral numérique est du type de données DT_R4 ou DT_R8.  Exemples : 6.45E3f, .89E-2l et 1.05E+7F.|  
|{D}+"."{D}*{E}?{FS}|Littéral numérique non intégral avec au moins un chiffre significatif (D), une décimale, un exposant (E) et un identificateur flottant ou long (FS). Ce littéral numérique est du type de données DT_R4 ou DT_R8.  Exemples : 1.E-4f, 4.6E6L et 8.365E+2f.|  
|{D}*.{D}+|Littéral numérique non intégral avec précision et échelle. Il a une décimale et une fraction décimale d'au moins un chiffre (D). Ce littéral numérique est du type de données DT_NUMERIC.  Exemples : 0,9, 5,8 et 0,346.|  
|{D}+.{D}*|Littéral numérique non intégral avec précision et échelle. Il a au moins un chiffre significatif (D) et une décimale. Ce littéral numérique est du type de données DT_NUMERIC.  Exemples : 6,0, 0,2 et 8,0.|  
|#{D}+|Identificateur de lignage. Il est composé du signe dièse ( # ) et d'au moins un chiffre (D). Exemples : #123.|  
|0[xX]{H}+{uU}|Littéral numérique en notation hexadécimale. Il comprend un zéro, un « x » majuscule ou minuscule, au moins un « H » majuscule et, éventuellement, le suffixe non signé. Exemples : 0xFF0A et 0X000010000U.|  
  
 Pour plus d’informations sur les types de données utilisés par l’évaluateur d’expression, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Les expressions peuvent comprendre des littéraux numériques de différents types de données. Lorsque l'évaluateur d'expression évalue ces expressions, il convertit les données vers des types compatibles. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
 Toutefois, la conversion entre certains types de données requiert une conversion explicite. L'évaluateur d'expression dispose de l'opérateur de conversion permettant de réaliser la conversion explicite des types de données. Pour plus d’informations, consultez [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
### <a name="mapping-numeric-literals-to-integration-services-data-types"></a>Mappage des littéraux numériques avec des types de données Integration Services  
 L'évaluateur d'expression effectue les conversions suivantes lorsqu'il évalue les littéraux numériques :  
  
-   Un littéral numérique intégral est mappé avec un type de données integer comme suit :  
  
    |Suffixe|Type du résultat|  
    |------------|-----------------|  
    |None|DT_I4|  
    |U|DT_UI4|  
    |L|DT_I8|  
    |UL|DT_UI8|  
  
    > **IMPORTANT !** Si le suffixe long (« L » ou « l ») est absent, l'évaluateur d'expression mappe les valeurs signées avec le type de données DT_I4 et les valeurs non signées avec le type de données DT_UI4, même si la valeur dépasse le type de données.  
  
-   Un littéral numérique qui comprend un exposant est converti vers le type de données DT_R4 ou DT_R8. Si l'expression comprend le suffixe long, elle est convertie vers le type de données DT_R8 ; si elle comprend le suffixe flottant, elle est convertie vers le type de données DT_R4.  
  
-   Si un littéral numérique non intégral comprend « F » ou « f », il est mappé avec le type de données DT_R4. S'il comprend « L » ou « l » et que le nombre est un entier, il est mappé avec le type de données DT_I8. S'il s'agit d'un nombre réel, il est mappé avec le type de données DT_R8. S'il comprend le suffixe long, il est converti vers le type de données DT_R8.  
  
-   Un littéral numérique non intégral avec précision et échelle est mappé avec le type de données DT_NUMERIC.  
  
## <a name="string-literals"></a>Littéraux de chaîne  
 Les littéraux de chaîne doivent être placés entre guillemets. Le langage d'expression fournit un ensemble de séquences d'échappement pour les caractères généralement placés en mode échappement, tels que les caractères non imprimables et les guillemets.  
  
 Un littéral de chaîne comprend zéro caractère ou plus, placés entre guillemets. Si une chaîne contient des guillemets, ceux-ci doivent être en mode échappement afin que l'expression puisse être analysée. Tout caractère sur deux octets est autorisé dans une chaîne, à l'exception du caractère « \x0000 » car ce caractère est la marque de fin NULL d'une chaîne.  
  
 Les chaînes peuvent comprendre d'autres caractères nécessitant une séquence d'échappement. Le tableau suivant décrit les séquences d'échappement des littéraux de chaîne.  
  
|Séquence d'échappement|Description|  
|---------------------|-----------------|  
|\a|Alerte|  
|\b|Retour arrière|  
|\f|Saut de page|  
|\n|Nouvelle ligne|  
|\r|Retour chariot|  
|\t|Tabulation horizontale|  
|\v|Tabulation verticale|  
|\\"|Guillemets|  
|\\\|Barre oblique inverse|  
|\xhhhh|Caractère Unicode en notation hexadécimale|  
  
## <a name="boolean-literals"></a>Littéraux booléens  
 L’évaluateur d’expression prend en charge les littéraux booléens habituels : **True** et **False**. L'évaluateur d'expression ne respecte pas la casse ; toute combinaison de lettres majuscules et minuscules y est autorisée. Par exemple, « TRUE » fonctionne de la même façon que « True ».  
  
> **REMARQUE :** Dans une expression, un littéral booléen doit être délimité par des espaces.  
  
  
