---
title: Conversions de types de données | Documents Microsoft
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
- data types [ODBC], conversions
- SQL data types [ODBC], conversions
- converting data [ODBC]
- converting data types [ODBC]
- C data types [ODBC], conversions
ms.assetid: d311fe1c-d882-4136-9fa5-220a4121e04c
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0501e4bf627d8dafddfbf5020345d43135af9d6d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-conversions"></a>Conversions de types de données
Données qui peuvent être converties à partir d’un type à un autre à un des quatre fois : lorsque données sont transférées de la variable d’une application vers un autre C à C, lorsque les données dans une variable d’application sont envoyées à un paramètre d’instruction C en SQL, lorsque les données dans une colonne de jeu de résultats sont retournées dans une variable d’application (SQL vers C), et lorsque les données sont transférées à partir de la source de données d’une colonne à un autre SQL (SQL).  
  
 Toute conversion se produit lorsque les données sont transférées à partir de la variable d’une application à l’autre est en dehors de la portée de ce document.  
  
 Lorsqu’une application lie une variable à un paramètre de colonne ou une instruction de jeu de résultats, l’application spécifie implicitement une conversion de type de données dans son choix du type de données de la variable d’application. Par exemple, qu'une colonne contient des données de type entier. Si l’application lie une variable entière à la colonne, elle spécifie qu’aucune conversion n’effectuée ; Si l’application lie une variable de caractère à la colonne, il spécifie que les données d’être convertie à partir d’entier en caractère.  
  
 ODBC définit comment les données sont converties entre chaque type de données SQL et C. En fait, ODBC prend en charge toutes les conversions raisonnables, tels que des caractères à entier et float et ne prend pas en charge les conversions mal définies, tels que float en date. Pilotes sont requis pour prendre en charge toutes les conversions pour chaque type de données SQL que pris en charge. Pour obtenir une liste complète des conversions entre des types de données SQL et C, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) et [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) annexe d : Types de données.  
  
 ODBC définit également une fonction scalaire de conversion des données à partir d’un type de données SQL à un autre. Le **convertir** fonction scalaire est mappée par le pilote à la fonction scalaire sous-jacent ou des fonctions définies pour effectuer des conversions dans la source de données. Étant donné que cette fonction est mappée aux fonctions de propres au SGBD, ODBC ne définit pas le fonctionnement de ces conversions ou les conversions doivent être pris en charge. Une application découvre les conversions sont prises en charge par une source spécifique de pilote et de données via les options SQL_CONVERT dans **SQLGetInfo**. Pour plus d’informations sur la **convertir** fonction scalaire, consultez [les séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) et [fonction de Conversion de Type de données explicite](../../../odbc/reference/appendixes/explicit-data-type-conversion-function.md).
