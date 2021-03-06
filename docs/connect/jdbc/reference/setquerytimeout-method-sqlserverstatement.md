---
title: "setQueryTimeout (méthode) (SQLServerStatement) | Documents Microsoft"
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
apiname: SQLServerStatement.setQueryTimeout
apilocation: sqljdbc.jar
apitype: Assembly
ms.assetid: 0c513265-cd0c-4b38-9494-94458c17a16d
caps.latest.revision: "8"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 3b9d48ed5202a55c8713811ee9746805d84ef92c
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="setquerytimeout-method-sqlserverstatement"></a>setQueryTimeout (méthode) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le nombre de secondes pendant lesquelles le pilote doit attendre un [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à exécuter pour le nombre de secondes donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setQueryTimeout(int seconds)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *secondes*  
  
 Un **int** qui indique le nombre de secondes à attendre, ou 0 s’il n’existe aucune limite.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setQueryTimeout est spécifiée par la méthode setQueryTimeout dans l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
