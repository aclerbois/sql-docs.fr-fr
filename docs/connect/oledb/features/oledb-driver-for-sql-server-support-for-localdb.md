---
title: Pilote OLE DB SQL Server Support for LocalDB | Documents Microsoft
description: Pilote OLE DB pour la prise en charge de SQL Server pour la base de données locale
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: ''
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology:
- docset-sql-devref
ms.tgt_pltfrm: ''
ms.topic: reference
author: pmasl
ms.author: Pedro.Lopes
manager: jhubbard
ms.workload: On Demand
ms.openlocfilehash: 28238586191b72011b11271af4ae76f47098343d
ms.sourcegitcommit: 9f4330a4b067deea396b8567747a6771f35e6eee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="ole-db-driver-for-sql-server-support-for-localdb"></a>Pilote OLE DB SQL Server Support for LocalDB
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  À compter de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], une version légère de SQL Server, appelée LocalDB, sera disponible. Cette rubrique explique comment se connecter à une base de données dans une instance LocalDB.  
  
## <a name="remarks"></a>Notes  
 Pour plus d'informations sur LocalDB, notamment comment installer LocalDB et configurer votre instance LocalDB, consultez :  
  
-   [Référence SQL Server Express LocalDB](../../../relational-databases/sql-server-express-localdb-reference.md)  
  
-   [SQL Server 2016 Express LocalDB](../../../database-engine/configure-windows/sql-server-2016-express-localdb.md)  
  
 Pour résumer, LocalDB vous permet d'effectuer les opérations suivantes :  
  
-   Utiliser **sqllocaldb.exe i** pour déterminer le nom de l'instance par défaut.  
  
-   Utiliser le mot clé de chaîne de connexion **AttachDBFilename** pour spécifier le fichier de base de données auquel le serveur doit se rattacher. Lorsque vous utilisez **AttachDBFilename**, si vous ne spécifiez pas le nom de la base de données avec le mot clé de chaîne de connexion **Database** , la base de données est supprimée de l'instance LocalDB lorsque l'application se ferme.  
  
-   Spécifiez une instance LocalDB dans votre chaîne de connexion :  
  
```  
SERVER=(localdb)\v11.0  
```  
  
 Si nécessaire, vous pouvez créer une instance LocalDB avec sqllocaldb.exe. Vous pouvez également utiliser sqlcmd.exe pour ajouter et modifier des bases de données dans une instance LocalDB. Par exemple, **sqlcmd -S (localdb)\v11.0**.  
  
## <a name="see-also"></a>Voir aussi  
 [Pilote de base de données OLE pour les fonctionnalités SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
