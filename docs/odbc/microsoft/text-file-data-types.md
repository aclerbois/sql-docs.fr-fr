---
title: Types de données de fichier texte | Documents Microsoft
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
- text file driver [ODBC], data types
- ODBC desktop database drivers [ODBC], text file driver
- desktop database drivers [ODBC], text file driver
- text file data types [ODBC]
- Jet-based ODBC drivers [ODBC], text file driver
ms.assetid: e113112e-ae42-469e-8e4b-a365a10d9071
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 14b808ff5d2cdc22afa050e2c7e0b760532b880d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="text-file-data-types"></a>Types de données de fichier texte
Le tableau suivant montre comment les types de données texte sont mappées aux types de données ODBC SQL. Notez que pas tous les types de données SQL ODBC sont prises en charge par le pilote ODBC texte.  
  
|Type de données texte|Type de données ODBC|  
|--------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATETIME|SQL_TIMESTAMP|  
|FLOAT|SQL_DOUBLE|  
|INTEGER|SQL_INTEGER|  
|LONGCHAR|SQL_LONGVARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC. Toutes les conversions dans l’annexe D de la *de référence du programmeur ODBC* sont pris en charge pour les types de données SQL répertoriées dans le tableau précédent.  
  
 Le tableau suivant présente des limitations sur les types de données de texte.  
  
|Type de données| Description|  
|---------------|-----------------|  
|CHAR|Création d’une colonne de type CHAR de zéro ou de longueur non spécifiée retourne en fait une colonne de bits de 255.<br /><br /> Dans les fichiers délimités, une colonne de type CHAR peut ou ne peut pas avoir de délimiteurs guillemets au début et à la fin ; dans les fichiers de longueur fixe, les guillemets doubles ne sont pas utilisés comme séparateurs.|  
|DATETIME|MM-DD-YY (par exemple, 17-01-92)<br /><br /> MMM JJ-AA (par exemple, janvier-17-92)<br /><br /> JJ-MMM-AA (par exemple, 17-Jan-92)<br /><br /> AAAA-MM-JJ (par exemple, 1992-01-17)<br /><br /> AAAA-MMM-JJ (par exemple, janvier 1992-17)<br /><br /> Séparateurs de date mixtes ne sont pas autorisés dans une table.<br /><br /> Le texte ISAM met en forme un champ de date/heure dans le format États-Unis ou européenne, selon le paramètre International dans le panneau de configuration Windows.|  
|FLOAT|La largeur maximale inclut le signe et la virgule décimale. Dans Schema.ini, la largeur est représentée comme suit :<br /><br /> 14.083 est défini sur 6 largeur FLOAT<br /><br /> -14.083 est 7 de largeur FLOAT<br /><br /> +14.083 est 7 de largeur FLOAT<br /><br /> 14083. est FLOAT largeur 6<br /><br /> ODBC retourne toujours 8 pour les colonnes de type FLOAT.<br /><br /> Les colonnes de type FLOAT peuvent également être en notation scientifique, par exemple :<br /><br /> -3.04E + 2 est 8 de largeur Float<br /><br /> 25E4 est 4 de largeur Float<br /><br /> **Remarque** notation décimale et scientifique ne peuvent pas être mélangée dans une colonne.<br /><br /> Les valeurs NULL sont représentées par une chaîne vide remplie dans les fichiers de longueur fixe et sont omis dans les fichiers délimités.<br /><br /> Données float peuvent être complétées avec des espaces à gauche.|  
|INTEGER|Les valeurs valides pour les colonnes de type INTEGER sont 32 767 pour -32766.<br /><br /> Dans Schema.ini, la largeur est représentée comme suit :<br /><br /> 14083 est entier largeur 5<br /><br /> 0 est entier largeur 1<br /><br /> ODBC retourne toujours 4 pour les colonnes de type INTEGER.<br /><br /> La largeur maximale inclut un signe. La largeur maximale d’une colonne d’entiers est 11, bien que la largeur peut être supérieure en raison des espaces sont autorisés dans les tables de format fixe.|  
|LONGCHAR|Limite la théorique de la largeur d’une colonne LONGCHAR, que ce soit de longueur fixe ou délimité est 65500K. L’ISAM texte est plus probable prendre en charge fiable jusqu'à 32 Ko environ.|  
  
 Vous trouverez davantage de contraintes sur les types de données dans [Limitations du Type de données](../../odbc/microsoft/data-type-limitations.md).
