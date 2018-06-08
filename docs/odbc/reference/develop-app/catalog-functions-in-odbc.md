---
title: Dans ODBC, les fonctions de catalogue | Documents Microsoft
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
- catalog functions [ODBC], listed
- functions [ODBC], catalog functions
ms.assetid: 4f28f557-7eca-4905-aa6d-45a6cf501a66
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0bbff6af05123484c09cbd514f626b05d438d79e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalog-functions-in-odbc"></a>Fonctions de catalogue dans ODBC
ODBC contient les fonctions de catalogue suivantes :  
  
|Fonction| Description|  
|--------------|-----------------|  
|**SQLTables**|Retourne une liste des catalogues, des schémas, des tables ou des types de tables dans la source de données.|  
|**SQLColumns**|Retourne une liste de colonnes dans une ou plusieurs tables.|  
|**SQLStatistics**|Retourne une liste des statistiques sur une table unique. Retourne également une liste des index associés à cette table.|  
|**SQLSpecialColumns**|Retourne une liste de colonnes qui identifie de façon unique une ligne dans une table unique. Retourne également une liste de colonnes dans cette table sont automatiquement mises à jour.|  
|**SQLPrimaryKeys**|Retourne une liste de colonnes qui composent la clé primaire d’une table unique.|  
|**SQLForeignKeys**|Retourne une liste de clés étrangères dans une seule table ou une liste de clés étrangères dans d’autres tables qui font référence à une table unique.|  
|**SQLTablePrivileges**|Retourne une liste de privilèges associés à une ou plusieurs tables.|  
|**SQLColumnPrivileges**|Retourne une liste de privilèges associés à une ou plusieurs colonnes dans une table unique.|  
|**SQLProcedures**|Retourne une liste des procédures dans la source de données.|  
|**SQLProcedureColumns**|Retourne une liste de paramètres d’entrée et de sortie, la valeur de retour et les colonnes du jeu de résultats d’une procédure unique.|  
|**SQLGetTypeInfo**|Retourne une liste des types de données SQL pris en charge par la source de données. Ces types de données sont généralement utilisées dans **CREATE TABLE** et **ALTER TABLE** instructions.|  
  
 Étant donné que **SQLTables**, **SQLColumns**, **SQLStatistics**, et **SQLSpecialColumns** est conforme à la CLI de groupe ouvert, et **SQLGetTypeInfo** est conforme à la norme ISO 92 CLI, ils sont implémentés par la plupart des pilotes. Les autres fonctions de catalogue sont dans le niveau de conformité ODBC.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)  
  
-   [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)  
  
-   [Vues de schéma](../../../odbc/reference/develop-app/schema-views.md)
