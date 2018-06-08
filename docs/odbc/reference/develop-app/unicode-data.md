---
title: Les données Unicode | Documents Microsoft
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
- Unicode [ODBC], data
- data types [ODBC], Unicode
- C data types [ODBC], Unicode
- SQL data types [ODBC], Unicode
ms.assetid: abc28718-e6d9-49fb-97ff-402d50c3c375
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee404891b20f721cec0ea9e56b9eab1e78ad6f8f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unicode-data"></a>Données Unicode
Types de données SQL Unicode sont fournies pour décrire les données qui se trouve au format Unicode en mode natif dans le SGBD. Un type de données C Unicode est fourni pour permettre à une application lier des données dans une mémoire tampon Unicode. Le Gestionnaire de pilotes peut convertir les données à partir d’un type Unicode C (SQL_C_WCHAR) afin de faciliter la fonction avec un pilote ANSI.  
  
 Une application ODBC 3.0 ou 2. *x* application sera toujours une liaison avec les types de données ANSI. Pour des performances optimales, une application de ODBC 3.5 (ou version ultérieure) doit se lier au type de données C ANSI si le type de colonne SQL est ANSI et doit se lier au type de données Unicode C si le type de colonne SQL est au format Unicode.  
  
 Les indicateurs de type SQL Unicode sont SQL_WCHAR, SQL_WVARCHAR et SQL_WLONGVARCHAR. Les données SQL_WCHAR ont une longueur de chaîne fixe, tandis que SQL_WVARCHAR a une longueur variable avec un maximum de déclaré et SQL_WLONGVARCHAR a une longueur variable avec un maximum dépend de la source de données.  
  
 L’indicateur de type C Unicode est SQL_C_WCHAR. Il s’agit de la valeur par défaut pour chacun des indicateurs de type SQL Unicode. Tous les types SQL peuvent être convertis en SQL_C_WCHAR, et SQL_C_WCHAR peut être convertie à tous les types SQL. Une application peut récupérer les données de trois manières :  
  
-   Récupérer les données en tant que SQL_C_CHAR.  
  
-   Récupérer les données en tant que SQL_C_WCHAR.  
  
-   Déclarer les données en tant que SQL_C_TCHAR. Il s’agit d’une macro qui insère SQL_C_WCHAR si l’application est compilée dans une application Unicode ou insère SQL_C_CHAR s’il est compilé en tant qu’une application ANSI.  
  
 SQL_C_TCHAR est déclarée dans une fonction comme suit :  
  
```  
SQLBindParameter(StatementHandle, 1, SQL_PARAM_INPUT, SQL_C_TCHAR, SQL_WCHAR, NameLen, 0, Name, 0, &Name)  
```  
  
 Lorsque l’application est compilée en tant qu’une application Unicode, la *ValueType* argument serait sera remplacé par SQL_C_TCHAR SQL_C_WCHAR. Lorsque l’application est compilée comme une application ANSI, la *ValueType* argument sera remplacé par SQL_C_CHAR.  
  
 Les pilotes Unicode doivent toujours prendre en charge ANSI les types de données SQL_CHAR. Si une application utilisant un pilote Unicode lie à SQL_CHAR, le Gestionnaire de pilotes ne mappera pas les données SQL_CHAR à SQL_WCHAR. Le pilote Unicode doit accepter les données SQL_CHAR.  
  
 Le Gestionnaire de pilotes stocke le pilote et les noms de source de données au format Unicode et les mappe à la norme ANSI en fonction des besoins. Si un caractère Unicode ne peut pas être mappé à un caractère ANSI (tel qu’il peut se produire si les caractères à partir d’une page de codes qui n’est pas la page de code natif de l’ordinateur sont utilisés dans le pilote et les noms de source de données), les caractères qui ne peuvent pas être convertis sont représentés par un caractère par défaut fourni par le système.
