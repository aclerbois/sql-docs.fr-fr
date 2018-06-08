---
title: Objets (OLE DB) la Source de données | Documents Microsoft
description: Objets source de données (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-data-source-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], data source objects
- OLE DB Driver for SQL Server, data source objects
- MSOLEDBSQL, data source objects
- OLE DB Driver for SQL Server, data source objects
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- CLSID
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 2eb3583366cf896ecf2382a5d2f36d1298e4c4a4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-objects-ole-db"></a>Objets source de données (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Pilote OLE DB pour SQL Server utilise la terme source de données pour l’ensemble des interfaces OLE DB utilisé pour établir un lien vers un magasin de données, tel que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Création d’une instance de l’objet de source de données du fournisseur est la première tâche d’un pilote OLE DB pour le consommateur de SQL Server.  
  
 Chaque fournisseur OLE DB déclare un identificateur de classe (CLSID) pour lui-même. Le CLSID pour le pilote OLE DB pour SQL Server est le CLSID_MSOLEDBSQL GUID C/C++ (le symbole MSOLEDBSQL_CLSID résoudra correct progid dans le fichier msoledbsql.h que vous référencez). Avec le CLSID, le consommateur utilise OLE **CoCreateInstance** fonction pour fabriquer une instance de l’objet de source de données.  
  
 Pilote OLE DB pour SQL Server est un serveur in-process. Instances de pilote OLE DB pour les objets SQL Server sont créés à l’aide de la macro CLSCTX_INPROC_SERVER de manière à indiquer le contexte exécutable.  
  
 Le pilote OLE DB pour l’objet de source de données SQL Server expose les interfaces d’initialisation OLE DB qui permettent au consommateur de se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bases de données.  
  
 Chaque connexion établie via le pilote OLE DB pour SQL Server définit automatiquement les options suivantes :  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 Cet exemple utilise la macro d’identificateur de classe pour créer un pilote OLE DB pour l’objet de source de données SQL Server et obtenir une référence à son **IDBInitialize** interface.  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_MSOLEDBSQL, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 Création réussie d’une instance d’un pilote OLE DB pour l’objet de source de données SQL Server, l’application consommateur peut continuer par l’initialisation de la source de données et création de sessions. Les sessions OLE DB présentent des interfaces qui permettent d'accéder à des données et de les manipuler.  
  
 Le pilote OLE DB pour SQL Server établit sa première connexion à une instance spécifique de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le cadre d’une initialisation de source de données. La connexion est maintenue tant qu’une référence est maintenue sur n’importe quelle interface de l’initialisation de source de données, ou jusqu'à ce que le **IDBInitialize::Uninitialize** méthode est appelée.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Propriétés de Source de données & #40 ; OLE DB & #41 ;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [Propriétés d’informations de Source de données](../../oledb/ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [Propriétés d’initialisation et d’autorisation](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [Sessions](../../oledb/ole-db-data-source-objects/sessions.md)  
  
-   [Propriétés de session - OLE DB Driver pour SQL Server](../../oledb/ole-db-data-source-objects/session-properties-oledb-driver-for-sql-server.md)  
  
-   [Persistante des objets Source de données](../../oledb/ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation OLE DB Driver pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
