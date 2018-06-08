---
title: Prise en charge les Transactions locales | Documents Microsoft
description: Transactions locales dans le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-transactions
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- OLE DB Driver for SQL Server, transactions
- local transactions [OLE DB]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: de00c4aac3125209bb56a1867f07b1f395804cc8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supporting-local-transactions"></a>Prise en charge des transactions locales
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Une session délimite l’étendue de transaction pour un pilote OLE DB pour la transaction locale de SQL Server. Lorsque, à l’initiative d’un consommateur, le pilote OLE DB pour SQL Server soumet une demande à une instance connectée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la demande constitue une unité de travail pour le pilote OLE DB pour SQL Server. Transactions locales encapsulent toujours une ou plusieurs unités de travail sur un pilote OLE DB unique pour la session SQL Server.  
  
 À l’aide de la valeur par défaut du pilote OLE DB pour le mode de validation automatique de SQL Server, une seule unité de travail est traitée comme étendue d’une transaction locale. Une seule unité participe à la transaction locale. Lorsqu’une session est créée, le pilote OLE DB pour SQL Server commence une transaction pour la session. Une fois que l'unité de travail a été exécutée avec succès, le travail est validé. En cas d'échec, tout travail commencé est annulé et l'erreur est signalée au consommateur. Dans les deux cas, le pilote OLE DB pour SQL Server commence une nouvelle transaction locale pour la session afin que tout le travail est effectué au sein d’une transaction.  
  
 Le pilote OLE DB pour le consommateur de SQL Server peut diriger un contrôle plus précis sur l’étendue de transaction locale à l’aide de la **ITransactionLocal** interface. Lorsqu’une session de consommateur lance une transaction, toutes les unités de travail de session entre la transaction démarrer point et l’éventuelle **valider** ou **abandonner** les appels de méthode sont traités comme une unité atomique. Le pilote OLE DB pour SQL Server commence implicitement une transaction lorsque vous y êtes invité par le consommateur. Si le consommateur ne demande pas la rétention, la session revient au comportement parent du niveau de la transaction, soit le plus généralement le mode de validation automatique.  
  
 Le pilote OLE DB pour SQL Server prend en charge **ITransactionLocal::StartTransaction** paramètres comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*isoLevel*[in]|Niveau d'isolation à utiliser avec cette transaction. Dans les transactions locales, le pilote OLE DB pour SQL Server prend en charge les éléments suivants :<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE ATTRIBUÉE AU PARAMÈTRE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> Remarque : À partir de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ISOLATIONLEVEL_SNAPSHOT est valide pour le *isoLevel* argument ou non le contrôle de version est activé pour la base de données. Cependant, une erreur se produit si l'utilisateur essaie d'exécuter une instruction et que le suivi des versions n'est pas activé et/ou que la base de données n'est pas en lecture seule. En outre, l’erreur XACT_E_ISOLATIONLEVEL se produit si ISOLATIONLEVEL_SNAPSHOT est spécifié comme le *isoLevel* lorsqu’il est connecté à une version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].|  
|*indicateur isoFlags*[in]|Le pilote OLE DB pour SQL Server retourne une erreur pour toute valeur différente de zéro.|  
|*pOtherOptions*[in]|Si la valeur n’est pas NULL, le pilote OLE DB pour SQL Server demande l’objet d’options de l’interface. Le pilote OLE DB pour SQL Server retourne XACT_E_NOTIMEOUT si l’objet d’options *ulTimeout* membre n’est pas zéro. Le pilote OLE DB pour SQL Server ignore la valeur de la *szDescription* membre.|  
|*pulTransactionLevel*[out]|Si la valeur n’est pas NULL, le pilote OLE DB pour SQL Server retourne le niveau imbriqué de la transaction.|  
  
 Pour les transactions locales, le pilote OLE DB pour SQL Server implémente **ITransaction::Abort** paramètres comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*pboidReason*[in]|Ignoré s'il est défini. Peut être égal à NULL en toute sécurité.|  
|*fRetaining*[in]|Lorsque la valeur est TRUE, une nouvelle transaction est commencée implicitement pour la session. La transaction doit être validée ou terminée par le consommateur. Si la valeur est FALSE, le pilote OLE DB pour SQL Server revient en mode autocommit pour la session.|  
|*fAsync*[in]|Abandon asynchrone n’est pas pris en charge par le pilote OLE DB pour SQL Server. Le pilote OLE DB pour SQL Server retourne XACT_E_NOTSUPPORTED si la valeur n’est pas FALSE.|  
  
 Pour les transactions locales, le pilote OLE DB pour SQL Server implémente **ITransaction::Commit** paramètres comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*fRetaining*[in]|Lorsque la valeur est TRUE, une nouvelle transaction est commencée implicitement pour la session. La transaction doit être validée ou terminée par le consommateur. Si la valeur est FALSE, le pilote OLE DB pour SQL Server revient en mode autocommit pour la session.|  
|*grfTC*[in]|Asynchrone et phase un retourne ne sont pas pris en charge par le pilote OLE DB pour SQL Server. Le pilote OLE DB pour SQL Server retourne XACT_E_NOTSUPPORTED pour toute valeur autre que XACTTC_SYNC.|  
|*grfRM*[in]|Doit être égal à 0.|  
  
 Le pilote OLE DB pour SQL Server, ensembles de lignes sur la session sont conservés lors d’une validation locale ou d’abandonner l’opération selon les valeurs des propriétés de l’ensemble de lignes DBPROP_ABORTPRESERVE et DBPROP_COMMITPRESERVE. Par défaut, ces propriétés sont égales à VARIANT_FALSE et tous les pilote OLE DB pour SQL Server, les ensembles de lignes sur la session sont perdues après un abandon ou valider l’opération.  
  
 Le pilote OLE DB pour SQL Server n’implémente pas le **ITransactionObject** interface. La tentative d'un consommateur pour extraire une référence sur l'interface retourne E_NOINTERFACE.  
  
 Cet exemple utilise **ITransactionLocal**.  
  
```  
// Interfaces used in the example.  
IDBCreateSession*   pIDBCreateSession   = NULL;  
ITransaction*       pITransaction       = NULL;  
IDBCreateCommand*   pIDBCreateCommand   = NULL;  
IRowset*            pIRowset            = NULL;  
  
HRESULT             hr;  
  
// Get the command creation and local transaction interfaces for the  
// session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
if (FAILED(hr = pIDBCreateCommand->QueryInterface(IID_ITransactionLocal,  
    (void**) &pITransaction)))  
    {  
    // Process error. Release any references and return.  
    }  
  
// Start the local transaction.  
if (FAILED(hr = ((ITransactionLocal*) pITransaction)->StartTransaction(  
    ISOLATIONLEVEL_REPEATABLEREAD, 0, NULL, NULL)))  
    {  
    // Process error from StartTransaction. Release any references and  
    // return.  
    }  
  
// Get data into a rowset, then update the data. Functions are not  
// illustrated in this example.  
if (FAILED(hr = ExecuteCommand(pIDBCreateCommand, &pIRowset)))  
    {  
    // Release any references and return.  
    }  
  
// If rowset data update fails, then terminate the transaction, else  
// commit. The example doesn't retain the rowset.  
if (FAILED(hr = UpdateDataInRowset(pIRowset, bDelayedUpdate)))  
    {  
    // Get error from update, then terminate.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, XACTTC_SYNC, 0)))  
        {  
        // Get error from failed commit.  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions](../../oledb/ole-db-transactions/transactions.md)   
 [Utilisation de l’isolement d’instantané](../../oledb/features/working-with-snapshot-isolation.md)  
  
  
