---
title: IRowsetFastLoad (OLE DB) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- IRowsetFastLoad interface
ms.assetid: d19a7097-48d9-409a-aff9-277891b7aca7
caps.latest.revision: 35
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d747a4892940effe8b5b21a7b9ceddbde1ff71aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="irowsetfastload-ole-db"></a>IRowsetFastLoad (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le **IRowsetFastLoad** interface expose la prise en charge de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] basé sur mémoire des opérations de copie en bloc. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Consommateurs du fournisseur OLE DB du Client natifs utilisent l’interface pour ajouter rapidement des données à un fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table.  
  
 Si vous affectez la valeur VARIANT_TRUE à SSPROP_ENABLEFASTLOAD pour une session, vous ne pouvez pas lire les données des ensembles de lignes retournés ultérieurement à partir de cette session. Lorsque SSPROP_ENABLEFASTLOAD a la valeur VARIANT_TRUE, tous les ensembles de lignes créés sur la session sera de type IRowsetFastLoad. Ensembles de lignes iRowsetFastLoad ne prennent pas en charge les fonctionnalité d’extraction de lignes ; Par conséquent, les données à partir de ces ensembles de lignes ne peuvent pas être lues.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Méthode| Description|  
|------------|-----------------|  
|[IRowsetFastLoad::Commit &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-commit-ole-db.md)|Marque la fin d'un lot de lignes insérées et écrit les lignes dans la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[IRowsetFastLoad::InsertRow &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-insertrow-ole-db.md)|Ajoute une ligne à l'ensemble de lignes de copie en bloc.|  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces & #40 ; OLE DB & #41 ;](http://msdn.microsoft.com/library/34c33364-8538-45db-ae41-5654481cda93)   
 [Copie de données à l’aide de IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md)   
 [Envoyer des données BLOB à SQL SERVER en utilisant IROWSETFASTLOAD et ISEQUENTIALSTREAM & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)  
  
  
