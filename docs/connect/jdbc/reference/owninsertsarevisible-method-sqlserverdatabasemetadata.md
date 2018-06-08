---
title: Méthode ownInsertsAreVisible (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.ownInsertsAreVisible
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fe76aa3-a539-4335-822f-69cc35a9e7e0
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d5710bffa0305a7e3b282ef72e9a33f94c7a0cda
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="owninsertsarevisible-method-sqlserverdatabasemetadata"></a>Méthode ownInsertsAreVisible (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si les propres insertions d'un jeu de résultats sont visibles.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean ownInsertsAreVisible(int type)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *type*  
  
 Un **int** qui indique le jeu de résultats type, qui peut prendre l’une des valeurs suivantes comme défini dans java.sql.ResultSet ou SQLServerResultSet :  
  
## <a name="javasqlresultset-types"></a>Types java.sql.ResultSet  
 TYPE_FORWARD_ONLY  
  
 TYPE_SCROLL_SENSITIVE  
  
 TYPE_SCROLL_INSENSITIVE  
  
## <a name="sqlserverresultset-types"></a>Types SQLServerResultSet  
 TYPE_SS_SCROLL_STATIC  
  
 TYPE_SS_SCROLL_KEYSET  
  
 TYPE_SS_DIRECT_FORWARD_ONLY  
  
 TYPE_SS_SERVER_CURSOR_FORWARD_ONLY  
  
 TYPE_SS_SCROLL_DYNAMIC  
  
## <a name="return-value"></a>Valeur retournée  
 **true** si les insertions sont visibles. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode ownInsertsAreVisible est spécifiée par la méthode ownInsertsAreVisible dans l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
