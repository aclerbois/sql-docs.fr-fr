---
title: Méthode getSQLStateType (SQLServerDatabaseMetaData) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.getSQLStateType
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ee4d6751-68a3-4d04-831c-e6d704c59e63
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6d5778b2b42af466ced101633a38ac9d0db83791
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getsqlstatetype-method-sqlserverdatabasemetadata"></a>Méthode getSQLStateType (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indique si le SQLSTATE retourné par la méthode SQLException.getSQLState est X/Open (maintenant appelé Open Group), SQL CLI, SQL99 (JDBC 3.0) ou SQL:2003 (JDBC 4.0).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int getSQLStateType()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Un **int** qui indique le type de SQLSTATE, qui peut être une des valeurs suivantes :  
  
-   Pour Java Runtime Environment version 5.0 : si le **xopenStates** a la valeur de propriété de connexion **true**, cette méthode retourne DatabaseMetaData.sqlStateXOpen. Dans le cas contraire, DatabaseMetaData.sqlStateSQL99.  
  
-   Pour Java Runtime Environment version 6.0 : si le **xopenStates** a la valeur de propriété de connexion **true**, cette méthode retourne DatabaseMetaData.sqlStateXOpen. Dans le cas contraire, DatabaseMetaData.sqlStateSQL.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getSQLStateType est spécifiée par la méthode getSQLStateType dans l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
