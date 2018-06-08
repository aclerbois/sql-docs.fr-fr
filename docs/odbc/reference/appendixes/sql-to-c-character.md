---
title: 'SQL à c : caractère | Documents Microsoft'
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
- converting data from SQL to C types [ODBC], character
- character data type [ODBC]
- data conversions from SQL to C types [ODBC], character
ms.assetid: 7fdb7f38-b64d-48f2-bcb4-1ca96b2bbdb6
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: db139b1307fa1817e0b9ed709be5ee5db2086756
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-character"></a>SQL à c : caractère
Les identificateurs pour les types de données SQL ODBC caractères sont :  
  
 SQL_CHAR  
  
 SQL_VARCHAR  
  
 SQL_LONGVARCHAR  
  
 SQL_WCHAR  
  
 SQL_WVARCHAR  
  
 SQL_WLONGVARCHAR  
  
 Le tableau suivant présente les types de données à laquelle les données de caractères SQL peuvent être converties à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Longueur en octets de données < *BufferLength*<br /><br /> Longueur en octets de données > = *BufferLength*|data<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
|SQL_C_WCHAR|Longueur des données de caractères < *BufferLength*<br /><br /> Longueur des données de caractères > = *BufferLength*|data<br /><br /> Données tronquées|Longueur des données de caractères<br /><br /> Longueur des données de caractères|n/a<br /><br /> 01004|  
|SQL_C_STINYINT SQL_C_UTINYINT SQL_C_TINYINT SQL_C_SBIGINT SQL_C_UBIGINT SQL_C_SSHORT SQL_C_USHORT SQL_C_SHORT SQL_C_SLONG SQL_C_ULONG SQL_C_LONG SQL_C_NUMERIC|Les données converties sans troncation [b]<br /><br /> Les données converties avec la troncation des fractions [a]<br /><br /> La conversion de données entraînerait une perte de chiffres entiers (par opposition aux fractions de seconde) [a]<br /><br /> Données ne sont pas un *littéral numérique*[b].|data<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|Nombre d’octets du type de données C<br /><br /> Nombre d’octets du type de données C<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_FLOAT SQL_C_DOUBLE|Les données sont dans la plage du type de données à laquelle le nombre est en cours de conversion [a]<br /><br /> Données sont trouve en dehors de la plage du type de données à laquelle le nombre est en cours de conversion [a]<br /><br /> Données ne sont pas un *littéral numérique*[b].|data<br /><br /> Indéfini<br /><br /> Indéfini|Taille du type de données C<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 22003<br /><br /> 22018|  
_C_BIT|Les données sont 0 ou 1<br /><br /> Données sont supérieures à 0, inférieure à 2 et non égal à 1<br /><br /> Données sont inférieur à 0 ou supérieur ou égal à 2<br /><br /> Données ne sont pas un *littéral numérique*|data<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|1 [b]<br /><br /> 1 [b]<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données|n/a<br /><br /> 01004|  
|SQL_C_TYPE_DATE|Valeur de données est valide *valeur de date*[a]<br /><br /> Valeur de données est valide *-valeur d’horodateur*; partie heure est égale à zéro [a]<br /><br /> Valeur de données est valide *-valeur d’horodateur*; partie heure est différent de zéro [a], [c]<br /><br /> Valeur de données n’est pas valide *valeur de date* ou *-valeur d’horodateur*[a]|data<br /><br /> data<br /><br /> Données tronquées<br /><br /> Indéfini|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Indéfini|n/a<br /><br /> n/a<br /><br /> 01 S 07<br /><br /> 22018|  
|SQL_C_TYPE_TIME|Valeur de données est valide *-valeur d’heure et les valeur est 0 fractions*[a]<br /><br /> Valeur de données est valide *-valeur d’horodateur ou une valeur d’heure valide*; fractions de seconde partie des secondes est égal à zéro [a], [d]<br /><br /> Valeur de données est valide *-valeur d’horodateur*; fractions de seconde partie des secondes est différent de zéro [a], [a], [e]<br /><br /> Valeur de données n’est pas valide *-valeur d’heure* ou *-valeur d’horodateur*[a]|data<br /><br /> data<br /><br /> Données tronquées<br /><br /> Indéfini|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Indéfini|n/a<br /><br /> n/a<br /><br /> 01 S 07<br /><br /> 22018|  
_C_TYPE_TIMESTAMP|Valeur de données est valide *-valeur d’horodateur ou une valeur d’heure valide*; fractionnaire secondes ne pas tronqué [a]<br /><br /> Valeur de données est valide *-valeur d’horodateur ou une valeur d’heure valide*; fractionnaire secondes tronqué [a]<br /><br /> Valeur de données est valide *valeur de date*[a]<br /><br /> Valeur de données est valide *-valeur d’heure*[a]<br /><br /> Valeur de données n’est pas valide *valeur de date*, *-valeur d’heure*, ou *-valeur d’horodateur*[a]|data<br /><br /> Données tronquées<br /><br /> Données [f]<br /><br /> Données [g]<br /><br /> Indéfini|16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> n/a<br /><br /> n/a<br /><br /> 22018|  
|Tous les types d’intervalle C|Valeur de données est valide *la valeur d’intervalle*; aucune troncature<br /><br /> Valeur de données est valide *la valeur d’intervalle*; troncation d’un ou plusieurs champs à droite<br /><br /> Les données sont intervalle valide ; précision significatifs du champ d’en-tête est perdu<br /><br /> La valeur de données n’est pas une valeur d’intervalle valide|data<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22015<br /><br /> 22018|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote part du principe que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] Il s’agit de la taille du type de données C correspondante.  
  
 [c] la partie heure de la *-valeur d’horodateur* est tronqué.  
  
 [d] la partie date de la *-valeur d’horodateur* est ignoré.  
  
 [e] la partie fractions de secondes de l’horodatage est tronquée.  
  
 [f] les champs d’heure de la structure d’horodatage sont définies à zéro.  
  
 [g] les champs de date de la structure d’horodatage sont définies à la date actuelle.  
  
 Lorsque les données de caractères SQL sont converties en numérique, date, heure, timestamp ou données d’intervalle C, espaces à gauche et sont ignorées.
