---
title: Exécution d’opérations asynchrones | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- initialization [SQL Server Native Client]
- database connections [SQL Server Native Client]
- data access [SQL Server Native Client], asynchronous operations
- connections [SQL Server Native Client]
- asynchronous operations [SQL Server Native Client]
- rowsets [SQL Server], initializing
- SQLNCLI, asynchronous operations
- SQL Server Native Client, asynchronous operations
ms.assetid: 8fbd84b4-69cb-4708-9f0f-bbdf69029bcc
caps.latest.revision: 45
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1b433851601bb3bbe1a9bd339b4af2b0835c1673
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="performing-asynchronous-operations"></a>Exécution d'opérations asynchrones
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet aux applications d'effectuer des opérations de base de données asynchrones. Le traitement asynchrone permet aux méthodes d'être retournées immédiatement sans blocage du thread appelant. Cela rend le multithreading plus puissant et plus souple, sans obliger le développeur à créer explicitement des threads ou à gérer la synchronisation. Les applications requièrent un traitement asynchrone lors de l'initialisation d'une connexion de base de données, ou lors de l'initialisation du résultat de l'exécution d'une commande.  
  
## <a name="opening-and-closing-a-database-connection"></a>Ouverture et fermeture d'une connexion de base de données  
 Lorsque vous utilisez la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif, les applications conçues pour initialiser un objet de source de données de façon asynchrone peuvent définir DBPROPVAL_ASYNCH_INITIALIZE dans la propriété DBPROP_INIT_ASYNCH avant d’appeler **IDBInitialize::Initialize**. Lorsque cette propriété est définie, le fournisseur retourne immédiatement à partir de l’appel à **initialiser** avec S_OK, si l’opération s’est effectuée immédiatement, ou DB_S_ASYNCHRONOUS, si l’initialisation se poursuit de façon asynchrone. Les applications peuvent interroger pour les **IDBAsynchStatus** ou [ISSAsynchStatus](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)interface sur l’objet de source de données, puis appelez **IDBAsynchStatus::GetStatus** ou[ISSAsynchStatus::WaitForAsynchCompletion](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) pour obtenir l’état de l’initialisation.  
  
 De plus, la propriété SSPROP_ISSAsynchStatus a été ajoutée au jeu de propriétés DBPROPSET_SQLSERVERROWSET. Les fournisseurs qui prennent en charge l'interface **ISSAsynchStatus** doivent implémenter cette propriété avec la valeur VARIANT_TRUE.  
  
 **IDBAsynchStatus::Abort** ou [ISSAsynchStatus::Abort](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-abort-ole-db.md) peut être appelée pour annuler asynchrone **initialiser** appeler. Le consommateur doit demander explicitement l'initialisation de la source de données asynchrone. Dans le cas contraire, **IDBInitialize::Initialize** ne retourne pas jusqu'à ce que l’objet de source de données est entièrement initialisé.  
  
> [!NOTE]  
>  Les objets de source de données utilisés pour le regroupement de connexions ne peut pas appeler le **ISSAsynchStatus** interface dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif. Le **ISSAsynchStatus** interface n’est pas exposée pour les objets de source de données regroupés.  
>   
>  Si une application force explicitement l’utilisation du moteur de curseur, **IOpenRowset::OpenRowset** et **IMultipleResults::GetResult** ne prendra pas en charge le traitement asynchrone.  
>   
>  En outre, la dll de proxy/stub de la communication à distance (dans MDAC 2.8) ne peut pas appeler le **ISSAsynchStatus** dans l’interface [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Le **ISSAsynchStatus** interface n’est pas exposée via la communication à distance.  
>   
>  Composants de service ne gèrent pas **ISSAsynchStatus**.  
  
## <a name="execution-and-rowset-initialization"></a>Exécution et initialisation de l'ensemble de lignes  
 Les applications conçues pour ouvrir de façon asynchrone le résultat de l'exécution d'une commande peuvent définir DBPROPVAL_ASYNCH_INITIALIZE dans la propriété DBPROP_ROWSET_ASYNCH. Lors de la définition de ce bit avant d’appeler **IDBInitialize::Initialize**, **ICommand::Execute**, **IOpenRowset::OpenRowset** ou **IMultipleResults::GetResult**, le *riid* argument doit être défini à IID_IDBAsynchStatus, IID_ISSAsynchStatus ou IID_IUnknown.  
  
 La méthode est retournée immédiatement avec S_OK si l’initialisation de l’ensemble de lignes se termine immédiatement, ou avec DB_S_ASYNCHRONOUS si l’ensemble de lignes poursuit son initialisation de façon asynchrone, avec *ppRowset* définie sur l’interface demandée sur l’ensemble de lignes. Pour le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif, cette interface peut être uniquement **IDBAsynchStatus** ou **ISSAsynchStatus**. Jusqu'à ce que l’ensemble de lignes soit entièrement initialisé, cette interface se comporte comme si elle était dans un état suspendu et l’appel **QueryInterface** pour les interfaces **IID_IDBAsynchStatus** ou **IID_ISSAsynchStatus** peut retourner E_NOINTERFACE. À moins que le consommateur ne demande explicitement un traitement asynchrone, l'ensemble de lignes est initialisé de façon synchrone. Toutes les interfaces demandées sont disponibles quand **IDBAsynchStaus::GetStatus** ou **ISSAsynchStatus::WaitForAsynchCompletion** retourné avec une indication que l’opération asynchrone est terminée. Cela ne signifie pas nécessairement que l'ensemble de lignes soit entièrement rempli, mais il est prêt et complètement fonctionnel.  
  
 Si la commande exécutée ne retourne pas d’un ensemble de lignes, il retourne toujours immédiatement avec un objet qui prend en charge **IDBAsynchStatus**.  
  
 S'il vous faut obtenir plusieurs résultats à partir de l'exécution d'une commande asynchrone, vous devez :  
  
-   définir DBPROPVAL_ASYNCH_INITIALIZE dans la propriété DBPROP_ROWSET_ASYNCH, avant d'exécuter la commande ;  
  
-   Appelez **ICommand::Execute**et demande **IMultipleResults**.  
  
 Le **IDBAsynchStatus** et **ISSAsynchStatus** interfaces peuvent ensuite être obtenues en interrogeant l’interface de résultats multiples à l’aide **QueryInterface**.  
  
 Lorsque la commande a terminé l’exécution, **IMultipleResults** peut servir comme d’habitude, avec une exception à partir d’un cas synchrone : DB_S_ASYNCHRONOUS peut être retourné, auquel cas **IDBAsynchStatus** ou **ISSAsynchStatus** peut être utilisé pour déterminer si l’opération est terminée.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, l'application appelle une méthode non bloquante, effectue d'autres traitements, puis retourne au traitement des résultats. **ISSAsynchStatus::WaitForAsynchCompletion** attend sur l’objet d’événement interne jusqu'à ce que l’opération d’exécution asynchrone est terminée ou la quantité de temps spécifié par *dwMilisecTimeOut* est passé.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 **ISSAsynchStatus::WaitForAsynchCompletion** attend sur l’objet d’événement interne jusqu'à ce que l’opération d’exécution asynchrone est terminée ou le *dwMilisecTimeOut* la valeur est passée.  
  
 L'exemple suivant illustre le traitement asynchrone avec plusieurs jeux de résultats :  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 Pour empêcher tout blocage, le client peut vérifier l'état d'une opération asynchrone en cours d'exécution, comme le montre l'exemple suivant :  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 L'exemple suivant montre comment vous pouvez annuler l'opération asynchrone en cours d'exécution :  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [Propriétés et comportements de l’ensemble de lignes](../../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus & #40 ; OLE DB & #41 ;](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
