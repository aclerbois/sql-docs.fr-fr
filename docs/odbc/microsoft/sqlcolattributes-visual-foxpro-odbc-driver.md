---
title: SQLColAttributes (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- SQLColAttribute function [ODBC], Visual FoxPro ODBC Driver
ms.assetid: d403dfa0-c26d-47d4-91d9-2f29aa387399
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee4dc59599053bbbf676f94e8e94540aad83ec82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolattributes-visual-foxpro-odbc-driver"></a>SQLColAttributes (le pilote ODBC Visual FoxPro)
> [!NOTE]  
>  Cette rubrique contient des informations spécifiques au pilote ODBC Visual FoxPro. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Prise en charge : complet  
  
 Conformité d’API ODBC : Niveau principal  
  
 Retourne les informations de descripteur pour une colonne dans un jeu de résultats. Informations de descripteur sont retournées comme une chaîne de caractères, une valeur de descripteur dépendant 32 bits ou une valeur entière.  
  
> [!NOTE]  
>  **SQLColAttributes** ne peut pas être utilisée pour retourner des informations sur la signet colonne (0).  
  
 Le pilote ODBC Visual FoxPro prend en charge tous les *fDescType* valeurs. Le tableau suivant inclut des commentaires sur l’implémentation du pilote de valeurs sélectionnées.  
  
|*fDescType*|Commentaire|  
|-----------------|-------------|  
|SQL_COLUMN_AUTO_INCREMENT|Retourne FALSE : Visual FoxPro ne comporte aucun champ de compteur.|  
|SQL_COLUMN_CASE_SENSITIVE|Retourne toujours la valeur TRUE si le type de colonne est le caractère.|  
|SQL_COLUMN_LABEL|Retourne le nom de colonne, qui est également renvoyé par SQL_COLUMN_NAME.|  
|SQL_COLUMN_MONEY|Retourne la valeur TRUE si le type de colonne est monétaire (représenté par un « Y » dans le langage Visual FoxPro).|  
|SQL_COLUMN_OWNER_NAME|Retourne toujours une chaîne vide.|  
|SQL_COLUMN_QUALIFIER_NAME|Retourne toujours une chaîne vide.|  
|SQL_COLUMN_SEARCHABLE|Renvoie SQL_UNSEARCHABLE pour les colonnes de type général ; ces colonnes ne peut pas être utilisés dans une clause WHERE.<br /><br /> Renvoie SQL_SEARCHABLE pour les colonnes de type caractère ou Mémo avec NOCPTRANS pas défini ; ces colonnes peuvent être utilisées dans une clause WHERE avec tout opérateur de comparaison.<br /><br /> Retourne SQL_ALL_EXCEPT_LIKE pour tous les autres types de colonne ; ces colonnes peuvent être utilisées dans une clause WHERE avec tous les opérateurs de comparaison à l’exception de type.|  
|SQL_COLUMN_TABLE_NAME|Retourne toujours une chaîne vide.|  
  
 Pour plus d’informations, consultez [SQLColAttributes](../../odbc/reference/syntax/sqlcolattributes-function.md) dans les *de référence du programmeur ODBC*.
