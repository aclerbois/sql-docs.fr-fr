---
title: Résultats des messages SQL Server | Documents Microsoft
description: résultats des messages SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-errors
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], SQL Server message results
- OLE DB error handling, SQL Server message results
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d9ab76565bac6a1fbf41dd5f8372776ea8625975
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-message-results"></a>Résultats des messages SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Les éléments suivants [!INCLUDE[tsql](../../../includes/tsql-md.md)] instructions ne génèrent pas de pilote OLE DB pour SQL Server les ensembles de lignes ou un nombre de lignes affectées lors de l’exécution :  
  
-   PRINT  
  
-   RAISERROR avec une gravité égale ou inférieure à 10  
  
-   DBCC  
  
-   SET SHOWPLAN  
  
-   SET STATISTICS  
  
 Ces instructions retournent un ou plusieurs messages d'information ou entraînent le renvoi par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de messages d'information au lieu de résultats sous forme d'ensemble de lignes ou de nombre. Succès de l’exécution, le pilote OLE DB pour SQL Server retourne S_OK et les messages sont disponibles pour le pilote OLE DB pour le consommateur de SQL Server.  
  
 Le pilote OLE DB pour SQL Server retourne S_OK et possède un ou plusieurs messages d’information disponibles après l’exécution de plusieurs [!INCLUDE[tsql](../../../includes/tsql-md.md)] instructions ou l’exécution par le consommateur d’un pilote OLE DB pour la fonction membre de SQL Server.  
  
 Le pilote OLE DB pour le consommateur SQL Server permettant la spécification dynamique du texte de requête doit vérifier les interfaces d’erreur après chaque exécution de la fonction membre, quelle que soit la valeur du code de retour, la présence ou l’absence d’une **IRowset** ou **IMultipleResults** référence de l’interface ou un nombre de lignes affectées.  
  
## <a name="see-also"></a>Voir aussi  
 [Erreurs](../../oledb/ole-db-errors/errors.md)  
  
  
