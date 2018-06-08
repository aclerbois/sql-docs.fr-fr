---
title: getConcurrency (méthode) (SQLServerResultSet) | Documents Microsoft
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
- SQLServerResultSet.getConcurrency
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 207e25f4-769c-4ff3-913c-3517b06208e4
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 94bf72d388a3bdf1ae30ee9febfc1c7c5692fc45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getconcurrency-method-sqlserverresultset"></a>getConcurrency (méthode) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère le mode d’accès concurrentiel de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int getConcurrency()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Un **int** qui indique le type d’accès concurrentiel, qui peut prendre l’une des valeurs suivantes :  
  
 ResultSet.CONCUR_READ_ONLY  
  
 ResultSet.CONCUR_UPDATABLE  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getConcurrency est spécifiée par la méthode getConcurrency dans l’interface java.sql.ResultSet.  
  
 La concurrence utilisée est déterminée par le [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet qui a créé le jeu de résultats.  
  
 Cette méthode permet de déterminer la concurrence réelle. Si l'application a sélectionné CONCUR_READ_ONLY ou CONCUR_UPDATABLE, ces options seront retournées. Si l'application a utilisé la concurrence par défaut, c'est CONCUR_READ_ONLY qui est retourné.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
