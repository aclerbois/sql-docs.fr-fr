---
title: Types de données Microsoft Excel | Documents Microsoft
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
- data types [ODBC], Excel driver
- Excel data types [ODBC]
- Jet-based ODBC drivers [ODBC], Excel driver
- desktop database drivers [ODBC], Excel driver
- ODBC desktop database drivers [ODBC], Excel driver
- Excel driver [ODBC], data types
ms.assetid: 7b44c8e5-0bc3-4912-8a5d-56f4d5562fe6
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3c1a2c6159f8d0d112e2cae5e1de687f8af3cf2e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-excel-data-types"></a>Types de données Microsoft Excel
Le tableau suivant montre comment les types de données du pilote Microsoft Excel sont mappées aux types de données ODBC SQL. Le pilote Microsoft Excel affecte ces types de données aux colonnes de tables Microsoft Excel basés sur les données dans la colonne.  
  
|Type de données Microsoft Excel|Type de données ODBC|  
|-------------------------------|--------------------|  
|Monétaire (Currency)|SQL_NUMERIC|  
|DATETIME|SQL_TIMESTAMP|  
|LOGIQUE|SQL_BIT|  
|NUMBER|SQL_DOUBLE|  
|TEXT|SQL_VARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC SQL. Toutes les conversions dans l’annexe D de la *de référence du programmeur ODBC* sont pris en charge pour les types de données ODBC SQL répertoriées plus haut dans cette rubrique.  
  
 Le tableau suivant présente des limitations sur les types de données Microsoft Excel.  
  
|Type de données| Description|  
|---------------|-----------------|  
|Données chiffrées|Le pilote Microsoft Excel ne peut pas lire les données chiffrées.|  
|Chaînes d’erreur|Le pilote Microsoft Excel ne peut pas retourner une chaîne de caractères pour les valeurs d’erreur Microsoft Excel (# n/a !, #VALUE !, #REF !, #DIV/0 !, #NUM !, #NAME ? et #NULL !), mais retourne une valeur NULL à la place.|  
|LOGIQUE|La valeur dans une colonne logique est retournée dans une mémoire tampon SQL_C_CHAR, 0 ou 1.|  
|NUMBER|Si une colonne d’entiers est créée, les nombres qui sont trop volumineux pour le type de données entier peuvent être entrés et contenant des valeurs non entières de données peuvent être insérées, avec le résultat que la colonne peut être convertie en SQL_DOUBLE.|  
|TEXT|Lorsque les lignes d’une colonne contient plusieurs types de données Microsoft Excel, le pilote ODBC Microsoft Excel affecte le type de données SQL_VARCHAR à la colonne. Il existe une exception à cela : si la colonne contient uniquement deux ou trois des types de données datetime (DATE, TIME et DATETIME), le pilote ODBC Microsoft Excel affecte le type de données SQL_TIMESTAMP à la colonne.<br /><br /> Création d’une colonne de texte de zéro ou de longueur non spécifiée retourne en fait une colonne de 255 octets.<br /><br /> Un littéral de chaîne de caractères peut contenir n’importe quel caractère ANSI (décimal 1-255). Utilisez deux consécutifs apostrophes accolées ('') pour représenter un guillemet simple (').<br /><br /> Insertion d’une valeur NULL dans une colonne avec un type de données autre que SQL_VARCHAR entraîne le type de données de la colonne à modifier pour SQL_VARCHAR.|  
  
 Vous trouverez davantage de contraintes sur les types de données dans [Limitations du Type de données](../../odbc/microsoft/data-type-limitations.md).
