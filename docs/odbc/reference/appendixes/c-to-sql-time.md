---
title: 'C en SQL : temps | Documents Microsoft'
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
- data conversions from C to SQL types [ODBC], time
- time data type [ODBC]
- converting data from c to SQL types [ODBC], time
ms.assetid: a8da43c9-d9a5-45e5-bd9a-1dd633db2ee0
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 381a0ee578b17c37c5646e495025a17538956085
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-to-sql-time"></a>C en SQL : heure
L’identificateur pour le type de données ODBC C heure est :  
  
 SQL_C_TYPE_TIME  
  
 Le tableau suivant présente les types de données à laquelle les données C de temps peuvent être converties à l’ODBC SQL. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longueur d’octet de colonne > = 8<br /><br /> Colonne de longueur d’octet < 8<br /><br /> Valeur de données n’est pas une heure valide|n/a<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longueur de colonne > = 8<br /><br /> Colonne de longueur < 8 caractères<br /><br /> Valeur de données n’est pas une heure valide|n/a<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_TIME|Valeur de données est une heure valide<br /><br /> Valeur de données n’est pas une heure valide|n/a<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Valeur de données est une heure valide [a]<br /><br /> Valeur de données n’est pas une heure valide|n/a<br /><br /> 22007|  
  
 [a] la date de partie de l’horodatage est définie pour la date actuelle et les fractions de seconde partie de l’horodatage est définie à zéro.  
  
 Pour plus d’informations sur les valeurs qui sont valides dans une structure SQL_C_TYPE_TIME, consultez [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md), plus haut dans cette annexe.  
  
 Lorsque les données de temps C sont converties en données SQL de type caractère, les données de caractères résultant sont dans le «*hh*:*mm*:*ss*« format.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion des données de l’heure de données C de type et suppose que la taille du tampon de données est la taille du type de données heure C. La valeur de l’indicateur/longueur est passée dans le *StrLen_or_Ind* argument dans **SQLPutData** et dans la mémoire tampon spécifiée avec la *StrLen_or_IndPtr* argument dans **SQLBindParameter**. Le tampon de données est spécifié avec la *DataPtr* argument dans **SQLPutData** et *ParameterValuePtr* argument dans **SQLBindParameter**.
