---
title: Détail de l’erreur SQL Server | Documents Microsoft
description: Détail de l’erreur SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-errors
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], error details
- IErrorRecords interface
- IErrorInfo interface
- OLE DB error handling, error details
- ISQLServerErrorInfo interface
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: f271da81733649c94f01b06281f7463c8ac719b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-error-detail"></a>Détail des erreurs SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le pilote OLE DB pour SQL Server définit l’interface d’erreur spécifique au fournisseur [ISQLServerErrorInfo](http://msdn.microsoft.com/library/a8323b5c-686a-4235-a8d2-bda43617b3a1). L'interface retourne davantage de détails sur une erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et s'avère utile en cas d'échec de l'exécution d'une commande ou d'opérations d'ensemble de lignes.  
  
 Il existe deux façons d’obtenir l’accès à **ISQLServerErrorInfo** interface.  
  
 Le consommateur peut appeler **IErrorRecords::GetCustomerErrorObject** pour obtenir un **ISQLServerErrorInfo** pointeur, comme indiqué dans l’exemple de code suivant. (Il est inutile d’obtenir **ISQLErrorInfo.**) Les deux **ISQLErrorInfo** et **ISQLServerErrorInfo** sont des objets d’erreur OLE DB personnalisés, **ISQLServerErrorInfo** en cours de l’interface à utiliser pour obtenir des informations d’erreurs de serveur, notamment des détails sur les numéros de ligne et de nom de procédure.  
  
```  
// Get the SQL Server custom error object.  
if(FAILED(hr=pIErrorRecords->GetCustomErrorObject(  
   nRec, IID_ISQLServerErrorInfo,  
   (IUnknown**)&pISQLServerErrorErrorInfo)))  
```  
  
 Une autre méthode pour obtenir un **ISQLServerErrorInfo** pointeur consiste à appeler la **QueryInterface** méthode sur une déjà obtenu **ISQLErrorInfo** pointeur. Notez que puisque **ISQLServerErrorInfo** contient un surensemble des informations disponibles à partir de **ISQLErrorInfo**, il est judicieux d’accéder directement à **ISQLServerErrorInfo** via **GetCustomerErrorObject**.  
  
 Le **ISQLServerErrorInfo** interface expose une fonction membre, [ISQLServerErrorInfo::GetErrorInfo](../../oledb/ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). La fonction retourne un pointeur à une structure SSERRORINFO et un pointeur à une mémoire tampon de chaîne. Les deux pointeurs font référence mémoire, le consommateur doit libérer à l’aide de la **IMalloc::Free** (méthode).  
  
 Les membres de la structure SSERRORINFO sont interprétés par le consommateur comme suit.  
  
|Membre| Description|  
|------------|-----------------|  
|*pwszMessage*|Message d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Identique à la chaîne retournée dans **IErrorInfo::GetDescription**.|  
|*pwszServer*|Nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour la session.|  
|*pwszProcedure*|S'il y a lieu, nom de la procédure d'où provient l'erreur. Sinon, une chaîne vide.|  
|*lNative*|Numéro d'erreur natif [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Identique à la valeur retournée dans le *plNativeError* paramètre de **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|État d'un message d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravité d'un message d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|S'il y a lieu, numéro de ligne d'une procédure stockée sur laquelle s'est produite l'erreur.|  
  
## <a name="see-also"></a>Voir aussi  
 [Erreurs](../../oledb/ole-db-errors/errors.md)   
 [RAISERROR & #40 ; Transact-SQL & #41 ;](../../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
