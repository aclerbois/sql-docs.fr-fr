---
title: "Commit (méthode) (SQLServerConnection) | Documents Microsoft"
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
apiname: SQLServerConnection.commit
apilocation: sqljdbc.jar
apitype: Assembly
ms.assetid: c7346165-51bf-4844-b64c-29833c147236
caps.latest.revision: "11"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 8d99a568ebff4f97fead61019430b5f5c176deef
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="commit-method-sqlserverconnection"></a>Commit (méthode) (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rend toutes les modifications apportées depuis le précédent commit ou rollback permanentes et libère tous les verrous de base de données actuellement maintenus par cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void commit()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode de validation est spécifiée par la méthode de validation dans l’interface java.sql.Connection.  
  
 Cette méthode doit être utilisée uniquement lorsque le mode de validation automatique a été désactivé.  
  
 Notez que cette méthode échoue et lève une exception si le client démarre une transaction manuelle, et si, pour une quelconque raison, SQL Server restaure ensuite cette transaction manuelle. Par exemple, une exception est levée si le client appelle une procédure stockée qui appelle explicitement ROLLBACK TRANSACTION, et le client appelle ensuite la méthode de validation. En outre, si SQL Server génère une erreur de gravité suffisante (16 ou plue) pour restaurer le client transaction manuelle initiée par ; un appel ultérieur à la méthode de validation lève une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection, classe](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
