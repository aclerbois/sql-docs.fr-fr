---
title: "setAutoCommit (méthode) (SQLServerConnection) | Documents Microsoft"
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
apiname: SQLServerConnection.setAutoCommit
apilocation: sqljdbc.jar
apitype: Assembly
ms.assetid: db1e22d2-e53f-474e-8c99-cb1fff7f491a
caps.latest.revision: "13"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: c59314ab0ecc4260442f13206e327f91a7ea8b59
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="setautocommit-method-sqlserverconnection"></a>setAutoCommit (méthode) (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le mode de validation automatique pour cet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet à l’état donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setAutoCommit(boolean value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *valeur*  
  
 **true** pour activer le mode de validation automatique pour la connexion, **false** pour la désactiver.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setAutoCommit est spécifiée par la méthode setAutoCommit dans l’interface java.sql.Connection.  
  
 Si une connexion est en mode de validation automatique, toutes ses instructions SQL sont exécutées et validées en tant que transactions individuelles. Dans le cas contraire, ses instructions SQL sont regroupées dans des transactions qui sont terminent par un appel à la [validation](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md) méthode ou la [rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md) (méthode). Par défaut, les nouvelles connexions sont en mode de validation automatique.  
  
 La validation se produit à la fin de l'instruction ou à l'exécution suivante, selon l'opération qui se produit en premier. Dans le cas d’instructions qui retournent un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet, l’instruction se termine lorsque la dernière ligne du jeu de résultats a été récupérée, ou lorsque le jeu de résultats a été fermé. Dans les cas plus avancés, une instruction unique peut retourner plusieurs résultats en plus des valeurs de paramètre de sortie. Dans ces cas, la validation se produit lorsque tous les résultats et valeurs de paramètre de sortie ont été récupérés.  
  
 Lorsque le mode de validation automatique est **false**, le pilote JDBC implicitement démarre une nouvelle transaction après chaque validation.  
  
> [!NOTE]  
>  Si cette méthode est appelée au cours d'une transaction, la transaction est validée.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection, classe](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
