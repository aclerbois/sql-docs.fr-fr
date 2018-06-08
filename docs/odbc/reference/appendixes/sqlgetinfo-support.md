---
title: Prise en charge SQLGetInfo | Documents Microsoft
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
- compatibility [ODBC], SQLGetInfo
- backward compatibility [ODBC], SQLGetInfo
- SQLGetInfo function [ODBC], support
ms.assetid: 57326f57-daba-46b6-b0be-6c97213b9ef1
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f3f51deb4cb589ed5981f6361eeef2abe6a727cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetinfo-support"></a>Prise en charge SQLGetInfo
Lorsqu’une application ODBC 2. *x* application appelle **SQLGetInfo** à un ODBC 3 *.x* pilote, le *InfoType* arguments dans le tableau suivant doivent être pris en charge.  
  
|*InfoType*|Valeur renvoyée|  
|----------------|-------------|  
|SQL_ALTER_TABLE (ODBC 2.0) **Remarque :** ce type d’information n’est pas déconseillé ; les masques de bits dans la colonne à droite sont déconseillés.|Un masque de bits SQLINTEGER énumérant les clauses de la **ALTER TABLE** instruction pris en charge par la source de données.<br /><br /> Les masques de bits suivants sont utilisés pour déterminer les clauses sont pris en charge :<br /><br /> SQL_AT_DROP_COLUMN = la possibilité de supprimer des colonnes est pris en charge. Si cela entraîne en cascade ou limiter le comportement est définie par le pilote. (ODBC VERSION 2.0)<br /><br /> SQL_AT_ADD_COLUMN = la possibilité d’ajouter la prise en charge de plusieurs colonnes dans une instruction ALTER TABLE. Ce bit n’associe pas avec les autres SQL_AT_ADD_COLUMN_XXX ou SQL_AT_CONSTRAINT_XXX bits. (ODBC VERSION 2.0)|  
|SQL_FETCH_DIRECTION (ODBC VERSION 1.0)<br /><br /> Le type d’informations a été introduit dans ODBC 1.0 ; chaque masque de bits est étiqueté avec la version dans laquelle elle a été introduite.|Un masque de bits SQLINTEGER énumérant les options de direction d’extraction prises en charge.<br /><br /> Les masques de bits suivants sont utilisés conjointement avec l’indicateur pour déterminer quelles options sont prises en charge :<br /><br /> SQL_FD_FETCH_NEXT (ODBC 1.0) SQL_FD_FETCH_BOOKMARK (ODBC 1.0) SQL_FD_FETCH_ABSOLUTE (ODBC 1.0) SQL_FD_FETCH_PRIOR (ODBC 1.0) SQL_FD_FETCH_LAST (ODBC 1.0) SQL_FD_FETCH_RELATIVE (ODBC 1.0) SQL_FD_FETCH_BOOKMARK (ODBC VERSION 2.0)|  
|SQL_LOCK_TYPES (ODBC VERSION 2.0)|Un masque de bits SQLINTEGER le verrou pris en charge de l’énumération des types pour le *généalogique* argument dans **SQLSetPos**.<br /><br /> Les masques de bits suivants sont utilisés conjointement avec l’indicateur pour déterminer quels types de verrouillage sont prises en charge :<br /><br /> SQL_LCK_NO_CHANGE SQL_LCK_EXCLUSIVE SQL_LCK_UNLOCK|  
|SQL_ODBC_API_CONFORMANCE (ODBC VERSION 1.0)|Une valeur SQLSMALLINT indiquant le niveau de conformité de ODBC.<br /><br /> SQL_OAC_NONE = None<br /><br /> SQL_OAC_LEVEL1 = 1 au niveau de prise en charge<br /><br /> SQL_OAC_LEVEL2 = niveau 2 est pris en charge|  
|SQL_ODBC_SQL_CONFORMANCE (ODBC VERSION 1.0)|Une valeur SQLSMALLINT indiquant la grammaire SQL pris en charge par le pilote. Consultez [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md) pour une définition de niveaux de conformité SQL.<br /><br /> SQL_OSC_MINIMUM = grammaire minimale prise en charge<br /><br /> SQL_OSC_CORE = grammaire de noyaux pris en charge<br /><br /> SQL_OSC_EXTENDED = grammaire étendu pris en charge|  
|SQL_POS_OPERATIONS (ODBC VERSION 2.0)|Un masque de bits SQLINTEGER énumérant les opérations prises en charge dans **SQLSetPos**.<br /><br /> Les masques de bits suivants sont utilisés conjointement avec l’indicateur pour déterminer quelles options sont prises en charge :<br /><br /> SQL_POS_POSITION (ODBC VERSION 2.0) SQL_POS_REFRESH (ODBC VERSION 2.0) SQL_POS_UPDATE (ODBC VERSION 2.0) SQL_POS_DELETE (ODBC VERSION 2.0) SQL_POS_ADD (ODBC VERSION 2.0)|  
|SQL_POSITIONED_STATEMENTS (ODBC VERSION 2.0)|Un masque de bits SQLINTEGER l’énumération prises en charge positionné des instructions SQL.<br /><br /> Les masques de bits suivants sont utilisés pour déterminer quelles instructions sont pris en charge :<br /><br /> SQL_PS_POSITIONED_DELETE SQL_PS_POSITIONED_UPDATE SQL_PS_SELECT_FOR_UPDATE|  
|SQL_SCROLL_CONCURRENCY (ODBC VERSION 1.0)|Un masque de bits SQLINTEGER énumérant les options de contrôle d’accès concurrentiel pris en charge pour le curseur.<br /><br /> Les masques de bits suivants sont utilisés pour déterminer quelles options sont prises en charge :<br /><br /> SQL_SCCO_READ_ONLY = curseur est en lecture seule. Aucune mise à jour n’est autorisées.<br /><br /> SQL_SCCO_LOCK = curseur utilise le niveau de verrouillage pour vous assurer que la ligne peut être mis à jour le plus bas.<br /><br /> SQL_SCCO_OPT_ROWVER = curseur le contrôle d’accès concurrentiel optimiste utilise en comparant des versions de ligne, telles que SQLBase ROWID ou Sybase TIMESTAMP.<br /><br /> SQL_SCCO_OPT_VALUES = curseur le contrôle d’accès concurrentiel optimiste utilise en comparant les valeurs.|  
|SQL_STATIC_SENSITIVITY (ODBC VERSION 2.0)|Un masque de bits SQLINTEGER énumérant si les modifications apportées par une application pour un curseur statique ou jeu de clés via **SQLSetPos** ou mise à jour positionnée ou les instructions delete peuvent être détectées par l’application.<br /><br /> SQL_SS_ADDITIONS = Added lignes sont visibles par le curseur ; le curseur peut défiler pour ces lignes. Où ces lignes sont ajoutées au curseur dépend du pilote.<br /><br /> SQL_SS_DELETIONS = Deleted lignes ne sont plus disponibles jusqu’au curseur et ne laissent pas un « trou » dans le jeu de résultats ; une fois que le curseur se place à partir d’une ligne supprimée, elle ne peut pas retourner à cette ligne.<br /><br /> SQL_SS_UPDATES = mises à jour de lignes sont visibles par le curseur ; Si le curseur défile à partir d’et renvoie à une ligne mise à jour, les données retournées par le curseur sont les données mises à jour, pas les données d’origine. Cette option s’applique les mises à jour ou les curseurs statiques uniquement sur les curseurs pilotés par jeu de clés qui ne mettent pas à jour la clé. Cette option ne s’applique pas pour un curseur dynamique ou dans le cas dans lequel une clé est modifiée dans un curseur mixte.<br /><br /> Si une application peut détecter les modifications apportées à l’ensemble par d’autres utilisateurs, y compris d’autres curseurs dans la même application, de résultats dépend du type de curseur.|  
  
 Un ODBC 3 *.x* application utilisant une ODBC 3 *.x* pilote ne doit pas appeler **SQLGetInfo** avec la *InfoType* arguments décrits dans le tableau précédent, mais doit utiliser la version 3 ODBC *.x* *InfoType* les arguments répertoriés dans le paragraphe suivant. Il n’est pas une correspondance univoque entre *InfoType* arguments utilisés dans ODBC 2. *x* et ceux utilisés dans ODBC 3 *.x*. Un ODBC 3 *.x* application utilisant une API ODBC 2. *x* pilote, quant à eux, doit utiliser le *InfoType* arguments décrits précédemment.  
  
 Certains types d’informations dans le tableau précédent sont déconseillées au profit de types d’informations d’attributs curseur. Ces déconseillée informations types SQL_FETCH_DIRECTION, SQL_LOCK_TYPES, SQL_POS_OPERATIONS, SQL_POSITIONED_STATEMENTS, SQL_SCROLL_CONCURRENCY et SQL_STATIC_SENSITIVITY. Les nouveaux types d’attributs de curseur sont SQL_XXX_CURSOR_ATTRIBUTES2 SQL_XXX_CURSOR_ATTRIBUTES1and, où XXX est égal à DYNAMIC, FORWARD_ONLY, KEYSET_DRIVEN ou STATIC. Chacun des nouveaux types d’indique les fonctionnalités du pilote pour un type de curseur unique. Pour plus d’informations sur ces options, consultez la [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.
