---
title: Affectation de stockage | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-results
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- column-wise binding [ODBC]
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- SQLBindCol function
- storing data [ODBC]
- result sets [ODBC], storage
- SQL Server Native Client ODBC driver, data storage
- row-wise binding
- binding result sets [SQL Server Native Client]
- array binding
ms.assetid: 11c81955-5300-495f-925f-9256f2587b58
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1651df87ad3bcf0ac9e5d0f8c865e2d92f925dd8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assigning-storage"></a>Assignation du stockage
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Une application peut assigner le stockage pour les résultats avant ou après avoir exécuté une instruction SQL. Si une application prépare ou exécute en premier l'instruction SQL, elle peut se renseigner à propos du jeu de résultats avant d'assigner le stockage pour les résultats. Par exemple, si le jeu de résultats est inconnu, l'application doit extraire le nombre de colonnes avant de pouvoir lui assigner du stockage.  
  
 Pour associer du stockage pour une colonne de données, une application appelle [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)et lui passe :  
  
-   Le type de données vers lequel les données doivent être converties.  
  
-   L'adresse d'un tampon de sortie pour les données.  
  
     L'application doit allouer cette mémoire tampon, qui doit être suffisamment grande pour contenir les données au format dans lequel elles sont converties.  
  
-   La longueur du tampon de sortie.  
  
     Cette valeur est ignorée si les données retournées ont une largeur fixe dans C, tel qu'un entier, un nombre réel ou une structure de date.  
  
-   L'adresse d'un tampon mémoire dans lequel retourner le nombre d'octets de données disponibles.  
  
 Une application peut également lier des colonnes de jeu de résultats à des tableaux de variables de programme afin de prendre en charge l'extraction de lignes de jeu de résultats en blocs. Il existe deux types différents de liaison de tableau :  
  
-   La liaison basée sur les colonnes est finie lorsque chaque colonne est liée à son propre tableau de variables.  
  
     La liaison est spécifiée en appelant [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) avec *attribut* défini à SQL_ATTR_ROW_BIND_TYPE et *ValuePtr* défini à SQL_BIND_BY_COLUMN. Tous les tableaux doivent contenir le même nombre d'éléments.  
  
-   La liaison basée sur les lignes est finie lorsque tous les paramètres dans l'instruction SQL sont liés en tant qu'unité à un tableau de structures qui contiennent les variables individuelles pour les paramètres.  
  
     La liaison est spécifiée en appelant **SQLSetStmtAttr** avec *attribut* défini à SQL_ATTR_ROW_BIND_TYPE et *ValuePtr* défini sur la taille de la structure qui contient les variables qui recevront le résultat de définir les colonnes.  
  
 L'application définit également SQL_ATTR_ROW_ARRAY_SIZE au nombre d'éléments dans les tableaux de colonnes ou de lignes et définit SQL_ATTR_ROW_STATUS_PTR et SQL_ATTR_ROWS_FETCHED_PTR.  
  
## <a name="see-also"></a>Voir aussi  
 [Le traitement des résultats &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
