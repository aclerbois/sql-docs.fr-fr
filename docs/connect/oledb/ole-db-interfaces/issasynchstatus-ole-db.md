---
title: ISSAsynchStatus (OLE DB) | Documents Microsoft
description: ISSAsynchStatus (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- ISSAsynchStatus (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSAsynchStatus interface
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: ca37010f2c0c0e42dd7e75f2dc87b0654fb31474
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="issasynchstatus-ole-db"></a>ISSAsynchStatus (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  **ISSAsynchStatus** interface expose la prise en charge de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des opérations asynchrones. Il s’agit d’une interface facultative qui hérite de l’interface OLE DB **IDBAsynchStatus**. Outre les méthodes **Abort** et **GetStatus** héritées de **IDBAsynchStatus**, **ISSAsynchStatus** fournit une nouvelle méthode qui permet d'attendre qu'une opération asynchrone se termine ou qu'un délai d'expiration soit dépassé.  
  
|Méthode| Description|  
|------------|-----------------|  
|[ISSAsynchStatus::Abort &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-abort-ole-db.md)|Annule une opération s'exécutant de manière asynchrone.|  
|[ISSAsynchStatus::GetStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md)|Retourne l'état d'une opération s'exécutant de manière asynchrone.|  
|[ISSAsynchStatus::WaitForAsynchCompletion &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md)|Attend que l'opération s'exécutant de façon asynchrone se termine ou qu'un délai d'expiration soit dépassé.|  
  
## <a name="remarks"></a>Notes  
 L'implémentation **ISSAsynchStatus** de la méthode **ISSAsynchStatus::GetStatus** est identique à la méthode **IDBAsynchStatus::GetStatus** , à la différence près que si l'initialisation d'un objet source de données est abandonnée, E_UNEXPECTED est retourné au lieu de DB_E_CANCELED (bien que **ISSAsynchStatus::WaitForAsynchCompletion** retourne DB_E_CANCELED). Cela signifie que l’objet de source de données n’est pas conservé dans l’état habituel après une opération d’abandon, de sorte que les autres tentatives d’initialisation puissent avoir lieu.  
  
 Les méthodes suivantes prennent en charge l'utilisation d'une exécution asynchrone dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   **ICommand::Execute**  
  
-   **IOpenRowset::OpenRowset**  
  
-   **IMultipleResults::GetResult**  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)  
  
  
