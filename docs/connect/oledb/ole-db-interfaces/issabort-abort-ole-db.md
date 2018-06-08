---
title: ISSAbort::Abort (OLE DB) | Documents Microsoft
description: ISSAbort::Abort (OLE DB)
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
- ISSAbort::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 444d51aeae49e9e626b0666904584bae8e2de6b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="issabortabort-ole-db"></a>ISSAbort::Abort (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Annule l'ensemble de lignes actuel plus toutes les commandes par lot associées à la commande actuelle.  
  
Le **ISSAbort** interface, exposée dans le pilote OLE DB pour SQL Server, fournit le **ISSAbort::Abort** méthode qui est utilisée pour annuler l’ensemble de lignes actuel plus toutes les commandes regroupées par lot avec la commande ayant généré initialement l’ensemble de lignes, et qui n’ont pas terminé l’exécution.  
  
 **ISSAbort** est un pilote OLE DB pour l’interface de spécifiques à SQL Server disponible à l’aide de **QueryInterface** sur la **IMultipleResults** objet retourné par **ICommand::Execute**  ou **IOpenRowset::OpenRowset**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT Abort(void);  
```  
  
## <a name="remarks"></a>Notes  
 Si la commande en cours d’abandon est dans une procédure stockée, l’exécution de la procédure stockée (et toutes les procédures ayant appelé cette procédure) sera terminée, ainsi que le lot de commandes qui contient l’appel de procédure stockée. Si le serveur est en train de transférer un jeu de résultats pour le client, le transfert sera arrêté. Si le client ne souhaite pas consommer un jeu de résultats, l'appel de la méthode **ISSAbort::Abort** avant la diffusion de l'ensemble de lignes permettra d'accélérer cette dernière. En revanche, si une transaction est ouverte et si XACT_ABORT est défini sur ON (c'est-à-dire activé), la transaction sera restaurée lors de l'appel de la méthode **ISSAbort::Abort** .  
  
 Après avoir **ISSAbort::Abort** retourne S_OK, associé **IMultipleResults** interface entre dans un état inutilisable et retourne DB_E_CANCELED à tous les appels de méthode (à l’exception des méthodes définies par le **IUnknown** interface) jusqu'à ce qu’il est libéré. Si une interface **IRowset** a été obtenue à partir de l'interface **IMultipleResults** avant un appel à **à Abort**, elle adopte également un état inutilisable et retourne DB_E_CANCELED à tous les appels de méthode (sauf pour les méthodes définies par l'interface **IUnknown** et **IRowset::ReleaseRows**) jusqu'à ce qu'elle soit diffusée après l'appel en bonne et due forme de la méthode **ISSAbort::Abort**.  
  
> [!NOTE]  
>  En commençant par [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], si l'état XACT_ABORT du serveur est défini sur ON, l'exécution de la méthode **ISSAbort::Abort** prendra fin et restaurera toutes les transactions implicites ou explicites actuelles lors de la connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'abandonneront pas la transaction actuelle.  
  
## <a name="arguments"></a>Arguments  
 Aucun.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Cette méthode signale les erreurs en attribuant à la propriété Nombre de l'objet Err global l'une des valeurs du tableau suivant.  
 La méthode **ISSAbort::Abort** retourne S_OK si le lot a été annulé ou bien DB_E_CANTCANCEL dans le cas inverse. Si le lot a déjà été annulé, DB_E_CANCELED est retourné.  
  
 DB_E_CANCELED  
 Le lot a déjà été annulé.  
  
 DB_E_CANTCANCEL  
 Le lot n'a pas été annulé.  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s’est produite ; Pour plus d’informations, utilisez le [ISQLServerErrorInfo](http://msdn.microsoft.com/library/a8323b5c-686a-4235-a8d2-bda43617b3a1) interface.  
  
 E_UNEXPECTED  
 L'appel à la méthode était inattendu. Par exemple, l'objet est dans un état zombie parce que la méthode **ISSAbort::Abort** a déjà été appelée.  
  
 E_OUTOFMEMORY  
 Erreur de mémoire insuffisante.  
  
  
