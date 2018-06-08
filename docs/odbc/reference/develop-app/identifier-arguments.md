---
title: Arguments de l’identificateur | Documents Microsoft
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
- identifier arguments [ODBC]
- catalog functions [ODBC], arguments
- arguments in catalog functions [ODBC], identifier
ms.assetid: b9de003f-cb49-4dec-b528-14a5b8ff12bd
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1d483c50928d93ecd64419726eb77ae162f221f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identifier-arguments"></a>Arguments de l’identificateur
Si une chaîne dans un argument de l’identificateur est placé entre guillemets, le pilote supprime de début et les espaces à droite et traite littéralement la chaîne entre guillemets. Si la chaîne n’est pas placé entre guillemets, le pilote supprime les espaces et des plis la chaîne en majuscules. La définition d’un argument d’identificateur à un pointeur null retourne SQL_ERROR et SQLSTATE HY009 (utilisation non valide d’un pointeur null), sauf si l’argument est un nom de catalogue et de catalogues ne sont pas pris en charge.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, ces arguments sont traités en tant qu’arguments de l’identificateur. Dans ce cas, le trait de soulignement (_) et le signe de pourcentage (%) sera traité comme le caractère réel, et non comme un caractère de modèle de recherche. Ces arguments sont traités comme un argument ordinaire ou d’un argument de modèle, en fonction de l’argument, si cet attribut a la valeur SQL_FALSE.  
  
 Bien que les identificateurs qui contiennent des caractères spéciaux doivent être mis entre guillemets dans les instructions SQL, ils ne doivent pas être entre guillemets lorsqu’il est passé en tant qu’arguments de fonction de catalogue, étant donné que les caractères de guillemet transmis aux fonctions de catalogue sont interprétées littéralement. Par exemple, supposons que l’identificateur de guillemet (qui est spécifique au pilote et retournés via **SQLGetInfo**) est un guillemet double («). Le premier appel à **SQLTables** renvoie un jeu de résultats contenant plus d’informations sur la table de fournisseurs, tandis que le deuxième appel retourne des informations sur la table « Comptabilité », qui est probablement pas ce qui a été conçu.  
  
```  
SQLTables(hstmt1, NULL, 0, NULL, 0, "Accounts Payable", SQL_NTS, NULL, 0);  
SQLTables(hstmt2, NULL, 0, NULL, 0, "\"Accounts Payable\"", SQL_NTS, NULL, 0);  
```  
  
 Identificateurs entre guillemets sont utilisés pour distinguer un nom de colonne true une pseudo-colonne du même nom, tels que le ROWID dans Oracle. Si « ROWID » est passée dans un argument d’une fonction de catalogue, la fonction fonctionnera avec la pseudo-colonne ROWID si elle existe. Si la colonne pseudo-aléatoire n’existe pas, la fonction fonctionnera avec la colonne « ROWID ». Si le ROWID est passée dans un argument d’une fonction de catalogue, la fonction fonctionnera avec la colonne de ligne ROWID.  
  
 Pour plus d’informations sur les identificateurs entre guillemets, consultez [identificateurs entre guillemets](../../../odbc/reference/develop-app/quoted-identifiers.md).
