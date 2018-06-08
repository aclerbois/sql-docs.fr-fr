---
title: SQLStatistics | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLStatistics function
ms.assetid: e60101ae-a5f5-432f-a32a-d8e6fb0cbde8
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ec1adc3518bdd4f43d04a8f5f53515dc2e44d386
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlstatistics"></a>SQLStatistics
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  **SQLStatistics** peut être exécuté sur un curseur statique. Toute tentative d'exécution de **SQLStatistics** sur un curseur pouvant être mis à jour (curseur de jeu de clés ou curseur dynamique) retourne SQL_SUCCESS_WITH_INFO pour indiquer que le type de curseur a été modifié.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction SQLStatistics](http://go.microsoft.com/fwlink/?LinkId=59372)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
