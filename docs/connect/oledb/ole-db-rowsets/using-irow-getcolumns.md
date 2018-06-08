---
title: À l’aide d’IRow::GetColumns | Documents Microsoft
description: À l’aide d’IRow::GetColumns pour accéder à toutes les colonnes dans une ligne
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- fetching rows
- IRow interface
- single row fetching [OLE DB Driver for SQL Server]
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- GetColumns method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d4d42ce2793f2f33ec1fc61b95a52074f0c10705
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-irowgetcolumns"></a>Utilisation d'IRow::GetColumns
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le **IRow** implémentation autorise un accès séquentiel avant uniquement aux colonnes. Vous pouvez soit accéder à toutes les colonnes de la ligne avec un seul appel à **IRow::GetColumns** ou appelez **IRow::GetColumns** chaque fois que vous y accéder à plusieurs colonnes dans la ligne.  
  
 Les appels multiples à **IRow::GetColumns** ne doivent pas se chevaucher. Par exemple, si le premier appel à **IRow::GetColumns** récupère les colonnes 1, 2 et 3, le deuxième appel à **IRow::GetColumns** doit appeler les colonnes 4, 5 et 6. Si appels ultérieurs à **IRow::GetColumns** se chevauchent, l’indicateur d’état (champ dwstatus dans DBCOLUMNACCESS) a la valeur DBSTATUS_E_UNAVAILABLE.  
  
## <a name="see-also"></a>Voir aussi  
 [Extraction d’une ligne unique avec IRow](../../oledb/ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
  
