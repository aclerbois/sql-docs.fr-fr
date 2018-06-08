---
title: TRANSLATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- TRANSLATE
- TRANSLATE_TSQL
helpviewer_keywords:
- TRANSLATE function
ms.assetid: 0426fa90-ef6d-4d19-8207-02ee59f74aec
caps.latest.revision: 5
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 4e070109028ed446636610b1008b76fc33bf495e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="translate-transact-sql"></a>TRANSLATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Renvoie la chaîne fournie comme premier argument une fois que des caractères spécifiés dans le deuxième argument sont traduits en un jeu de caractères de destination.

## <a name="syntax"></a>Syntaxe   
```
TRANSLATE ( inputString, characters, translations) 
```

## <a name="arguments"></a>Arguments   

inputString   
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de n’importe quel type de caractère (nvarchar, varchar, nchar, char).

caractères   
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de n’importe quel type de caractère contenant des caractères à remplacer.

traductions   
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de caractères qui correspond au deuxième argument en type et en longueur.

## <a name="return-types"></a>Types de retour   
Renvoie une expression de caractères du même type que `inputString` où les caractères du deuxième argument sont remplacés par les caractères correspondants issus du troisième argument.

## <a name="remarks"></a>Notes    

La fonction `TRANSLATE` renvoie une erreur si des caractères et des traductions ont des longueurs différentes. La fonction `TRANSLATE` doit renvoyer l’entrée inchangée si des valeurs NULL sont fournies comme caractères ou arguments de remplacement. Le comportement de la fonction `TRANSLATE` doit être identique à la fonction [REPLACE](../../t-sql/functions/replace-transact-sql.md).   

Le comportement de la fonction `TRANSLATE` est équivalent à l’utilisation de plusieurs fonctions `REPLACE`.

`TRANSLATE` est toujours conscient du classement SC.

## <a name="examples"></a>Exemples   

### <a name="a-replace-square-and-curly-braces-with-regular-braces"></a>A. Remplacer les crochets et les accolades par des parenthèses    
La requête suivante remplace les crochets et les accolades dans la chaîne d’entrée par des parenthèses :
```
SELECT TRANSLATE('2*[3+4]/{7-2}', '[]{}', '()()');
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]
```
2*(3+4)/(7-2)
```

>  [!NOTE]
>  La fonction `TRANSLATE` de cet exemple est équivalente à l’instruction suivante utilisant `REPLACE`, mais beaucoup plus simple : `SELECT REPLACE(REPLACE(REPLACE(REPLACE('2*[3+4]/{7-2}','[','('), ']', ')'), '{', '('), '}', ')');` 


###  <a name="b-convert-geojson-points-into-wkt"></a>B. Convertir les points GeoJSON en WKT    
GeoJSON est un format d’encodage de diverses structures de données géographiques. Avec la fonction `TRANSLATE`, les développeurs peuvent facilement convertir les points GeoJSON au format WKT et vice versa. La requête suivante remplace les crochets et les accolades dans l’entrée par des parenthèses :   
```sql
SELECT TRANSLATE('[137.4, 72.3]' , '[,]', '( )') AS Point,
    TRANSLATE('(137.4 72.3)' , '( )', '[,]') AS Coordinates;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   


|Point  |Coordinates |  
---------|--------- |
(137.4  72.3) |[137.4,72.3] |


## <a name="see-also"></a> Voir aussi
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [Fonctions de chaîne (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   

