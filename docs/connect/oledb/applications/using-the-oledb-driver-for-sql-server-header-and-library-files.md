---
title: À l’aide du pilote de base de données OLE pour SQL Server en-tête et les fichiers de bibliothèque | Documents Microsoft
description: À l’aide du pilote OLE DB pour les fichiers de bibliothèque et d’en-tête SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- header files [OLE DB Driver for SQL Server]
- MSOLEDBSQL, header files
- OLE DB, header files
- library files [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, header files
- data access [OLE DB Driver for SQL Server], header files
- data access [OLE DB Driver for SQL Server], library files
- OLE DB Driver for SQL Server, library files
- MSOLEDBSQL, library files
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d958d4b1f12f5a109c5727832eb764fd2b497d70
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-the-ole-db-driver-for-sql-server-header-and-library-files"></a>À l’aide du pilote de base de données OLE pour SQL Server en-tête et les fichiers de bibliothèque
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Le pilote OLE DB pour l’en-tête de SQL Server et les fichiers bibliothèques sont installés lorsque le pilote OLE DB pour l’option de kit de développement logiciel de SQL Server est activée au cours du processus d’installation. Lors du développement d'une application, il importe de copier et d'installer tous les fichiers requis pour le développement sur votre environnement de développement. Pour plus d’informations sur l’installation et la redistribution du pilote OLE DB pour SQL Server, consultez [installation pilote OLE DB pour SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md).  
  
 Le pilote OLE DB pour l’en-tête de SQL Server et les fichiers bibliothèques sont installés à l’emplacement suivant :  
  
 *%Program Files%* \Microsoft SQL Server\Client SDK\OLEDB\180\SDK  
  
 Le pilote OLE DB pour le fichier d’en-tête SQL Server (msoledbsql.h) peut être utilisé pour ajouter le pilote OLE DB pour la fonctionnalité d’accès de données de SQL Server pour vos applications personnalisées. Le pilote OLE DB pour le fichier d’en-tête SQL Server contient toutes les définitions, des attributs, des propriétés et les interfaces nécessaires pour tirer parti des nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Outre le pilote OLE DB pour le fichier d’en-tête SQL Server, il existe également un fichier de bibliothèque de msoledbsql.lib qui est la bibliothèque d’exportation pour [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) fonctionnalité.  
  
 Le pilote OLE DB pour le fichier d’en-tête SQL Server est rétrocompatible avec le fichier d’en-tête sqloledb.h utilisé avec Microsoft Data Access Components (MDAC), mais ne contient pas les CLSID pour SQLOLEDB (le fournisseur OLE DB pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inclus avec MDAC) ou des symboles pour Fonctionnalités XML (ce qui ne sont pas pris en charge par le pilote OLE DB pour SQL Server).    
  
 Les applications OLE DB qui utilisent le pilote OLE DB pour SQL Server doivent uniquement référencer msoledbsql.h. Si une application utilise MDAC (SQLOLEDB) et le pilote OLE DB pour SQL Server, il peut faire référence à sqloledb.h et msoledbsql.h, mais la référence à sqloledb.h doit figurer en premier.  
  
## <a name="using-the-ole-db-driver-for-sql-server-header-file"></a>À l’aide du pilote de base de données OLE de fichier d’en-tête SQL Server  
 Pour utiliser le pilote OLE DB pour le fichier d’en-tête SQL Server, vous devez utiliser un **incluent** instruction dans votre code de programmation C/C++. Les sections suivantes décrivent comment effectuer cela les applications OLE DB.  
  
> [!NOTE]  
>  Le pilote OLE DB pour les fichiers de bibliothèque et d’en-tête SQL Server peut uniquement être compilée à l’aide de Visual Studio C++ 2012 ou version ultérieure.  
  
### <a name="ole-db"></a>OLE DB  
 Pour utiliser le pilote OLE DB pour le fichier d’en-tête SQL Server dans une application OLE DB, à l’aide de lignes de code de programmation suivantes :  
  
```    
include "msoledbsql.h";  
```  
  
> [!NOTE]  
>  Si l’application possède un **incluent** instruction pour sqloledb.h, le **incluent** instruction pour msoledbsql.h doit venir après elle.  
  
 Lorsque vous créez une connexion à une source de données via le pilote OLE DB pour SQL Server, utilisez « MSOLEDBSQL » comme chaîne de nom du fournisseur.  

  
## <a name="component-names-and-properties-by-version"></a>Noms des composants et propriétés par version  

|Propriété|Pilote d’OLE DB pour SQL Server|MDAC|  
|--------|----------------------------|----|   
|PROGID pour OLE DB|MSOLEDBSQL|SQLOLEDB|  
|Nom du fichier d'en-tête OLE DB|msoledbsql.h|Sqloledb.h|  
|DLL du fournisseur OLE DB|msoledbsql.dll|Sqloledb.dll| 
  
  
## <a name="static-linking-and-bcp-functions"></a>Liaison statique et fonctions BCP  
 Lorsqu'une application utilise des fonctions BCP, il est important pour l'application de spécifier dans la chaîne de connexion le pilote de la même version fourni avec le fichier d'en-tête et la bibliothèque utilisés pour compiler l'application.  
  
 Pour plus d’informations, consultez exécution [effectuant des opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Génération d’applications avec OLE DB Driver pour SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
  
  
