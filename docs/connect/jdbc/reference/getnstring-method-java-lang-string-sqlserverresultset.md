---
title: "Méthode getNString (java.lang.String) (SQLServerResultSet) | Documents Microsoft"
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
ms.assetid: 546d77e2-723a-42ac-ba3f-fabf2395d376
caps.latest.revision: "15"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 94384f77db527050a820f7d6d02202c534d269cd
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="getnstring-method-javalangstring-sqlserverresultset"></a>Méthode getNString (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de la colonne désignée dans la ligne actuelle de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant qu’objet java.lang.String.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getNString(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnLabel*  
  
 Chaîne qui contient l’étiquette de colonne.  
  
## <a name="return-value"></a>Valeur retournée  
 Un objet String.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getNString est spécifiée par la méthode getNString dans l’interface java.sql.SQLServerResultSet.  
  
 Cette méthode peut être utilisée pour récupérer la valeur d’un **nvarchar**, **nchar**, **nvarchar (max)**, **ntext**, ou **xml** colonne dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet. Si vous essayez d'utiliser cette méthode pour récupérer les valeurs d'autres types de données, une exception est levée.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getNString &#40; SQLServerResultSet &#41;](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
