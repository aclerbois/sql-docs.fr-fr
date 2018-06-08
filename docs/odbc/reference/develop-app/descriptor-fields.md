---
title: Champs de descripteur | Documents Microsoft
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
- descriptors [ODBC], fields
- header fields [ODBC]
- record fields [ODBC]
ms.assetid: f38623c8-fdd4-4601-b1f0-97c593d31177
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cddf01c0ef40b582410773ba109c2d1c23ee1e7a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="descriptor-fields"></a>Champs de descripteur
Les descripteurs contiennent *en-tête* et *enregistrement* champs qui décrivent complètement les colonnes ou des paramètres.  
  
 Un descripteur contient une seule copie des champs d’en-tête suivant. Modification d’un champ d’en-tête affecte toutes les colonnes ou des paramètres.  
  
|||  
|-|-|  
|SQL_DESC_ALLOC_TYPE|SQL_DESC_BIND_TYPE|  
|SQL_DESC_ARRAY_SIZE|SQL_DESC_COUNT|  
|SQL_DESC_ARRAY_STATUS_PTR|SQL_DESC_ROWS_PROCESSED_PTR|  
|SQL_DESC_BIND_OFFSET_PTR||  
  
 Un descripteur contient zéro ou plusieurs enregistrements de descripteurs. Chaque enregistrement décrit une colonne ou un paramètre, selon le type du descripteur. Lorsqu’une nouvelle colonne ou un paramètre est liée, un nouvel enregistrement est ajouté au descripteur. Une colonne ou un paramètre est indépendant, un enregistrement est supprimé à partir du descripteur. Chaque enregistrement contient une seule copie des champs suivants :  
  
|||  
|-|-|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_DESC_LOCAL_TYPE_NAME|  
|SQL_DESC_BASE_COLUMN_NAME|SQL_DESC_NAME|  
|SQL_DESC_BASE_TABLE_NAME|SQL_DESC_NULLABLE|  
|SQL_DESC_CASE_SENSITIVE|SQL_DESC_OCTET_LENGTH|  
|SQL_DESC_CATALOG_NAME|SQL_DESC_OCTET_LENGTH_PTR|  
|SQL_DESC_CONCISE_TYPE|SQL_DESC_PARAMETER_TYPE|  
|SQL_DESC_DATA_PTR|SQL_DESC_PRECISION|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQL_DESC_SCALE|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|SQL_DESC_SCHEMA_NAME|  
|SQL_DESC_DISPLAY_SIZE|SQL_DESC_SEARCHABLE|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_DESC_TABLE_NAME|  
|SQL_DESC_INDICATOR_PTR|SQL_DESC_TYPE|  
|SQL_DESC_LABEL|SQL_DESC_TYPE_NAME|  
|SQL_DESC_LENGTH|SQL_DESC_UNNAMED|  
|SQL_DESC_LITERAL_PREFIX|SQL_DESC_UNSIGNED|  
|SQL_DESC_LITERAL_SUFFIX|SQL_DESC_UPDATABLE|  
  
 Plusieurs attributs d’instruction correspondant au champ d’en-tête d’un descripteur. Si ces attributs via un appel à **SQLSetStmtAttr** et en définissant le champ d’en-tête de descripteur correspondant en appelant **SQLSetDescField** ont le même effet. Est de même pour **SQLGetStmtAttr** et **SQLGetDescField**, de récupérer les mêmes informations. Appeler les fonctions de l’instruction à la place les fonctions de descripteur a l’avantage qu’un handle de descripteur n’est pas à récupérer.  
  
 Les champs d’en-tête suivantes peuvent être définies en définissant les attributs d’instruction :  
  
|||  
|-|-|  
|SQL_DESC_ARRAY_SIZE|SQL_DESC_BIND_TYPE|  
|SQL_DESC_ARRAY_STATUS_PTR|SQL_DESC_ROWS_PROCESSED_PTR|  
|SQL_DESC_BIND_OFFSET_PTR||  
  
 Cette section contient les rubriques suivantes.  
  
-   [Nombre d’enregistrements](../../../odbc/reference/develop-app/record-count.md)  
  
-   [Enregistrements de descripteurs liés](../../../odbc/reference/develop-app/bound-descriptor-records.md)  
  
-   [Champs différés](../../../odbc/reference/develop-app/deferred-fields.md)  
  
-   [Vérification de la cohérence](../../../odbc/reference/develop-app/consistency-check.md)
