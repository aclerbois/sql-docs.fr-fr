---
title: L’établissement d’une connexion à une Source de données | Documents Microsoft
description: L’établissement d’une connexion à une source de données à l’aide du pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb-driver-for-sql-server
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- data sources [OLE DB Driver for SQL Server]
- connections [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, data source connections
- CoCreateInstance method
- OLE DB data sources [OLE DB Driver for SQL Server]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 41a5890e7c7e3638267fc85723911c7f6c861276
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="establishing-a-connection-to-a-data-source"></a>Établissement d'une connexion à une source de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Pour accéder au pilote OLE DB pour SQL Server, le consommateur doit d’abord créer une instance d’un objet de source de données en appelant le **CoCreateInstance** (méthode). Un identificateur de classe unique (CLSID) identifie chaque fournisseur OLE DB. Pour le pilote OLE DB pour SQL Server, l’identificateur de classe est CLSID_MSOLEDBSQL. Vous pouvez également utiliser le symbole MSOLEDBSQL_CLSID qui sera résolu en le pilote OLE DB pour SQL Server qui est utilisé dans le msoledbsql.h que vous référencez.  
  
 La source de données objet expose le **IDBProperties** interface, le consommateur utilise pour fournir des informations d’authentification de base telles que le nom du serveur, nom de la base de données, ID d’utilisateur et mot de passe. Le **IDBProperties::SetProperties** méthode est appelée pour définir ces propriétés.  
  
 S'il existe plusieurs instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui s'exécutent sur l'ordinateur, le nom du serveur est spécifié sous la forme NomServeur\NomInstance.  
  
 L’objet de source de données expose également le **IDBInitialize** interface. Une fois que les propriétés sont définies, la connexion à la source de données est établie en appelant le **IDBInitialize::Initialize** (méthode). Par exemple :  
  
```  
CoCreateInstance(CLSID_MSOLEDBSQL,   
                 NULL,   
                 CLSCTX_INPROC_SERVER,  
                 IID_IDBInitialize,   
                 (void **) &pIDBInitialize)  
```  
  
 Cet appel à **CoCreateInstance** crée un objet unique de la classe associée CLSID_MSOLEDBSQL (CSLID associé avec les données et le code permettant de créer l’objet). IID_IDBInitialize est une référence à l’identificateur de l’interface (**IDBInitialize**) à utiliser pour communiquer avec l’objet.  
  
 Les éléments suivants sont un exemple de fonction qui initialise et établit une connexion à la source de données.  
  
```  
void InitializeAndEstablishConnection() {  
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the OLE DB Driver for SQL Server.  
   hr = CoCreateInstance(CLSID_MSOLEDBSQL,   
                         NULL,   
                         CLSCTX_INPROC_SERVER,  
                         IID_IDBInitialize,   
                         (void **) &pIDBInitialize);  
   // Initialize property values needed to establish connection.  
   for (i = 0 ; i < 4 ; i++)   
      VariantInit(&InitProperties[i].vValue);  
  
   // Server name.  
   // See DBPROP structure for more information on InitProperties  
   InitProperties[0].dwPropertyID  = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt    = VT_BSTR;  
   InitProperties[0].vValue.bstrVal=   
                     SysAllocString(L"Server");  
   InitProperties[0].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid       = DB_NULLID;  
  
   // Database.  
   InitProperties[1].dwPropertyID  = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt    = VT_BSTR;  
   InitProperties[1].vValue.bstrVal= SysAllocString(L"database");  
   InitProperties[1].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid       = DB_NULLID;  
  
   // Username (login).  
   InitProperties[2].dwPropertyID  = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt    = VT_BSTR;  
   InitProperties[2].vValue.bstrVal= SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid       = DB_NULLID;  
   InitProperties[3].dwOptions    = DBPROPOPTIONS_REQUIRED;  
   InitProperties[3].colid       = DB_NULLID;  
  
   // Construct the DBPROPSET structure(rgInitPropSet). The   
   // DBPROPSET structure is used to pass an array of DBPROP   
   // structures (InitProperties) to the SetProperties method.  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties   = 4;  
   rgInitPropSet[0].rgProperties   = InitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties,   
                           (void **)&pIDBProperties);  
   hr = pIDBProperties->SetProperties(1, rgInitPropSet);   
   pIDBProperties->Release();  
  
   // Now establish the connection to the data source.  
   pIDBInitialize->Initialize();  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’une application OLE DB Driver pour SQL Server](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)  
  
  
