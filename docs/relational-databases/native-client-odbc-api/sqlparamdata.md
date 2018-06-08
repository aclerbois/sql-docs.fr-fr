---
title: SQLParamData | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLParamData function
ms.assetid: 92349482-ea22-4a6a-8484-e9c6566794fa
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9ae818bb7c3e81be3a1a8a4bd2532ece7106e539
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlparamdata"></a>SQLParamData
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Lors du retour SQLParamData le *ValuePtrPtr* associé à un paramètre table, l’application doit appeler SQLPutData avec *StrLen_Or_Ind*. Si *StrLen_Or_Ind* a une valeur supérieure à 0, cela signifie que l’application est prête pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour collecter des données de paramètre pour la ligne de paramètre table suivante. Si *StrLen_Or_Ind* a une valeur égale à 0, cela signifie qu'il n'y a plus de lignes de données pour le paramètre table. Pour plus d’informations, consultez [liaison et les valeurs des colonnes et les paramètres Data Transfer of Table-Valued](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLParamData](http://go.microsoft.com/fwlink/?LinkId=80706)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
