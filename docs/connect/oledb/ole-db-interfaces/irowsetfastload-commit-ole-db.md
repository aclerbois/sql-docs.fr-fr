---
title: IRowsetFastLoad::Commit (OLE DB) | Microsoft Docs
description: IRowsetFastLoad::Commit (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: ''
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- IRowsetFastLoad::Commit (OLE DB)
apitype: COM
helpviewer_keywords:
- Commit method
author: pmasl
ms.author: Pedro.Lopes
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 93e3b1a7bf97da890e77e3c51d6c1f5bc6f67ee0
ms.sourcegitcommit: 9f4330a4b067deea396b8567747a6771f35e6eee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="irowsetfastloadcommit-ole-db"></a>IRowsetFastLoad::Commit (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Marque la fin d'un lot de lignes insérées et écrit les lignes dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour obtenir des exemples, consultez [en bloc copie de données en bloc avec IRowsetFastLoad &#40;OLE DB&#41; ](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) et [envoyer les données BLOB à SQL SERVER à l’aide de IROWSETFASTLOAD et ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT Commit(  
      BOOL fDone);  
```  
  
## <a name="arguments"></a>Arguments  
 *fDone*[in]  
 Si FALSE, l'ensemble de lignes conserve la validation et peut être utilisé par le consommateur pour l'insertion de ligne supplémentaire. Si TRUE, l'ensemble de lignes perd la validation et aucune insertion supplémentaire ne peut être effectuée par le consommateur.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Cette méthode signale les erreurs en attribuant à la propriété Nombre de l'objet Err global l'une des valeurs du tableau suivant.  
 La méthode a réussi et toutes les données insérées ont été écrites dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s'est produite. Extrayez les informations sur l'erreur pour le texte d'erreur spécifique à partir du fournisseur.  
  
 E_UNEXPECTED  
 La méthode a été appelée sur un ensemble de lignes de copie en bloc précédemment invalidé par le **IRowsetFastLoad::Commit** (méthode).  
  
## <a name="remarks"></a>Notes  
 Un pilote OLE DB pour SQL Server de lignes de copie en bloc se comporte comme un ensemble de lignes en mode de mise à jour différée. Comme l’utilisateur insère des données de ligne via l’ensemble de lignes, les lignes insérées sont traitées de la même manière qu’insertions en attente sur un ensemble de lignes prenant en charge **IRowsetUpdate**.  
  
 Le consommateur doit appeler le **valider** méthode sur l’ensemble de lignes de copie en bloc pour écrire des lignes insérées dans la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table dans la même façon que les **IRowsetUpdate::Update** méthode est utilisée pour envoyer des lignes en attente à une instance de SQL Server.  
  
 Si le consommateur libère sa référence à l’ensemble de lignes de copie en bloc sans appeler le **valider** méthode insérée de toutes les lignes non écrites précédemment sont perdues.  
  
 Le consommateur peut traiter par lot des lignes insérées en appelant le **valider** méthode avec la *fDone* argument a la valeur FALSE. Lorsque *fDone*est définie sur TRUE, l’ensemble de lignes devient non valide. Un ensemble de lignes de copie en bloc non valide prend uniquement en charge la **ISupportErrorInfo** interface et **IRowsetFastLoad::Release** (méthode).  
  
## <a name="see-also"></a>Voir aussi  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  
