---
title: "Méthode setTime aux valeurs d’heure et de calendrier | Documents Microsoft"
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
apiname: SQLServerCallableStatement.setTime (java.lang.String, java.lang.Time, java.lang.Calendar))
apilocation: sqljdbc.jar
apitype: Assembly
ms.assetid: ca08fea8-ee1a-49e4-a973-2923d325df79
caps.latest.revision: "19"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 3b97a045544be6ad677430fa5c537a8268a31993
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="settime-method-javalangstring-javasqltime-javautilcalendar"></a>Méthode setTime (java.lang.String, java.sql.Time, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le paramètre désigné selon les valeurs d'heure et de calendrier spécifiées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setTime(java.lang.String sCol,  
                    java.sql.Time x,  
                    java.util.Calendar c)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sCol*  
  
 A **chaîne** qui contient le nom du paramètre.  
  
 *x*  
  
 Un objet de l’heure.  
  
 *c*  
  
 Un objet de calendrier.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setTime est spécifiée par la méthode setTime dans l’interface java.sql.CallableStatement.  
  
 À partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] version 3.0 du pilote JDBC, le comportement de cette méthode est modifié par la **sendTimeAsDatetime** propriété de connexion ([définissant les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md)) et [ SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Pour plus d’informations, consultez [java.sql.Time configurer comment les valeurs sont envoyées au serveur](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode setTime &#40; SQLServerCallableStatement &#41;](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)   
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement, classe](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
