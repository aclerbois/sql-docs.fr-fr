---
title: Wrappers de classe Java ADO | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- class wrappers [ADO]
ms.assetid: 1fc09dc1-9e32-412e-9f43-b8eb8bb483ca
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 40c9943eb1004dd612e46a144ec50e6753181be7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-java-class-wrappers"></a>Wrappers de classe Java ADO
Ce code déclare une instance de la ADO [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) wrapper de classe et initialise le tout sur la même ligne de code. En outre, il déclare des variables pour chacun des arguments dans le [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) (méthode), en particulier pour [LockType](../../../ado/reference/ado-api/locktype-property-ado.md) et [CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md) (Java ne gère pas énumérées types). Il ouvre et ferme le **Recordset** objet. Définissant simplement Rs1 NULL planifie cette variable doit être libéré lorsque Java effectue sa version systématique et intermittente des objets inutilisés.  
  
```  
public static void main( String args[])  
{  
   msado15._Recordset   Rs1 = new msado15.Recordset();  
   Variant Source     = new Variant( "SELECT * FROM Authors" );  
   Variant Connect    = new Variant( "DSN=AdoDemo;UID=admin;PWD=;" );  
   int     LockType   = msado15.CursorTypeEnum.adOpenForwardOnly;  
   int     CursorType = msado15.LockTypeEnum.adLockReadOnly;  
   int     Options    = -1;  
  
   Rs1.Open( Source, Connect, LockType,  CursorType, Options );  
   Rs1.Close();  
   Rs1 = null;  
  
   System.out.println( "Success!\n" );  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation du SDK Microsoft pour Java](../../../ado/guide/appendixes/using-the-microsoft-sdk-for-java.md)
