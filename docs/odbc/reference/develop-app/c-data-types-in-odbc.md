---
title: Types de données C ODBC | Documents Microsoft
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
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
ms.assetid: c91bef31-3794-4736-966a-d50997b2233c
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 384f26d8fa33d956582caecf8c6e0911e96a23d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-data-types-in-odbc"></a>Types de données C dans ODBC
ODBC définit les types de données C qui sont utilisés par les variables d’application et de leurs identificateurs de type correspondant. Ils sont utilisés par les mémoires tampons qui sont liées aux colonnes du jeu de résultats et les paramètres de l’instruction. Par exemple, qu'une application veut récupérer des données d’une colonne de jeu de résultats au format caractère. Elle déclare une variable avec le SQLCHAR * type de données et lie cette variable à la colonne du jeu de résultats avec l’identificateur type SQL_C_CHAR. Pour obtenir une liste complète des types de données C et des identificateurs de type, consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 ODBC définit également un mappage par défaut de chaque type de données SQL à un type de données C. Par exemple, un entier de 2 octets dans la source de données est mappé à un entier de 2 octets dans l’application. Pour utiliser le mappage par défaut, une application spécifie l’identificateur de type SQL_C_DEFAULT. Toutefois, utilisation de cet identificateur est déconseillée pour des raisons d’interopérabilité.  
  
 Tous les types de données entier C définis dans ODBC 1 *.x* ont été signés. Les types de données C non signés et leurs identificateurs de type correspondants ont été ajoutés dans ODBC 2.0. Pour cette raison, les applications et les pilotes doivent être particulièrement prudent lorsque vous traitez des 1 *.x* versions.  
  
## <a name="c-data-type-extensibility"></a>Extensibilité du Type de données C  
 Dans ODBC 3.8, vous pouvez spécifier les types de données C spécifiques au pilote. Cela vous permet de lier un type SQL en tant que type dans les applications ODBC C spécifiques au pilote lorsque vous appelez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md), ou [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md). Cela peut être utile pour prendre en charge de nouveaux types de serveur, car des types de données C existants ne peuvent pas représenter correctement les nouveaux types de données de serveur. À l’aide des types spécifiques au pilote C peut augmenter le nombre de conversions qui effectuent des pilotes.  
  
 Par exemple, un système de gestion de base de données (SGBD) a introduit un nouveau type SQL, **DATETIMEOFFSET**, pour représenter la date et l’heure avec les informations de fuseau horaire. Il ne serait aucun type spécifique de C dans ODBC qui correspondait à **DATETIMEOFFSET**. Une application devrait lier **DATETIMEOFFSET** en tant que SQL_C_BINARY et cast à un de données défini par l’utilisateur de type. Compter dans ODBC 3.8 extensibilité du type de données C, un pilote peut définir un nouveau type C correspondant. Par exemple, pour le nouveau type SQL DATETIMEOFFSET, le pilote peut définir un nouveau type C correspondant comme SQL_C_DATETIMEOFFSET. Ensuite, une application peut lier le nouveau type SQL comme un type de C spécifiques au pilote.  
  
 Un type de données C est défini dans le pilote comme suit :  
  
-   Le niveau de compatibilité de ODBC pour une application, le pilote ODBC et le Gestionnaire de pilotes est 3.8 (ou version ultérieure).  
  
-   La plage de données d’un type de C spécifiques au pilote est comprise entre 0 x 4000 et 0x7FFF.  
  
-   Le pilote définit la structure des données correspondant au type C.  Pour cela, dans le Kit de développement spécifiques au pilote.  
  
 Le Gestionnaire de pilote ne validera pas un type de C défini dans la plage de 0 x 4000 et 0x7FFF ; le pilote effectue la validation et toute conversion de type de données. Mais si la plage de données d’un type C passé au Gestionnaire de pilote est comprise entre 0 x 0000 et 0x3FFF ou entre 0 x 8000 et 0xFFFF, le Gestionnaire de pilote validera le type de données C.  
  
> [!NOTE]  
>  Types de données C spécifiques au pilote doivent être décrits dans la documentation du pilote.  
  
 Pour spécifier un niveau de compatibilité ODBC de 3.8, une application appelle [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md) le SQL_ATTR_ODBC_VERSION de l’attribut la valeur **SQL_OV_ODBC3_80**. Pour déterminer la version du pilote, une application appelle **SQLGetInfo** avec SQL_DRIVER_ODBC_VER.  
  
 Pour plus d’informations sur ODBC 3.8, consultez [Nouveautés ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Type de données C](../../../odbc/reference/appendixes/c-data-types.md)
