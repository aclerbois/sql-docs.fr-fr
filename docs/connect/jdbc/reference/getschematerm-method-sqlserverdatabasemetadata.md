---
title: Méthode getSchemaTerm (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getSchemaTerm
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3e4a400f-0859-4ac3-983e-c25633b33683
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 12b93016639c6f151e5ad490bb363fa9344e97b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getschematerm-method-sqlserverdatabasemetadata"></a>Méthode getSchemaTerm (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère le terme favori pour « schema » dans cette base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getSchemaTerm()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 A **chaîne** qui contient le terme favori.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getSchemaTerm est spécifiée par la méthode getSchemaTerm dans l’interface java.sql.DatabaseMetaData.  
  
 Lorsque vous utilisez la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] avec un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données, cette méthode retourne « schema » comme terme favori.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
