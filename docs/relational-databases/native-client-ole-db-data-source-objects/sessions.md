---
title: Sessions | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-data-source-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- SQL Server Native Client OLE DB provider, sessions
ms.assetid: 3a980816-675c-4fba-acc9-429297d85bbd
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6de9aa78a6cbdd26900eb4565f3edc7783acdeb9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sessions"></a>Sessions
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  A [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] session du fournisseur OLE DB Native Client représente une connexion unique à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif requiert que les sessions délimitent l’espace de transaction pour une source de données. Tous les objets de commande créés à partir d'un objet session spécifique participent à la transaction locale ou distribuée de l'objet session.  
  
 Le premier objet session créé sur la source de données initialisée reçoit la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] établie au moment de l'initialisation. Lorsque toutes les références sur les interfaces de l'objet session sont libérées, la connexion à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est accessible à un autre objet session créé sur la source de données.  
  
 Un objet session supplémentaire créé sur la source de données établit sa propre connexion à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme spécifié par la source de données. La connexion à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est abandonnée lorsque l'application libère toutes les références aux objets créés au cours de cette session.  
  
 L’exemple suivant montre comment utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client pour se connecter à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données :  
  
```  
int main()  
{  
    // Interfaces used in the example.  
    IDBInitialize*      pIDBInitialize      = NULL;  
    IDBCreateSession*   pIDBCreateSession   = NULL;  
    IDBCreateCommand*   pICreateCmd1        = NULL;  
    IDBCreateCommand*   pICreateCmd2        = NULL;  
    IDBCreateCommand*   pICreateCmd3        = NULL;  
  
    // Initialize COM.  
    if (FAILED(CoInitialize(NULL)))  
    {  
        // Display error from CoInitialize.  
        return (-1);  
    }  
  
    // Get the memory allocator for this task.  
    if (FAILED(CoGetMalloc(MEMCTX_TASK, &g_pIMalloc)))  
    {  
        // Display error from CoGetMalloc.  
        goto EXIT;  
    }  
  
    // Create an instance of the data source object.  
    if (FAILED(CoCreateInstance(CLSID_SQLNCLI10, NULL,  
        CLSCTX_INPROC_SERVER, IID_IDBInitialize, (void**)  
        &pIDBInitialize)))  
    {  
        // Display error from CoCreateInstance.  
        goto EXIT;  
    }  
  
    // The InitFromPersistedDS function   
    // performs IDBInitialize->Initialize() establishing  
    // the first application connection to the instance of SQL Server.  
    if (FAILED(InitFromPersistedDS(pIDBInitialize, L"MyDataSource",  
        NULL, NULL)))  
    {  
        goto EXIT;  
    }  
  
    // The IDBCreateSession interface is implemented on the data source  
    // object. Maintaining the reference received maintains the   
    // connection of the data source to the instance of SQL Server.  
    if (FAILED(pIDBInitialize->QueryInterface(IID_IDBCreateSession,  
        (void**) &pIDBCreateSession)))  
    {  
        // Display error from pIDBInitialize.  
        goto EXIT;  
    }  
  
    // Releasing this has no effect on the SQL Server connection  
    // of the data source object because of the reference maintained by  
    // pIDBCreateSession.  
    pIDBInitialize->Release();  
    pIDBInitialize = NULL;  
  
    // The session created next receives the SQL Server connection of  
    // the data source object. No new connection is established.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd1)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
    // A new connection to the instance of SQL Server is established to support the  
    // next session object created. On successful completion, the  
    // application has two active connections on the SQL Server.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd2)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
    // pICreateCmd1 has the data source connection. Because the  
    // reference on the IDBCreateSession interface of the data source  
    // has not been released, releasing the reference on the session  
    // object does not terminate a connection to the instance of SQL Server.  
    // However, the connection of the data source object is now   
    // available to another session object. After a successful call to   
    // Release, the application still has two active connections to the   
    // instance of SQL Server.  
    pICreateCmd1->Release();  
    pICreateCmd1 = NULL;  
  
    // The next session created gets the SQL Server connection  
    // of the data source object. The application has two active  
    // connections to the instance of SQL Server.  
    if (FAILED(pIDBCreateSession->CreateSession(NULL,  
        IID_IDBCreateCommand, (IUnknown**) &pICreateCmd3)))  
    {  
        // Display error from pIDBCreateSession.  
        goto EXIT;  
    }  
  
EXIT:  
    // Even on error, this does not terminate a SQL Server connection   
    // because pICreateCmd1 has the connection of the data source   
    // object.  
    if (pICreateCmd1 != NULL)  
        pICreateCmd1->Release();  
  
    // Releasing the reference on pICreateCmd2 terminates the SQL  
    // Server connection supporting the session object. The application  
    // now has only a single active connection on the instance of SQL Server.  
    if (pICreateCmd2 != NULL)  
        pICreateCmd2->Release();  
  
    // Even on error, this does not terminate a SQL Server connection   
    // because pICreateCmd3 has the connection of the   
    // data source object.  
    if (pICreateCmd3 != NULL)  
        pICreateCmd3->Release();  
  
    // On release of the last reference on a data source interface, the  
    // connection of the data source object to the instance of SQL Server is broken.  
    // The example application now has no SQL Server connections active.  
    if (pIDBCreateSession != NULL)  
        pIDBCreateSession->Release();  
  
    // Called only if an error occurred while attempting to get a   
    // reference on the IDBCreateSession interface of the data source.  
    // If so, the call to IDBInitialize::Uninitialize terminates the   
    // connection of the data source object to the instance of SQL Server.  
    if (pIDBInitialize != NULL)  
    {  
        if (FAILED(pIDBInitialize->Uninitialize()))  
        {  
            // Uninitialize is not required, but it fails if an  
            // interface has not been released. Use it for  
            // debugging.  
        }  
        pIDBInitialize->Release();  
    }  
  
    if (g_pIMalloc != NULL)  
        g_pIMalloc->Release();  
  
    CoUninitialize();  
  
    return (0);  
}  
```  
  
 Connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets session du fournisseur OLE DB Native Client à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent générer une surcharge significative pour les applications qui en permanence de créer et de libérer les objets de session. La surcharge peut être réduite grâce à la gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] efficacement les objets session du fournisseur OLE DB Native Client. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Applications de fournisseur OLE DB du Client natives peuvent conserver la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion d’un objet de session active en gérant une référence sur au moins une interface de l’objet.  
  
 Par exemple, la gestion d'un pool de références d'objet de création de commande maintient des connexions actives pour ces objets session dans le pool. Objets session étant requis, le code de maintenance de pool passe valide **IDBCreateCommand** pointeur d’interface pour la méthode d’application qui requiert la session. Lorsque la méthode d'application ne requiert plus la session, la méthode retourne le pointeur d'interface au code de gestion du pool plutôt que de libérer la référence de l'application à l'objet de création de commande.  
  
> [!NOTE]  
>  Dans l’exemple précédent, le **IDBCreateCommand** interface est utilisée, car le **ICommand** interface implémente le **GetDBSession** (méthode), la seule méthode dans l’étendue ensemble de lignes ou de la commande qui permet à un objet déterminer la session à laquelle il a été créé. Par conséquent, un objet commande, et uniquement un objet commande, permet à une application de récupérer un pointeur d'objet source de données à partir duquel des sessions supplémentaires peuvent être créées.  
  
## <a name="see-also"></a>Voir aussi  
 [Objets Source de données & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
