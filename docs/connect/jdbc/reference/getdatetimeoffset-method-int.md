---
title: "Méthode getDateTimeOffset (int) | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: jdbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 8bb00356-4d6e-4625-b924-67646930fdf2
caps.latest.revision: "15"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 019000c882372e7a80ead2761d2e207738a25f72
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="getdatetimeoffset-method-int"></a>Méthode getDateTimeOffset (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cette méthode a été ajoutée dans [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] version 3.0 du pilote JDBC.  
  
 Récupère la valeur du paramètre désigné en tant qu’un [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) objet en fonction de l’index de paramètre de langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public microsoft.sql.DateTimeOffset getDateTimeOffset(int index)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *index*  
  
 Position ordinale du paramètre de base un.  
  
## <a name="return-value"></a>Valeur retournée  
 A [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Vous pouvez définir un [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) valeur de paramètre avec [SQLServerCallableStatement.setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getDateTimeOffset &#40; SQLServerCallableStatement &#41;](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)   
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement, classe](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
