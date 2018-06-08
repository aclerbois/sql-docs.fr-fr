---
title: Types de données Microsoft Access | Documents Microsoft
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
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7c12bee02bd747b5f44ce5c9651b26a3cdcc3080
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-access-data-types"></a>Types de données Microsoft Access
Le tableau suivant montre les types de données Microsoft Access, les types de données utilisés pour créer des tables et les types de données ODBC SQL.  
  
|Type de données Microsoft Access|Type de données (Create Table)|Type de données SQL ODBC|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|LONGBINARY|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|COMPTEUR|COMPTEUR|SQL_INTEGER|  
|Monétaire (Currency)|Monétaire (Currency)|SQL_NUMERIC|  
|DATE/HEURE|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|BINAIRE DE TYPE LONG|LONGBINARY|SQL_LONGVARBINARY|  
|TEXTE LONG|LONGTEXT|SQL_WLONGVARCHAR DE SQL_LONGVARCHAR [2] [3]|  
|MÉMO|LONGTEXT|SQL_WLONGVARCHAR DE SQL_LONGVARCHAR [2] [3]|  
|NOMBRE (taille du champ = unique)|UNIQUE|SQL_REAL|  
|NOMBRE (taille du champ = DOUBLE)|DOUBLE|SQL_DOUBLE|  
|NOMBRE (taille du champ = octet)|OCTET NON SIGNÉ|SQL_TINYINT|  
|NOMBRE (taille du champ = entier)|COURTE|SQL_SMALLINT|  
|NOMBRE (taille du champ = entier LONG)|LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE|LONGBINARY|SQL_LONGVARBINARY|  
|TEXT|VARCHAR|[1] DE SQL_VARCHAR, SQL_WVARCHAR [2]|  
ARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] les applications d’accès 4.0 uniquement. Longueur maximale de 4 000 octets. Comportement similaire à LONGBINARY.  
  
 [2] applications ANSI uniquement.  
  
 [3] applications Unicode et d’accès 4.0 uniquement.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** retourne des types de données ODBC. Il ne retourne pas tous les types de données Microsoft Access si plusieurs types de Microsoft Access est mappé au même type de données ODBC SQL. Toutes les conversions dans l’annexe D de la *de référence du programmeur ODBC* sont pris en charge pour les types de données SQL répertoriées dans le tableau précédent.  
  
 Le tableau suivant présente des limitations sur les types de données Microsoft Access.  
  
|Type de données| Description|  
|---------------|-----------------|  
|BINARY, VARBINARY et VARCHAR|Création d’une colonne BINARY, VARBINARY ou VARCHAR de zéro ou de longueur non spécifiée retourne en fait une colonne 510 octets.|  
|BYTE|Même si un champ de numéro d’accès Microsoft avec une taille égale à octets n’est pas signé, un nombre négatif peut être inséré dans le champ lorsque vous utilisez le pilote Microsoft Access.|  
|CHAR et VARCHAR LONGVARCHAR|Un littéral de chaîne de caractères peut contenir n’importe quel caractère ANSI (décimal 1-255). Utilisez deux consécutifs apostrophes accolées ('') pour représenter un guillemet simple (').<br /><br /> Procédures doivent être utilisées pour passer des données de caractères lors de l’utilisation des caractères spéciaux dans une colonne de type caractère.|  
|DATE|Les valeurs de date doivent être délimités en fonction du format de date canonique ODBC ou délimitées par le séparateur de date/heure (« # »). Dans le cas contraire, Microsoft Access traite la valeur comme une expression arithmétique et ne génère pas d’avertissement ou erreur.<br /><br /> Par exemple, la date « 5 mars 1996 » doit être représenté en tant que {d ' 1996-03-05'} ou #03/05/1996 #; Sinon, si seul le 03/05/1993 est soumis, Microsoft Access évaluera cette 3 divisé par 5 divisé par 1996. Cette valeur arrondit à l’entier 0, et étant donné que le zéro jour mappe à 1899-12-31, la date est utilisée.<br /><br /> Une barre verticale (&#124;) ne peut pas servir dans une valeur de date, même si placée à l’arrière de guillemets.|  
|GUID|Type de données limité à 4.0 de Microsoft Access.|  
|NUMERIC|Type de données limité à 4.0 de Microsoft Access.|  
  
 Vous trouverez davantage de contraintes sur les types de données dans [Limitations du Type de données](../../odbc/microsoft/data-type-limitations.md).
