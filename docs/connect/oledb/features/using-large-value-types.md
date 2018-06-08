---
title: À l’aide des Types de valeur volumineux | Documents Microsoft
description: À l’aide des types de valeur volumineux avec le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- large value data types
- MSOLEDBSQL, large value types
- OLE DB Driver for SQL Server, large value types
- data access [OLE DB Driver for SQL Server], large value types
- OLE DB Driver for SQL Server, large value data types
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 6c610ff761a2b1af3ee4f0feda402346efc1dcd6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-large-value-types"></a>Utilisation de types de valeur élevée
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Avant [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], l'utilisation de types de données de valeur élevée nécessitait une gestion particulière. Types de données de valeur élevée sont les types qui dépassent la taille de ligne maximale de 8 Ko. [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] introduit un **max** spécificateur pour **varchar**, **nvarchar**, et **varbinary** des types de données pour permettre le stockage de valeurs aussi grand que 2 ^ 31 -1 octets. Colonnes de la table et [!INCLUDE[tsql](../../../includes/tsql-md.md)] variables peuvent spécifier **varchar (max)**, **nvarchar (max)**, ou **varbinary (max)** des types de données.  
  
> [!NOTE]  
>  Types de données de valeur élevée peuvent avoir une taille maximale entre 1 et 8 Ko, ou elles peuvent être spécifiées comme un nombre illimité.  
  
 Auparavant, seuls [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tels que des types de données **texte**, **ntext** et **image** pouvaient atteindre de telles longueurs. Le **max** spécificateur pour **varchar**, **nvarchar**, et **varbinary** licenciés ces types de données. Toutefois, étant donné que les types de données long sont toujours disponibles, la plupart des interfaces pour les composants d’accès aux données OLE DB reste le même. Pour la compatibilité descendante avec les versions antérieures, l’indicateur DBCOLUMNFLAGS_ISLONG dans le pilote OLE DB pour SQL Server est en cours d’utilisation. Les fournisseurs et les pilotes écrits pour [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures continuent d'utiliser ces termes pour les nouveaux types lorsque la longueur maximale illimitée est spécifiée.  
  
> [!NOTE]  
>  Vous pouvez également spécifier **varchar (max)**, **nvarchar (max)**, et **varbinary (max)** des types de données en tant que types de paramètre d’entrée et de sortie des procédures stockées, types de retour de fonction, ou dans [CAST et CONVERT](../../../t-sql/functions/cast-and-convert-transact-sql.md) fonctions.  
  
> [!NOTE]  
>  Si vous répliquez des données, vous devrez peut-être configurer le [max d’option de configuration serveur text repl size](../../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) -1.  
  
## <a name="ole-db-driver-for-sql-server"></a>Pilote d’OLE DB pour SQL Server 
 Le pilote OLE DB pour SQL Server expose la **varchar (max)**, **varbinary (max)**, et **nvarchar (max)** respectivement les types en tant que DBTYPE_STR, DBTYPE_BYTES et DBTYPE_WSTR, .  
  
 Les types de données **varchar (max)**, **varbinary (max)**, et **nvarchar (max)** dans des colonnes ayant le **max** taille définie illimitée sont représentés en tant que ISLONG via les ensembles de lignes du schéma OLE DB principaux et les interfaces qui retournent des types de données de colonne.  
  
 L’objet de commande **IAccessor** implémentation a été modifiée pour autoriser la liaison en tant que DBTYPE_IUNKNOWN. Si le consommateur spécifie DBTYPE_IUNKNOWN et définit *pObject* à null, le fournisseur retournera le **ISequentialStream** d’interface au consommateur afin que le consommateur peut diffuser en continu **varchar (max)**, **nvarchar (max)**, ou **varbinary (max)** données de variables de sortie.  
  
 Les valeurs de paramètres de sortie diffusées en continu sont retournées après les lignes de résultat (le cas échéant). Si l’application tente de passer au résultat suivant défini en appelant **IMultipleResults::GetResult** sans consommer toutes les valeurs de paramètre de sortie retournées, DB_E_OBJECTOPEN sera retourné.  
  
 Pour prendre en charge la diffusion en continu, le pilote OLE DB pour SQL Server requiert des paramètres de longueur variable accessibles dans un ordre séquentiel. Cela signifie que DBPROP_ACCESSORDER doit être la valeur DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS ou DBPROPVAL_AO_SEQUENTIAL chaque fois que **varchar (max)**, **nvarchchar (max)**, ou **varbinary (max)** colonnes ou des paramètres de sortie sont liés à DBTYPE_IUNKNOWN. Les appels à **IRowset::GetData** échoue avec DBSTATUS_E_UNAVAILABLE si cette restriction de commande d’accès n’est pas respectée. Cette restriction ne s'applique pas lorsqu'aucune liaison de sortie n'utilise DBTYPE_IUNKNOWN.  
  
 Le pilote OLE DB pour SQL Server prend également en charge la liaison des paramètres de sortie en tant que DBTYPE_IUNKNOWN pour les types de données de valeur élevée simplifier les scénarios où une procédure stockée retourne des types de valeur élevée comme valeurs de retour qui sont exposées en tant que DBTYPE_IUNKNOWN au client.  
  
 Pour utiliser ces types, une application dispose des options suivantes :  
  
-   Lier en tant que type qui a pris en charge les liaisons avec le type de base de la colonne (par exemple, pour nvarchar (max), lier en tant que type pouvant être lié à nvarchar). Si la mémoire tampon n’est pas importante suffisamment troncation se produit, exactement comme pour le type de base, quoique des valeurs supérieures sont désormais disponibles.  
  
-   Lier en tant que type ayant des conversions prises en charge avec le type de base de la colonne et spécifier également DBTYPE_BYREF.  
  
-   Lier comme DBTYPE_IUNKNOWN et utiliser la diffusion en continu.  
  
 Lors du signalement de la taille maximale d’une colonne, le pilote OLE DB pour SQL Server signalera :  
  
-   La taille maximale définie qui, par exemple, est 2000 pour une **varchar (** 2000 **)** colonne, ou  
  
-   La valeur « illimitée » qui dans le cas d’un **varchar (max)** colonne est égale à ~ 0. Cette valeur est définie pour la propriété de métadonnées DBCOLUMN_COLUMNSIZE.  
  
 Les règles de conversion standard s’appliquent à un **varchar (max)** colonne, ce qui signifie que toute conversion valide pour un **varchar (** 2000 **)** colonne sera également valide pour un **varchar (max)** colonne. Est de même pour **nvarchar (max)** et **varbinary (max)** colonnes.  
  
 Lors de la récupération de types de valeur élevée, l'approche la plus efficace consiste à lier en tant que DBTYPE_IUNKNOWN et à attribuer à la propriété d'ensemble de lignes DBPROP_ACCESSORDER la valeur DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS. La valeur sera alors diffusée en continu directement à partir du réseau sans mise en mémoire tampon intermédiaire, comme dans l'exemple suivant :  
  
```  
#define UNICODE  
#define _UNICODE  
#define DBINITCONSTANTS  
#define INITGUID  
#define OLEDBVER 0x0250  // To include the correct interfaces.  
  
#include <stdio.h>  
#include <tchar.h>  
#include <stddef.h>  
#include <iostream>  
  
using std::cout;  
using std::endl;  
  
#include <windows.h>  
  
#include <oledb.h>  
#include "msoledbsql.h"  
#include <oledberr.h>  
  
#define CHKHR_GOTO(hr, errMsg, Label) \  
   if (FAILED(hr)) \  
   { \  
      cout << errMsg << endl; \  
      goto Label; \  
   }  
  
#define MAX_COL_SIZE 8000  
  
// ROUNDUP on all platforms pointers must be aligned properly.  
#define ROUNDUP_AMOUNT 8  
#define ROUNDUP_(size,amount) (((ULONG)(size)+((amount)-1))&~((amount)-1))  
#define ROUNDUP(size) ROUNDUP_(size, ROUNDUP_AMOUNT)  
  
HRESULT InitializeAndEstablishConnection(IDBInitialize** ppIDBInitialize);  
void UnInitializeConnection(IDBInitialize* pIDBInitialize);  
HRESULT CreateAndSetCommand(IDBInitialize* pIDBInitialize, ICommandText** ppICommandText);  
HRESULT ProcessResultSet(IRowset* pIRowset);  
  
void DisplayTime()  
{  
   SYSTEMTIME st;  
   GetSystemTime(&st);  
   cout<< st.wHour << ":" << st.wMinute << ":" << st.wSecond << "." << st.wMilliseconds << endl;  
}  
  
void main()  
{  
   HRESULT hr;  
   IDBInitialize* pIDBInitialize = NULL;  
   ICommandText* pICommandText = NULL;  
   IMultipleResults* pIMultipleResults = NULL;  
   IRowset* pIRowset = NULL;  
  
   hr = InitializeAndEstablishConnection(&pIDBInitialize);  
   CHKHR_GOTO(hr, L"Failed to establish connection.", _ExitMain);  
  
   hr = CreateAndSetCommand(pIDBInitialize, &pICommandText);  
   CHKHR_GOTO(hr, L"Failed to set up command object.", _ExitMain);  
  
   DisplayTime();  
  
   hr = pICommandText->Execute(NULL,   
      IID_IMultipleResults,   
      NULL,   
      NULL,   
     (IUnknown **) &pIMultipleResults);  
  
   CHKHR_GOTO(hr, L"Failed to execute command.", _ExitMain);  
  
   while (1)  
   {  
      hr = pIMultipleResults->GetResult(  
         NULL,   
         DBRESULTFLAG_DEFAULT,   
         IID_IRowset,   
         NULL,   
         (IUnknown**)&pIRowset);  
  
   CHKHR_GOTO(hr, L"Failed to obtain a results from MR object.", _ExitMain);  
  
   if (hr == DB_S_NORESULT)  
      break;  
  
      if (pIRowset)  
      {  
         hr = ProcessResultSet(pIRowset);   
         CHKHR_GOTO(hr, L"Failed to process the current Rowset.", _ExitMain);  
  
         pIRowset->Release();  
         pIRowset = NULL;  
      }  
   }  
  
   DisplayTime();  
  
_ExitMain:  
  
   if (pIRowset)  
   {  
      pIRowset->Release();  
      pIRowset = NULL;  
   }  
  
   if (pIMultipleResults)  
   {  
      pIMultipleResults->Release();  
      pIMultipleResults = NULL;  
   }  
  
   if (pICommandText)  
   {  
      pICommandText->Release();  
      pICommandText = NULL;  
   }  
  
   UnInitializeConnection(pIDBInitialize);  
   return;  
};  
  
HRESULT InitializeAndEstablishConnection(IDBInitialize** ppIDBInitialize)  
{  
   HRESULT hr;  
   IDBInitialize* pIDBInitialize = NULL;  
   IDBProperties* pIDBProperties = NULL;  
  
   const int NUM_DBINIT_PROPS = 3;  
   const wchar_t* const g_wszServer = L".";  
   const wchar_t* const g_wszCatalog = L"AdventureWorks";  
   const wchar_t* const g_wszSecurity = L"SSPI";  
  
   DBPROPSET rgdbPropSetInit[1];  
   DBPROP rgdbPropInit [NUM_DBINIT_PROPS];  
  
   *ppIDBInitialize = NULL;  
   hr = CoInitialize(NULL);  
   CHKHR_GOTO(hr, L"Failed to initialize COM.", _ExitInitialize);  
  
   hr = CoCreateInstance(CLSID_MSOLEDBSQL,   
      NULL,   
      CLSCTX_INPROC_SERVER,  
      IID_IDBInitialize,   
      (void**)&pIDBInitialize);  
  
   CHKHR_GOTO(hr, L"Failed to create MSOLEDBSQL DataSource object.", _ExitInitialize);  
  
   for(int idxProp = 0; idxProp < NUM_DBINIT_PROPS; idxProp++)   
   {  
      VariantInit(&rgdbPropInit[idxProp].vValue);  
   }  
  
   rgdbPropInit[0].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   rgdbPropInit[0].vValue.vt = VT_BSTR;  
   rgdbPropInit[0].vValue.bstrVal= SysAllocString(g_wszServer);  
   rgdbPropInit[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   rgdbPropInit[0].colid = DB_NULLID;  
  
   if (rgdbPropInit[0].vValue.bstrVal == NULL)  
   {  
      hr = E_OUTOFMEMORY;  
      goto _ExitInitialize;  
   }  
  
   rgdbPropInit[1].dwPropertyID = DBPROP_INIT_CATALOG;  
   rgdbPropInit[1].vValue.vt = VT_BSTR;  
   rgdbPropInit[1].vValue.bstrVal= SysAllocString(g_wszCatalog);  
   rgdbPropInit[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   rgdbPropInit[1].colid = DB_NULLID;  
  
   if (rgdbPropInit[1].vValue.bstrVal == NULL)  
   {  
      hr = E_OUTOFMEMORY;  
      goto _ExitInitialize;  
   }  
  
   rgdbPropInit[2].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
   rgdbPropInit[2].vValue.vt = VT_BSTR;  
   rgdbPropInit[2].vValue.bstrVal= SysAllocString(g_wszSecurity);  
   rgdbPropInit[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   rgdbPropInit[2].colid = DB_NULLID;  
  
   if (rgdbPropInit[2].vValue.bstrVal == NULL)  
   {  
      hr = E_OUTOFMEMORY;  
      goto _ExitInitialize;  
   }  
  
   rgdbPropSetInit[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgdbPropSetInit[0].cProperties = NUM_DBINIT_PROPS;  
   rgdbPropSetInit[0].rgProperties = rgdbPropInit;  
  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);  
   CHKHR_GOTO(hr, L"Failed to QI DataSource object for IDBProperties.", _ExitInitialize);  
  
   hr = pIDBProperties->SetProperties(1, rgdbPropSetInit);   
   CHKHR_GOTO(hr, L"Failed to set DataSource object Properties.", _ExitInitialize);  
  
   pIDBProperties->Release();  
   pIDBProperties = NULL;  
  
   hr = pIDBInitialize->Initialize();  
   CHKHR_GOTO(hr, L"Failed to establish connection with the server.", _ExitInitialize);  
  
_ExitInitialize:  
  
   if (pIDBProperties)  
   {  
      pIDBProperties->Release();  
      pIDBProperties = NULL;  
   }  
  
   if (FAILED(hr))  
   {  
      if (pIDBInitialize)  
      {  
         pIDBInitialize->Release();  
         pIDBInitialize = NULL;  
      }  
   }  
  
   *ppIDBInitialize = pIDBInitialize;  
   return hr;  
}  
  
void UnInitializeConnection(IDBInitialize* pIDBInitialize)  
{  
   if (pIDBInitialize)  
   {  
      pIDBInitialize->Uninitialize();  
      pIDBInitialize->Release();  
      pIDBInitialize = NULL;  
   }  
   CoUninitialize();  
}  
  
HRESULT CreateAndSetCommand(IDBInitialize* pIDBInitialize, ICommandText** ppICommandText)  
{  
   HRESULT hr;  
   IDBCreateSession* pIDBCreateSession = NULL;  
   IDBCreateCommand* pIDBCreateCommand = NULL;  
   ICommandText* pICommandText = NULL;  
   ICommandProperties* pICommandProperties = NULL;  
   DBPROPSET rgCmdPropSet[1];  
   DBPROP rgCmdProperties[1];  
  
const wchar_t* const g_wCmdString = L"declare @x xml, @y nvarchar(max); select @x = (SELECT * FROM Sales.SalesOrderHeader FOR XML AUTO); select @x;";  
  
   *ppICommandText = NULL;  
  
   if (!pIDBInitialize)  
   {  
      hr = E_FAIL;  
      goto _ExitCreateAndSetCommand;  
   }  
  
   hr = pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void**) &pIDBCreateSession);  
   CHKHR_GOTO(hr, L"Failed to obtain IDBCreateSession interface from DSO.", _ExitCreateAndSetCommand);  
  
   hr = pIDBCreateSession->CreateSession(  
      NULL,   
      IID_IDBCreateCommand,   
      (IUnknown**) &pIDBCreateCommand);  
  
   CHKHR_GOTO(hr, L"Failed to Create a Session for command execution.", _ExitCreateAndSetCommand);  
  
   hr = pIDBCreateCommand->CreateCommand(  
      NULL,   
      IID_ICommandText,   
      (IUnknown**)&pICommandText);  
  
   CHKHR_GOTO(hr, L"Failed to Create a Command object.", _ExitCreateAndSetCommand);  
  
   hr = pICommandText->SetCommandText(DBGUID_DBSQL, g_wCmdString);  
   CHKHR_GOTO(hr, L"Failed to Set Command Text.", _ExitCreateAndSetCommand);  
  
   hr = pICommandText->QueryInterface(IID_ICommandProperties, (void**) &pICommandProperties);  
   CHKHR_GOTO(hr, L"Failed to obtain ICommandProperties interface from the command object.", _ExitCreateAndSetCommand);  
  
   rgCmdProperties[0].dwPropertyID = DBPROP_ACCESSORDER;  
   rgCmdProperties[0].vValue.vt = VT_I4;  
   rgCmdProperties[0].vValue.lVal = DBPROPVAL_AO_SEQUENTIAL;  
   rgCmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   rgCmdProperties[0].colid = DB_NULLID;  
  
   rgCmdPropSet[0].guidPropertySet = DBPROPSET_ROWSET;  
   rgCmdPropSet[0].cProperties = 1;  
   rgCmdPropSet[0].rgProperties = rgCmdProperties;  
  
   hr = pICommandProperties->SetProperties(1, rgCmdPropSet);   
   CHKHR_GOTO(hr, L"Failed to Set Command object Properties.", _ExitCreateAndSetCommand);  
  
_ExitCreateAndSetCommand:  
  
   if (pICommandProperties)  
   {  
      pICommandProperties->Release();  
      pICommandProperties = NULL;  
   }  
  
   if (pIDBCreateCommand)  
   {  
      pIDBCreateCommand->Release();  
      pIDBCreateCommand = NULL;  
   }  
  
   if (pIDBCreateSession)  
   {  
      pIDBCreateSession->Release();  
      pIDBCreateSession = NULL;  
   }  
  
   if (FAILED(hr))  
   {  
      if (pICommandText)  
      {  
         pICommandText->Release();  
         pICommandText = NULL;  
      }  
   }  
  
   *ppICommandText = pICommandText;  
   return hr;  
}  
  
HRESULT ProcessResultSet(IRowset* pIRowset)  
{  
   HRESULT hr;  
  
   IColumnsInfo* pIColumnsInfo = NULL;  
   DBCOLUMNINFO* pDBColumnInfo = NULL;  
   ULONG lNumCols = 0;  
   wchar_t* pStringsBuffer = NULL;  
  
   DBBINDING* pBindings = NULL;  
   DBOBJECT dbobj;  
   ULONG idxBinding;  
   IAccessor* pIAccessor = NULL;  
   HACCESSOR hAccessor = DB_NULL_HACCESSOR;  
   HROW hRows[1] = {DB_NULL_HROW};  
   HROW* pRow = &hRows[0];  
   BYTE* pBuffer = NULL;  
  
   ULONG lNumRowsRetrieved;  
   DBLENGTH dwOffset = 0;  
  
   hr = pIRowset->QueryInterface(IID_IColumnsInfo, (void **)&pIColumnsInfo);  
   CHKHR_GOTO(hr, L"Failed to QI Rowset for IColumnsInfo.", _ExitProcessResultSet);  
  
   hr = pIColumnsInfo->GetColumnInfo(&lNumCols, &pDBColumnInfo, &pStringsBuffer);  
   CHKHR_GOTO(hr, L"Failed to obtain Column Information.", _ExitProcessResultSet);  
  
   pBindings = new DBBINDING[lNumCols];  
  
   if (!pBindings)  
   {  
      hr = E_OUTOFMEMORY;  
      goto _ExitProcessResultSet;  
   }  
  
   memset(pBindings, 0, sizeof(DBBINDING) * lNumCols);  
  
   dbobj.dwFlags = STGM_READ;  
   dbobj.iid = IID_ISequentialStream;  
  
   for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)   
   {  
      pBindings[idxBinding].iOrdinal = idxBinding + 1;  
      pBindings[idxBinding].obStatus = dwOffset;  
      pBindings[idxBinding].obLength = dwOffset + sizeof(DBSTATUS);  
      pBindings[idxBinding].obValue = dwOffset + sizeof(DBSTATUS) + sizeof(DBLENGTH);  
  
      pBindings[idxBinding].pTypeInfo = NULL;  
      pBindings[idxBinding].pBindExt = NULL;  
      pBindings[idxBinding].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      pBindings[idxBinding].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      pBindings[idxBinding].eParamIO = DBPARAMIO_NOTPARAM;  
      pBindings[idxBinding].bPrecision = pDBColumnInfo[idxBinding].bPrecision;  
      pBindings[idxBinding].bScale = pDBColumnInfo[idxBinding].bScale;  
  
      pBindings[idxBinding].cbMaxLen = 0;  
      pBindings[idxBinding].wType = DBTYPE_WSTR;  
  
   // Determine the maximum number of bytes required in our buffer to  
   // contain the Unicode string representation of the provider's native  
   // data type, including room for the NULL-termination character  
   switch( pDBColumnInfo[idxBinding].wType )  
   {  
      case DBTYPE_NULL:  
      case DBTYPE_EMPTY:  
      case DBTYPE_I1:  
      case DBTYPE_I2:  
      case DBTYPE_I4:  
      case DBTYPE_UI1:  
      case DBTYPE_UI2:  
      case DBTYPE_UI4:  
      case DBTYPE_R4:  
      case DBTYPE_BOOL:  
      case DBTYPE_I8:  
      case DBTYPE_UI8:  
      case DBTYPE_R8:  
      case DBTYPE_CY:  
      case DBTYPE_ERROR:  
      // When the above types are converted to a string, they  
      // will all fit into 25 characters, so use that plus space  
      // for the NULL-terminator.  
  
      pBindings[idxBinding].cbMaxLen = (25 + 1) * sizeof(WCHAR);  
      break;  
  
      case DBTYPE_DECIMAL:  
      case DBTYPE_NUMERIC:  
      case DBTYPE_DATE:  
      case DBTYPE_DBDATE:  
      case DBTYPE_DBTIMESTAMP:  
      case DBTYPE_GUID:  
      // Converted to a string, the above types will all fit into  
      // 50 characters, so use that plus space for the terminator.  
  
      pBindings[idxBinding].cbMaxLen = (50 + 1) * sizeof(WCHAR);  
      break;  
  
      case DBTYPE_BYTES:  
      // In converting DBTYPE_BYTES to a string, each byte  
      // becomes two characters (e.g. 0xFF -> "FF"), so we  
      // will use double the maximum size of the column plus  
      // include space for the NULL-terminator.  
  
      pBindings[idxBinding].cbMaxLen = (pDBColumnInfo[idxBinding].ulColumnSize * 2 + 1) * sizeof(WCHAR);  
      break;  
  
      case DBTYPE_STR:  
      case DBTYPE_WSTR:  
      case DBTYPE_BSTR:  
      // Going from a string to our string representation,  
      // we can just take the maximum size of the column,  
      // a count of characters, and include space for the  
      // terminator, which is not included in the column size.  
  
      pBindings[idxBinding].cbMaxLen = (pDBColumnInfo[idxBinding].ulColumnSize + 1) * sizeof(WCHAR);  
      break;  
  
      default:  
      // For any other type, we will simply use our maximum  
      // column buffer size, since the display size of these  
      // columns may be variable (e.g. DBTYPE_VARIANT) or  
      // unknown (e.g. provider-specific types).  
      pBindings[idxBinding].cbMaxLen = MAX_COL_SIZE;  
      break;  
   }  
  
   // If the provider's native data type for this column is  
   // DBTYPE_IUNKNOWN or this is a BLOB column and the user  
   // has requested that we bind BLOB columns as ISequentialStream  
   // objects, bind this column as an ISequentialStream object if  
   // the provider supports our creating another ISequentialStream  
   // binding.  
   if(pDBColumnInfo[idxBinding].dwFlags & DBCOLUMNFLAGS_ISLONG)  
   {  
      pBindings[idxBinding].wType = DBTYPE_IUNKNOWN;  
  
      pBindings[idxBinding].cbMaxLen = sizeof(ISequentialStream*);  
  
      pBindings[idxBinding].pObject = (DBOBJECT *)CoTaskMemAlloc(sizeof(DBOBJECT));  
  
      if (!pBindings[idxBinding].pObject)  
      {  
         hr = E_OUTOFMEMORY;  
         goto _ExitProcessResultSet;  
      }  
  
      // Direct the provider to create an ISequentialStream  
      // object over the data for this column.  
      pBindings[idxBinding].pObject->iid = IID_ISequentialStream;  
  
      // We want read access on the ISequentialStream  
      // object that the provider will create for us  
      pBindings[idxBinding].pObject->dwFlags = STGM_READ;  
      }  
  
      // Ensure that the bound maximum length is no more than the  
      // maximum column size in bytes that we've defined.  
      pBindings[idxBinding].cbMaxLen = min(pBindings[idxBinding].cbMaxLen, MAX_COL_SIZE);  
  
      // Update the offset past the end of this column's data, so  
      // that the next column will begin in the correct place in  
      // the buffer.  
      dwOffset = pBindings[idxBinding].cbMaxLen + pBindings[idxBinding].obValue;  
  
      // Ensure that the data for the next column will be correctly  
      // aligned for all platforms, or, if we're done with columns,  
      // that if we allocate space for multiple rows that the data  
      // for every row is correctly aligned.  
      dwOffset = ROUNDUP(dwOffset);  
   }  
  
   hr = pIRowset->QueryInterface(IID_IAccessor, (void **) &pIAccessor);  
   CHKHR_GOTO(hr, L"Failed to obtain Accessor interface", _ExitProcessResultSet);  
  
   hr = pIAccessor->CreateAccessor(DBACCESSOR_ROWDATA,  
      lNumCols,  
      pBindings,  
      0,  
      &hAccessor,  
      NULL);  
  
   CHKHR_GOTO(hr, L"Failed to create Accessor", _ExitProcessResultSet);  
   for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)   
   {  
      cout << pDBColumnInfo[idxBinding].pwszName << endl;  
   }  
  
   lNumRowsRetrieved = 0;  
  
   hr = pIRowset->GetNextRows(  
      NULL,  
      0,  
      1,  
      &lNumRowsRetrieved,  
      &pRow);  
  
   CHKHR_GOTO(hr, L"Failed to fetch a row from the rowset", _ExitProcessResultSet);  
  
   pBuffer = new BYTE[sizeof(DBSTATUS) + sizeof(DBLENGTH) + sizeof(IUnknown*)];  
  
   if (!pBuffer)  
   {  
      hr = E_OUTOFMEMORY;  
      goto _ExitProcessResultSet;  
   }  
  
   while(lNumRowsRetrieved && hr != DB_S_ENDOFROWSET)   
   {  
      memset(pBuffer, 0, sizeof(DBSTATUS) + sizeof(DBLENGTH) + sizeof(IUnknown*));  
  
      hr = pIRowset->GetData(hRows[0], hAccessor, pBuffer);  
      CHKHR_GOTO(hr, L"Failed to obtain row data", _ExitProcessResultSet);  
  
      for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)  
      {  
         if (pBindings[idxBinding].wType == DBTYPE_IUNKNOWN)  
         {  
            BYTE pbBuff[3000];  
            ULONG cbNeeded = sizeof(pbBuff)/sizeof(BYTE);  
            ULONG cbRead;  
            ULONG cbReadTotal = 0;  
            ISequentialStream* pISequentialStream = NULL;  
  
            IUnknown* pIUnknown = *((IUnknown**)(pBuffer + pBindings[idxBinding].obValue));  
            pIUnknown->QueryInterface(IID_ISequentialStream, (void**)&pISequentialStream);  
  
            do  
            {  
               hr = pISequentialStream->Read(pbBuff, cbNeeded, &cbRead);  
               cbReadTotal += cbRead;  
            }  
            while (SUCCEEDED(hr) && hr != S_FALSE && cbRead == cbNeeded);  
  
               cout << "Total Bytes Read: " << cbReadTotal << endl;  
  
               pISequentialStream->Release();  
               pISequentialStream = NULL;  
               pIUnknown->Release();  
               pIUnknown = NULL;  
            }  
         }  
  
         pIRowset->ReleaseRows(1, pRow, NULL, NULL, NULL);  
  
         hr = pIRowset->GetNextRows(NULL,  
            0,  
            1,  
            &lNumRowsRetrieved,  
            &pRow);  
  
         CHKHR_GOTO(hr, L"Failed to fetch a row from the rowset.", _ExitProcessResultSet);  
   }  
  
_ExitProcessResultSet:  
  
   pIRowset->ReleaseRows(1, pRow, NULL, NULL, NULL);  
   delete [] pBuffer;  
  
   if (pIAccessor)  
   {  
      if (hAccessor != DB_NULL_HACCESSOR)  
      {  
         pIAccessor->ReleaseAccessor(hAccessor, NULL);  
      }  
  
      pIAccessor->Release();  
      pIAccessor = NULL;  
   }  
  
   if (pBindings)  
   {  
      for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)  
      {  
         if (pBindings[idxBinding].pObject)  
         CoTaskMemFree(pBindings[idxBinding].pObject);  
      }  
   }  
  
   delete [] pBindings;  
  
   CoTaskMemFree(pDBColumnInfo);  
   CoTaskMemFree(pStringsBuffer);  
  
   if (pIColumnsInfo)  
   {  
      pIColumnsInfo->Release();  
      pIColumnsInfo = NULL;  
   }  
  
   return hr;  
}  
```  
  
 Pour plus d’informations sur la façon dont le pilote OLE DB pour SQL Server expose les types de données de valeur élevée, consultez [BLOB et les objets OLE](../../oledb/ole-db-blobs/blobs-and-ole-objects.md).  

  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
  
  
