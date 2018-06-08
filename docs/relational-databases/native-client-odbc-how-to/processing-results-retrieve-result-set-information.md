---
title: Récupérer les informations du jeu de résultats (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC]
- result sets [ODBC], fetching
ms.assetid: 34f235e4-f80b-4123-8764-9deb18506f14
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a75d60fe1175026b8c458d867ad6b528172456e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="processing-results---retrieve-result-set-information"></a>Le traitement des résultats - récupérer les informations du jeu de résultats
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

    
### <a name="to-get-information-about-a-result-set"></a>Pour obtenir des informations sur un jeu de résultats  
  
1.  Appelez [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md) pour obtenir le nombre de colonnes dans le jeu de résultats.  
  
2.  Pour chaque colonne de l'ensemble de résultats :  
  
    -   Appelez [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) pour obtenir plus d’informations sur la colonne de résultats.  
  
     ou  
  
    -   Appelez [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md) pour obtenir des informations spécifiques sur la colonne de résultats.  
  
## <a name="see-also"></a>Voir aussi  
[Traiter les résultats & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-how-to/processing-results-process-results.md)

[Déterminer les caractéristiques d’un jeu de résultats & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-results/determining-the-characteristics-of-a-result-set-odbc.md)  
  
  
