---
title: "Actualiser les procédures, méthode-exemple (VB) | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: cf65ae1cc8e18e20d23d521fd4f7fc01f6c08b01
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="procedures-refresh-method-example-vb"></a>Actualiser les procédures, méthode-exemple (VB)
Le code suivant montre comment actualiser le [procédures](../../../ado/reference/adox-api/procedures-collection-adox.md) collection d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md). Cela est nécessaire avant [procédure](../../../ado/reference/adox-api/procedure-object-adox.md) des objets de la **catalogue** sont accessibles.  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Objet catalogue (ADOX)](../../../ado/reference/adox-api/catalog-object-adox.md)   
 [Collection de procédures (ADOX)](../../../ado/reference/adox-api/procedures-collection-adox.md)   
 [Refresh, méthode (ADO)](../../../ado/reference/ado-api/refresh-method-ado.md)
