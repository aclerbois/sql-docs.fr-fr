---
title: 'SQL à C: horodatage | Documents Microsoft'
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
- timestamp data type [ODBC]
- converting data from SQL to C types [ODBC], timestamp
- data conversions from SQL to C types [ODBC], timestamp
ms.assetid: 6a0617cf-d8c0-4316-8bb4-e6ddb45d7bf1
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5582bf0e3c3b498799c229176b824770eec27433
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-timestamp"></a>SQL à c : Timestamp
L’identificateur pour le type de données SQL ODBC timestamp est :  
  
 SQL_TYPE_TIMESTAMP  
  
 Le tableau suivant présente les types de données à laquelle les données timestamp SQL peuvent être converties à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longueur en octets de caractères<br /><br /> 20 < = *BufferLength* < = longueur d’octet de caractère<br /><br /> *BufferLength* < 20|data<br /><br /> Données tronquées [b]<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > longueur des caractères<br /><br /> 20 < = *BufferLength* < = longueur en caractères<br /><br /> *BufferLength* < 20|data<br /><br /> Données tronquées [b]<br /><br /> Indéfini|Longueur des données de caractères<br /><br /> Longueur des données de caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_TYPE_DATE|Partie horaire de timestamp est égal à zéro [a]<br /><br /> Partie horaire de timestamp est différente de zéro [a]|data<br /><br /> Données tronquées (c)|6 [f]<br /><br /> 6 [f]|n/a<br /><br /> 01 S 07|  
|SQL_C_TYPE_TIME|Partie fractions de secondes de l’horodatage est égal à zéro [a]<br /><br /> Partie fractions de secondes de l’horodatage est différente de zéro [a]|Données [d]<br /><br /> Données tronquées [d], [e]|6 [f]<br /><br /> 6 [f]|n/a<br /><br /> 01 S 07|  
_C_TYPE_TIMESTAMP|Partie fractions d’horodatage n’est pas tronqué [a]<br /><br /> Partie fractions de secondes de l’horodatage est tronquée [a]|Données [e]<br /><br /> Données tronquées [e]|16 [f]<br /><br /> 16 [f]|n/a<br /><br /> 01 S 07|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote part du principe que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] les fractions de secondes de l’horodatage sont tronqués.  
  
 [c] la partie heure de l’horodatage est tronquée.  
  
 [d] la partie de la date de l’horodatage est ignorée.  
  
 [e] la partie fractions de secondes de l’horodatage est tronquée.  
  
 [f] Il s’agit de la taille du type de données C correspondante.  
  
 Lors de l’horodatage de données SQL sont converties en données de type caractère C, la chaîne résultante est dans le «*aaaa*-*mm*-*jj* *hh*:*mm*:*ss*[.*f...*] « format, où jusqu'à neuf chiffres peut être utilisé pour les fractions de seconde. Ce format n’est pas affecté par le paramètre de pays Windows®. (À l’exception de la virgule décimale et les fractions de seconde, tout le format doit être utilisé, quelle que soit la précision du type de données timestamp SQL.)
