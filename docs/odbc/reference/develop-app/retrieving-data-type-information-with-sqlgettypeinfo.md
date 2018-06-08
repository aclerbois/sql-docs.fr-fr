---
title: La récupération des données de Type d’informations avec SQLGetTypeInfo | Documents Microsoft
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
- SQL data types [ODBC], identifiers
- SQLGetTypeInfo function [ODBC], retrieving data type information
- retrieving data type information [ODBC]
- type identifiers [ODBC], SQL
- identifiers [ODBC], SQL type
- SQL type identifiers [ODBC]
ms.assetid: d4f8b152-ab9e-4d05-a720-d10a08a6df81
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d764d8470343d67bd37c1ef7ce5dcf15962079e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieving-data-type-information-with-sqlgettypeinfo"></a>La récupération des informations de Type de données avec SQLGetTypeInfo
Étant donné que les mappages de types de données SQL sous-jacente aux identificateurs de type ODBC sont approximatifs, ODBC fournit une fonction (**SQLGetTypeInfo**) via lequel un pilote peut complètement décrivent chaque type de données SQL dans la source de données. Cette fonction retourne un jeu de résultats, chaque ligne qui décrit les caractéristiques d’un type de données, telles que le nom, identificateur de type, la précision, échelle et possibilité de valeur null.  
  
 Ces informations sont généralement utilisées par les applications génériques qui autorise l’utilisateur à créer et modifier des tables. Appel de ces applications **SQLGetTypeInfo** pour récupérer les informations de type de données et de présenter tout ou partie de celui-ci à l’utilisateur. De telles applications doivent connaître deux choses :  
  
-   Plusieurs types de données SQL peut mapper à un identificateur de type unique, ce qui peut rendre difficile de déterminer le type de données à utiliser. Pour résoudre ce problème, le jeu de résultats est trié par l’identificateur de type et ensuite par proximité à la définition de l’identificateur de type. En outre, les types de données : source de données sont prioritaires sur les types de données définis par l’utilisateur. Par exemple, supposons qu’une source de données définit les types de données entier et le compteur pour être identiques, sauf que le compteur est à incrémentation automatique. Supposons également que le type défini par l’utilisateur WHOLENUM est un synonyme d’entier. Chacun de ces types est mappé à SQL_INTEGER. Dans le **SQLGetTypeInfo** jeu de résultats, entier s’affiche en premier, suivie de WHOLENUM et puis de compteur. WHOLENUM apparaît après entier, car il est défini par l’utilisateur, mais avant de compteur, car il plus correspond à la définition de la SQL_INTEGER tapez identificateur.  
  
-   ODBC ne définit pas de noms de types de données pour une utilisation dans **CREATE TABLE** et **ALTER TABLE** instructions. Au lieu de cela, l’application doit utiliser le nom retourné dans la colonne TYPE_NAME du jeu de résultats retourné par **SQLGetTypeInfo**. La raison en est que bien que la plupart des SQL ne change pas en grande partie sur SGBD, les noms de types de données varient considérablement. Au lieu d’obliger les pilotes pour analyser des instructions SQL et remplacez les noms de types de données standard avec des noms de type de données propres au SGBD, ODBC exige des applications pour utiliser les noms propres au SGBD en premier lieu.  
  
 Notez que **SQLGetTypeInfo** ne décrit pas nécessairement tous les types de données une application peut rencontrer. En particulier, les jeux de résultats peuvent contenir des types de données pas directement pris en charge par la source de données. Par exemple, les types de données des colonnes dans les jeux de résultats retournés par les fonctions de catalogue sont définies par ODBC, et ces types de données ne peuvent pas être pris en charge par la source de données. Pour déterminer les caractéristiques des types de données dans un jeu de résultats, une application appelle **SQLColAttribute**.
