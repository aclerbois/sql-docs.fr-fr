---
title: Interface ADORecordsetConstruction | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- ADORecordsetConstruction
helpviewer_keywords:
- ADORecordsetConstruction interface [ADO]
ms.assetid: 08386eba-f1f7-4879-8ffd-8733930ecb2f
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5ff32133f7959b598d2c6bc7f2eb029906e66761
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adorecordsetconstruction-interface"></a>Interface ADORecordsetConstruction
Le **ADORecordsetConstruction** interface est utilisée pour construire un ADO **Recordset** objet OLE DB **ensemble de lignes** objet dans une application C/C++.  
  
 Cette interface prend en charge les propriétés suivantes :  
  
## <a name="properties"></a>Propriétés  
  
|||  
|-|-|  
|[chapitre](../../../ado/reference/ado-api/chapter-property-ado.md)|En lecture/écriture.<br />Obtient ou définit un OLE DB **chapitre** objet à partir de/sur cette ADO **Recordset** objet.|  
|[RowPosition](../../../ado/reference/ado-api/rowposition-property-ado.md)|En lecture/écriture.<br />Obtient ou définit un OLE DB **RowPosition** objet à partir de/sur cette ADO **Recordset** objet.|  
|[Rowset](../../../ado/reference/ado-api/rowset-property-ado.md)|En lecture/écriture.<br />Obtient ou définit un OLE DB **ensemble de lignes** objet à partir de/sur cette ADO **Recordset** objet.|  
  
## <a name="methods"></a>Méthodes  
 Aucun.  
  
## <a name="events"></a>Événements  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Étant donné un OLE DB **ensemble de lignes** objet (`pRowset`), la construction de ADO **Recordset** objet (`adoRs`) s’élève à trois opérations ci-après :  
  
1.  Créer un ADO **Recordset** objet :  
  
    ```  
    Recordset20Ptr adoRs;  
    adoRs.CreateInstance(__uuidof(Recordset));  
    ```  
  
2.  Requête le **IADORecordsetConstruction** de l’interface sur le **Recordset** objet :  
  
    ```  
    adoRecordsetConstructionPtr adoRsConstruct=NULL;  
    adoRs->QueryInterface(__uuidof(ADORecordsetConstruction),  
                         (void**)&adoRsConstruct);  
    ```  
  
3.  Appelez le `IADORecordsetConstruction::put_Rowset` méthode propriété OLE DB `Rowset` objet ADO `Recordset` objet :  
  
    ```  
    IUnknown *pUnk=NULL;  
    pRowset->QueryInterface(IID_IUnknown, (void**)&pUnk);  
    adoRsConstruct->put_Rowset(pUnk);  
    ```  
  
 Résultant `adoRs` objet représente maintenant l’ADO **Recordset** objet construit à partir d’OLE DB **ensemble de lignes** objet.  
  
 Vous pouvez également construire ADO **Recordset** objet OLE DB **chapitre** ou **RowPosition** objet.  
  
## <a name="requirements"></a>Spécifications  
 **Version :** ADO 2.0 et versions ultérieur  
  
 **Bibliothèque :** msado15.dll  
  
 **UUID :** 00000283-0000-0010-8000-00AA006D2EA4  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Rowset, propriété (ADO)](../../../ado/reference/ado-api/rowset-property-ado.md)
