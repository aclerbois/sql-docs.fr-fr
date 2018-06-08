---
title: Taille de la colonne, des chiffres décimaux, le transfert de la longueur en octets, taille d’affichage | Documents Microsoft
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
- display size of data types [ODBC]
- data types [ODBC], column size
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
- data types [ODBC], transfer octet length
ms.assetid: 723107a1-be08-4ea3-a8c0-b2c45d38d1aa
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f4a63c37dae0e8cbb06f00f5d043576028dd0508
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-size-decimal-digits-transfer-octet-length-and-display-size---odbc"></a>Taille de colonne, des chiffres décimaux, transférer la longueur en octets et afficher la taille - ODBC
Types de données sont caractérisent par leur taille de colonne (ou paramètre), de chiffres décimaux, de longueur et affichent la taille. Les fonctions ODBC suivantes retournent ces attributs pour un paramètre dans une instruction SQL ou pour un type de données SQL sur une source de données. Chaque fonction ODBC retourne un ensemble différent de ces attributs, comme suit :  
  
-   **SQLDescribeCol** retourne la colonne chiffres décimales et de taille des colonnes qu’il décrit.  
  
-   **SQLDescribeParam** retourne le paramètre de taille et decimal chiffres des paramètres, il décrit. **SQLBindParameter** définit le paramètre de taille et decimal chiffres pour un paramètre dans une instruction SQL.  
  
-   Les fonctions de catalogue **SQLColumns**, **SQLProcedureColumns**, et **SQLGetTypeInfo** retournez les attributs d’une colonne dans une table, le jeu de résultats, ou un paramètre de procédure et les attributs du catalogue des types de données dans la source de données. **SQLColumns** renvoie la taille de la colonne, chiffres décimaux et la longueur d’une colonne dans les tables spécifiées (par exemple, la table de base, vue ou une table système). **SQLProcedureColumns** renvoie la taille de la colonne, chiffres décimaux et la longueur d’une colonne dans une procédure. **SQLGetTypeInfo** retourne la taille maximale de la colonne et les chiffres décimaux minimales et maximales d’un type de données SQL sur une source de données.  
  
 Les valeurs retournées par ces fonctions pour la colonne ou la taille du paramètre correspond à « precision » comme définie dans ODBC 2. *x*. Toutefois, les valeurs ne correspondent pas nécessairement aux valeurs retournées dans SQL_DESC_PRECISION ou tout autre champ de descripteur un. Cela vaut également pour les chiffres décimaux, qui correspondent aux « montée en puissance » comme définie dans ODBC 2. *x*. Il ne correspond pas nécessairement aux valeurs retournées dans SQL_DESC_SCALE ou tout autre champ de descripteur un, mais proviennent des champs de descripteur différentes selon le type de données. Pour plus d’informations, consultez [taille de la colonne](../../../odbc/reference/appendixes/column-size.md) et [chiffres décimaux](../../../odbc/reference/appendixes/decimal-digits.md).  
  
 De même, les valeurs de longueur d’octet de transfert ne proviennent pas des SQL_DESC_LENGTH. Elles proviennent de la SQL_DESC_OCTET_LENGTH d’un champ d’un descripteur pour tous les types binaires et caractères. Il n’existe aucun champ de descripteur qui contient ces informations pour les autres types.  
  
 La valeur de taille d’affichage pour tous les types de données correspond à la valeur dans un champ de descripteur unique, colonnes SQL_DESC_DISPLAY_SIZE.  
  
 Champs de descripteur décrivent les caractéristiques d’un jeu de résultats. Champs de descripteur ne contiennent pas de valeurs valides sur les données avant l’exécution des instructions. Les valeurs de colonne, la taille, des chiffres décimaux et afficher la taille retournée par **SQLColumns**, **SQLProcedureColumns**, et **SQLGetTypeInfo**sur l’autre revanche, retourner les caractéristiques des objets de base de données, tels que de la table colonnes et types de données, qui existent dans le catalogue de la source de données. De même, dans son jeu de résultats, **SQLColAttribute** renvoie la taille de la colonne, les chiffres décimaux et transfert la longueur en octets des colonnes de la source de données ; ces valeurs ne sont pas nécessairement les mêmes valeurs dans les champs de descripteur SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_OCTET_LENGTH.  
  
 Pour plus d’informations sur ces champs de descripteur, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Rubriques connexes :  
  
-   [Taille de colonne](../../../odbc/reference/appendixes/column-size.md)  
-   [Nombres décimaux](../../../odbc/reference/appendixes/decimal-digits.md)  
-   [Longueur en octets du transfert](../../../odbc/reference/appendixes/transfer-octet-length.md)  
-   [Taille d’affichage](../../../odbc/reference/appendixes/display-size.md)
