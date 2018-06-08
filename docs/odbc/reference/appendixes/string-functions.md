---
title: Fonctions de chaîne | Documents Microsoft
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
- functions [ODBC], string functions
- string functions [ODBC]
ms.assetid: 270f669e-8aab-4db0-95a4-f2b3c69538b3
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 03f783cbc287d9c315844e1a9df53187e4e2ba28
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="string-functions"></a>Fonctions de chaîne
Le tableau suivant répertorie les fonctions de manipulation de chaîne. Une application peut déterminer les fonctions de chaîne sont pris en charge par un pilote en appelant **SQLGetInfo** avec un *type d’information* de SQL_STRING_FUNCTIONS.  
  
## <a name="remarks"></a>Notes  
 Arguments notée *exp_chaîne* peut être le nom d’une colonne, une *littéral de chaîne de caractères*, ou le résultat d’une autre fonction scalaire, où le type de données sous-jacent peut être représenté en tant que SQL_CHAR, SQL_VARCHAR ou SQL_LONGVARCHAR.  
  
 Arguments notée *exp_caractère* sont une chaîne de caractères de longueur variable.  
  
 Arguments notée *Démarrer*, *longueur*, ou *nombre* peut être un *littéral numérique* ou le résultat d’une autre fonction scalaire, où le type de données sous-jacent peut être représenté comme les SQL_TINYINT, SQL_SMALLINT ou SQL_INTEGER.  
  
 Les fonctions de chaîne répertoriées ici sont de base 1 ; le premier caractère dans la chaîne est le caractère 1.  
  
 Les fonctions scalaires chaîne BIT_LENGTH, CHAR_LENGTH, CHARACTER_LENGTH, OCTET_LENGTH et POSITION ont été ajoutées dans ODBC 3.0 pour les aligner avec SQL-92.  
  
|Fonction| Description|  
|--------------|-----------------|  
|**ASCII (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne la valeur du code ASCII du caractère à l’extrême gauche de *exp_chaîne* en tant qu’entier.|  
|**BIT_LENGTH (** *exp_chaîne* **)** (ODBC version 3.0)|Retourne la longueur en bits de l'expression de chaîne.<br /><br /> Ne fonctionne pas uniquement pour les types de données string, par conséquent, ne sont pas implicitement convert *exp_chaîne* en chaîne mais retournera la taille (interne) de tout type de données est fourni.|  
|**CHAR (** *code* **)** (ODBC version 1.0)|Retourne le caractère qui a l’ASCII valeur spécifiée par du code *code*. La valeur de *code* doit être comprise entre 0 et 255 ; sinon, la valeur de retour est la source de données.|  
|**CHAR_LENGTH (** *exp_chaîne* **)** (ODBC version 3.0)|Retourne la longueur en caractères de l’expression de chaîne, si l’expression de chaîne est de type caractère ; Sinon, retourne la longueur en octets de l’expression de chaîne (le plus petit entier au moins le nombre de bits divisé par 8). (Cette fonction est identique à la fonction CHARACTER_LENGTH.)|  
|**CHARACTER_LENGTH (** *exp_chaîne* **)** (ODBC version 3.0)|Retourne la longueur en caractères de l’expression de chaîne, si l’expression de chaîne est de type caractère ; Sinon, retourne la longueur en octets de l’expression de chaîne (le plus petit entier au moins le nombre de bits divisé par 8). (Cette fonction est identique à la fonction CHAR_LENGTH.)|  
|**CONCAT (** *string_exp1*,*string_exp2 ***)** (ODBC version 1.0)|Retourne une chaîne de caractères qui est le résultat de la concaténation de *string_exp2* à *string_exp1*. La chaîne résultante dépend de SGBD. Par exemple, si la colonne représentée par *string_exp1* contenait une valeur NULL, DB2 retour NULL, mais SQL Server serait retournerait la chaîne non NULL.|  
|**DIFFÉRENCE (** *string_exp1*,*string_exp2 ***)** (ODBC version 2.0)|Retourne une valeur entière qui indique la différence entre les valeurs retournées par la fonction SOUNDEX pour *string_exp1* et *string_exp2*.|  
|**INSERT (** *string_exp1*, *Démarrer*, *longueur*, *string_exp2 ***)** (ODBC version 1.0)|Retourne un caractère de chaîne où *longueur* caractères ont été supprimés de *string_exp1*, en commençant par *Démarrer*et où *string_exp2* a été insérée dans *exp_chaîne,* commençant à *Démarrer*.|  
|**LCASE (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne une chaîne égale à celle de *exp_chaîne*, avec toutes les majuscules caractères sont convertis en minuscules.|  
|**GAUCHE (** *exp_chaîne*, *nombre ***)** (ODBC version 1.0)|Retourne le plus à gauche *nombre* caractères de *exp_chaîne*.|  
|**LONGUEUR (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne le nombre de caractères dans *exp_chaîne,* à l’exclusion des espaces blancs de fin.<br /><br /> **LONGUEUR** accepte uniquement les chaînes. Par conséquent, la convertit implicitement *exp_chaîne* à une chaîne et retourne la longueur de cette chaîne (pas la taille interne du type de données).|  
|**LOCALISER (** *string_exp1*, *string_exp2*[, *Démarrer*]**)** (ODBC version 1.0)|Retourne la position de départ de la première occurrence de *string_exp1* dans *string_exp2*. La recherche de la première occurrence de *string_exp1* commence par la première position de caractère dans *string_exp2* , sauf si l’argument facultatif, *Démarrer*, est spécifié. Si *Démarrer* est spécifié, la recherche commence par la position de caractère indiquée par la valeur de *Démarrer*. Le premier caractère positionner dans *string_exp2* est indiqué par la valeur 1. Si *string_exp1* n’est trouvé dans *string_exp2*, la valeur 0 est renvoyée.<br /><br /> Si une application peut appeler la fonction scalaire localiser avec le *string_exp1*, *string_exp2*, et *Démarrer* arguments, le pilote retourne SQL_FN_STR_LOCATE lorsque **SQLGetInfo** est appelée avec un *Option* de SQL_STRING_FUNCTIONS. Si l’application peut appeler la fonction scalaire localiser avec uniquement le *string_exp1* et *string_exp2* arguments, le pilote retourne SQL_FN_STR_LOCATE_2 lorsque **SQLGetInfo** est appelée avec un *Option* de SQL_STRING_FUNCTIONS. Les pilotes qui prennent en charge l’appel de la fonction de recherche avec deux ou trois arguments renvoient SQL_FN_STR_LOCATE et SQL_FN_STR_LOCATE_2.|  
|**LTRIM (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne les caractères de *exp_chaîne*, avec des espaces de fin supprimés.|  
|**OCTET_LENGTH (** *exp_chaîne* **)** (ODBC version 3.0)|Retourne la longueur en octets de l'expression de chaîne. Le résultat est le plus petit entier qui n'est pas inférieur au nombre de bits divisé par 8.<br /><br /> Ne fonctionne pas uniquement pour les types de données string, par conséquent, ne sont pas implicitement convert *exp_chaîne* en chaîne mais retournera la taille (interne) de tout type de données est fourni.|  
|**POSITION (** *exp_caractère* **IN** *exp_caractère ***)** (ODBC version 3.0)|Retourne la position de la première expression de caractères dans la deuxième expression de caractères. Le résultat est une valeur numérique exacte avec une précision définie par l’implémentation et une échelle de 0.|  
|**RÉPÉTEZ (** *exp_chaîne,* *nombre ***)** (ODBC version 1.0)|Retourne une chaîne de caractères composée de *exp_chaîne* répété *nombre* fois.|  
|**Remplacer (** *string_exp1*, *string_exp2*, *exp_chaîne3 ***)** (ODBC version 1.0)|Recherche *string_exp1* foroccurrences de *string_exp2*et remplacez *exp_chaîne3*.|  
|**DROITE (** *exp_chaîne*, *nombre ***)** (ODBC version 1.0)|Retourne le plus à droite *nombre* caractères de *exp_chaîne*.|  
|**RTRIM (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne les caractères de *exp_chaîne* avec des espaces de fin supprimés.|  
|**SOUNDEX (** *exp_chaîne* **)** (ODBC 2.0)|Retourne une chaîne de caractères – dépendante de la source de données représentant le son des mots dans *exp_chaîne*. Par exemple, SQL Server retourne un code SOUNDEX de 4 chiffres ; Oracle retourne une représentation phonétique de chaque mot.|  
|**ESPACE (** *nombre* **)** (ODBC 2.0)|Retourne une chaîne de caractères composée de *nombre* des espaces.|  
|**SUBSTRING (** *exp_chaîne*, *Démarrer*, longueur **)** (ODBC version 1.0)|Retourne une chaîne de caractères qui est dérivée de *exp_chaîne*, en commençant à la position de caractère spécifiée par *Démarrer* pour *longueur* caractères.|  
|**UCASE (** *exp_chaîne* **)** (ODBC version 1.0)|Retourne une chaîne égale à celle de *exp_chaîne*, avec toutes les minuscules en majuscules.|
