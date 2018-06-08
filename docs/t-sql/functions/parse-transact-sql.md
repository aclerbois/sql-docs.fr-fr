---
title: PARSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PARSE
- PARSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PARSE function
ms.assetid: 6a2dbf10-f692-471b-9458-24d246963049
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 71317463fcfd585067832627f50bd8090fe0ae19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parse-transact-sql"></a>PARSE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne le résultat d'une expression, traduit en type de données demandé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PARSE ( string_value AS data_type [ USING culture ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *string_value*  
 Valeur **nvarchar**(4000) représentant la valeur mise en forme à analyser dans le type de données spécifié.  
  
 *string_value* doit être une représentation valide du type de données demandé, sinon PARSE génère une erreur.  
  
 *data_type*  
 Valeur littérale représentant le type de données demandé pour le résultat.  
  
 *culture*  
 Chaîne facultative qui identifie la culture dans laquelle *string_value* est mise en forme.  
  
 Si l’argument *culture* n’est pas fourni, la langue de la session en cours est utilisée. Cette langue est défini implicitement, ou explicitement à l'aide de l'instruction SET LANGAGE. *culture* accepte n’importe quelle culture prise en charge par le .NET Framework ; elle n’est pas limitée aux langues explicitement prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’argument *culture* n’est pas valide, PARSE génère une erreur.  
  
## <a name="return-types"></a>Types de retour  
 Retourne le résultat de l'expression, traduit en type de données demandé.  
  
## <a name="remarks"></a>Notes   
 Les valeurs NULL passées comme arguments à PARSE sont traitées de deux façons :  
  
1.  Si une constante NULL est passée, une erreur est générée. Une valeur NULL ne peut pas être analysée en un type de données différent d'une manière compatible avec la culture.  
  
2.  Si un paramètre avec une valeur NULL est passé au moment de l'exécution, la valeur NULL est retournée, afin d'éviter l'annulation du traitement entier.  
  
 Utilisez PARSE uniquement pour effectuer une conversion d'une chaîne en un type date/heure ou numérique. Pour les conversions de type général, continuez à utiliser CAST ou CONVERT. N'oubliez pas qu'il existe une certaine surcharge des performances lors de l'analyse de la valeur de chaîne.  
  
 PARSE repose sur la présence du CLR (Common Langage Runtime) .NET Framework.  
  
 Cette fonction ne peut pas être exécutée à distance car elle dépend de la présence du CLR. L'exécution à distance d'une fonction qui nécessite le CLR provoquerait une erreur sur le serveur distant.  
  
 **Informations supplémentaires sur le paramètre data_type**  
  
 Les valeurs du paramètre *data_type* sont limitées aux types répertoriés avec les styles dans le tableau ci-dessous. Les informations sur le style sont fournies pour aider à déterminer les types de modèles autorisés. Pour plus d’informations sur les styles, consultez la documentation du .NET Framework pour les énumérations **System.Globalization.NumberStyles** et **DateTimeStyles**.  
  
|Catégorie|Type|Type .NET Framework|Styles utilisés|  
|--------------|----------|-------------------------|-----------------|  
|Numérique|bigint|Int64|NumberStyles.Number|  
|Numérique|INT|Int32|NumberStyles.Number|  
|Numérique|SMALLINT|Int16|NumberStyles.Number|  
|Numérique|TINYINT|Byte|NumberStyles.Number|  
|Numérique|Décimal|Décimal|NumberStyles.Number|  
|Numérique|NUMERIC|Décimal|NumberStyles.Number|  
|Numérique|FLOAT|Double|NumberStyles.Float|  
|Numérique|REAL|Unique|NumberStyles.Float|  
|Numérique|SMALLMONEY|Décimal|NumberStyles.Currency|  
|Numérique|money|Décimal|NumberStyles.Currency|  
|Date et heure|Date|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|time|TimeSpan|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|DATETIME|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|smalldatetime|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|datetime2|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|datetimeoffset|DateTimeOffset|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
  
 **Informations supplémentaires sur le paramètre culture**  
  
 Le tableau suivant montre les mappages des langues de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux cultures .NET framework.  
  
|Nom complet|Alias|LCID|Culture spécifique|  
|---------------|-----------|----------|----------------------|  
|us_english|Anglais|1033|fr-FR|  
|Deutsch|German|1031|de-DE|  
|Français|Français|1036|fr-FR|  
|日本語|Japonais|1041|ja-JP|  
|Dansk|Danish|1030|da-DK|  
|Español|Espagnol|3082|es-ES|  
|Italiano|Italien|1040|it-IT|  
|Nederlands|Néerlandais|1043|nl-NL|  
|Norsk|Norvégien|2068|nn-NO|  
|Português|Portugais|2070|pt-PT|  
|Suomi|Finlandais|1035|fi|  
|Svenska|Suédois|1053|sv-SE|  
|čeština|Czech|1029|Cs-CZ|  
|magyar|Hongrois|1038|Hu-HU|  
|polski|Polonais|1045|Pl-PL|  
|română|Roumain|1048|Ro-RO|  
|hrvatski|Croate|1050|hr-HR|  
|slovenčina|Slovaque|1051|Sk-SK|  
|slovenski|Slovène|1060|Sl-SI|  
|ελληνικά|Greek|1032|El-GR|  
|български|Bulgare|1026|bg-BG|  
|русский|Russe|1049|Ru-RU|  
|Türkçe|Turc|1055|Tr-TR|  
|British|British English|2057|en-GB|  
|eesti|Estonien|1061|Et-EE|  
|latviešu|Letton|1062|lv-LV|  
|lietuvių|Lituanien|1063|lt-LT|  
|Português (Brasil)|Brésilien|1046|pt-BR|  
|繁體中文|Chinois traditionnel|1028|zh-TW|  
|한국어|Coréen|1042|Ko-KR|  
|简体中文|Chinois simplifié|2052|zh-CN|  
|Arabe|Arabe|1025|ar-SA|  
|ไทย|Thaïlandais|1054|Th-TH|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-parse-into-datetime2"></a>A. PARSE en datetime2  
  
```  
SELECT PARSE('Monday, 13 December 2010' AS datetime2 USING 'en-US') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-13 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-parse-with-currency-symbol"></a>B. PARSE avec symbole monétaire  
  
```  
SELECT PARSE('€345,98' AS money USING 'de-DE') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
345.98  
  
(1 row(s) affected)  
```  
  
### <a name="c-parse-with-implicit-setting-of-language"></a>C. PARSE avec définition implicite de la langue  
  
```  
-- The English language is mapped to en-US specific culture  
SET LANGUAGE 'English';  
SELECT PARSE('12/16/2010' AS datetime2) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-16 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
  
