---
title: 'Annexe d : les Types de données | Documents Microsoft'
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
- C data types [ODBC], defined
- SQL data types [ODBC], defined
- data types [ODBC]
- data types [ODBC], about data types
ms.assetid: 981d49c3-3531-4543-aa75-5bd9e4f67000
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bea62ceaba74b56087ecbb8fa28c8a0108426380
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appendix-d-data-types"></a>Annexe d : les Types de données
ODBC définit deux ensembles de types de données : SQL des types de données et les types de données C. Types de données SQL indiquent le type de données des données stockées dans la source de données. Types de données C indiquent le type de données des données stockées dans les mémoires tampon d’application.  
  
 Types de données SQL sont définies par chaque SGBD conformément à la norme SQL-92. Pour chaque type de données SQL spécifiée dans la norme SQL-92, ODBC définit un identificateur de type, qui est un **#define** valeur qui est passée en tant qu’argument dans les fonctions ODBC ou retournée dans les métadonnées d’un jeu de résultats. Le SQL-92 uniquement les types de données non pris en charge par ODBC sont bits (le type ODBC SQL_BIT possède des caractéristiques différentes), BIT_VARYING, TIME_WITH_TIMEZONE, TIMESTAMP_WITH_TIMEZONE et NATIONAL_CHARACTER. Les pilotes sont chargés de mapper les types de données SQL de données spécifique à la source pour les identificateurs de type de données SQL ODBC et les identificateurs de type de données spécifique au pilote SQL. Le type de données SQL est spécifié dans le champ SQL_DESC_CONCISE_TYPE d’un descripteur d’implémentation.  
  
 ODBC définit les types de données C et de leurs identificateurs de type ODBC correspondants. Une application spécifie le type de données C de la mémoire tampon qui reçoit les données de jeu de résultats en passant l’identificateur de type C approprié dans le *TargetType* argument dans un appel à **SQLBindCol** ou **SQLGetData**. Elle spécifie le type C de la mémoire tampon contenant un paramètre d’instruction en transmettant l’identificateur de type C approprié dans le *ValueType* argument dans un appel à **SQLBindParameter**. Le type de données C est spécifié dans le champ SQL_DESC_CONCISE_TYPE d’un descripteur de l’application.  
  
> [!NOTE]  
>  Il n’existe aucun type de données C spécifiques au pilote.  
  
 Chaque type de données SQL correspond à un type de données ODBC C. Avant de renvoyer des données à partir de la source de données, le pilote convertit en type de données C spécifié. Avant d’envoyer des données à la source de données, le pilote convertit le type de données C spécifié.  
  
 Cette annexe contient les rubriques suivantes.  
  
-   [Utilisation d’identificateurs de types de données](../../../odbc/reference/appendixes/using-data-type-identifiers.md)  
  
-   [Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md)  
  
-   [Type de données C](../../../odbc/reference/appendixes/c-data-types.md)  
  
-   [Identificateurs et descripteurs des types de données](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)  
  
-   [Identificateurs des pseudo-types](../../../odbc/reference/appendixes/pseudo-type-identifiers.md)  
  
-   [Transfert de données dans leur forme binaire](../../../odbc/reference/appendixes/transferring-data-in-its-binary-form.md)  
  
-   [Conseils pour les types de données d’intervalle et numériques](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md)  
  
-   [Contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)  
  
-   [Taille de colonne, nombres décimaux, longueur en octets du transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)  
  
-   [Conversion de données de SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)  
  
-   [Conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)  
  
 Pour obtenir une explication des types de données ODBC, consultez [des Types de données dans ODBC](../../../odbc/reference/develop-app/data-types-in-odbc.md). Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.
