---
title: SQL Server Native Client | Documents Microsoft
ms.date: 04/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e4d4fe39-0090-42a7-8405-6378370d11cb
caps.latest.revision: 43
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0d4694e013ce65fb1fb3c66b352b8e583a608899
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-native-client"></a>SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

SNAC, ou SQL Server Native Client, est un terme qui a été utilisé de façon interchangeable pour faire référence aux pilotes ODBC et OLE DB pour SQL Server.

**Remarque :** il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. Le nouveau fournisseur OLE DB est appelé le [pilote Microsoft OLE DB pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) qui sera mis à jour avec les fonctionnalités du serveur les plus récentes à l’avenir.


**Pour plus d’informations et pour télécharger les pilotes ODBC SNAC, visitez [SNAC le cycle de vie expliqué](https://blogs.msdn.microsoft.com/sqlreleaseservices/snac-lifecycle-explained/).**

Pour plus d’informations sur le pilote ODBC pour SQL Server, consultez [Microsoft ODBC Driver for SQL Server sur Windows](https://msdn.microsoft.com/library/jj730314(v=sql.110).aspx).  Voir aussi [présentation les nouveaux pilotes ODBC de Microsoft SQL Server](https://blogs.msdn.microsoft.com/sqlnativeclient/2013/01/23/introducing-the-new-microsoft-odbc-drivers-for-sql-server/), et [ODBC Driver 13.1 for SQL Server est publié](https://blogs.technet.microsoft.com/dataplatforminsider/2016/08/03/odbc-driver-13-1-for-sql-server-released/).  

 Pour plus d’informations sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des fonctions de Native Client fournie avec [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la dernière version disponible de SQL Server native Client :

-   [Prise en charge de SQL Server Native Client pour la base de données locale](../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  

-   [Détection des métadonnées](../../relational-databases/native-client/features/metadata-discovery.md)  

-   [Prise en charge de UTF-16 dans SQL Server Native Client 11.0](../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  

-   [Prise en charge SQL Server Native Client pour la haute disponibilité, la récupération d’urgence](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  

-   [Accès aux informations de diagnostic dans le journal des événements étendus](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  

ODBC dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prend en charge trois fonctionnalités qui ont été ajoutées à ODBC standard dans le Kit de développement logiciel Windows 7 :  

-   Exécution asynchrone sur les opérations relatives à une connexion. Pour plus d’informations, consultez [exécution asynchrone](http://go.microsoft.com/fwlink/?LinkID=191493).  

-   C. Extensibilité du type de données Pour plus d’informations, consultez [des Types de données C dans ODBC](http://go.microsoft.com/fwlink/?LinkID=191495).  

     Pour prendre en charge cette fonctionnalité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, SQLGetDescField peut retourner **SQL_C_SS_TIME2** (pour **temps** types) ou **SQL_C_SS_TIMESTAMPOFFSET** (pour **datetimeoffset**) au lieu de **SQL_C_BINARY**, si votre application utilise ODBC 3.8. Pour plus d’informations, consultez [prise en charge du Type de données de Date ODBC et les améliorations apportées au](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md).  

-   Appel de **SQLGetData** avec une petite mémoire tampon plusieurs fois afin de récupérer une valeur de paramètre de grande taille. Pour plus d’informations, consultez [la récupération des paramètres de sortie à l’aide de SQLGetData](http://go.microsoft.com/fwlink/?LinkID=191494).  

 Les rubriques suivantes décrivent des changements de comportement de Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  

-   Lors de l’appel **ICommandWithParameters::SetParameterInfo**, la valeur passée à la *pwszName* paramètre doit être un identificateur valide. Pour plus d’informations, consultez [ICommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/icommandwithparameters.md).  

-   **SQLDescribeParam** retournera toujours une valeur conforme de spécification ODBC. Pour plus d’informations, consultez [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md).  

-   [Changement de comportement du pilote ODBC lors de la gestion des conversions de caractères](../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  

## <a name="see-also"></a>Voir aussi  
[Installez SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
 [Fonctionnalités de SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md)  
