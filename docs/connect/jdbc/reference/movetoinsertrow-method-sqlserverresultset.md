---
title: moveToInsertRow (méthode) (SQLServerResultSet) | Documents Microsoft
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
- SQLServerResultSet.moveToInsertRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f3c54bfe-d5b7-4f6e-ae6c-3e8954e5b1c9
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e3a53a13c1352f33033a9507b66a4b257265acc3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="movetoinsertrow-method-sqlserverresultset"></a>moveToInsertRow (méthode) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Déplace le curseur vers la ligne d'insertion.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void moveToInsertRow()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode moveToInsertRow est spécifiée par la méthode moveToInsertRow dans l’interface java.sql.ResultSet.  
  
 La position actuelle du curseur est mémorisée lorsque le curseur est placé sur la ligne d'insertion. La ligne d'insertion est une ligne spéciale associée à un jeu de résultats pouvant être mis à jour. Elle constitue essentiellement un tampon dans lequel une nouvelle ligne peut être construite en appelant les méthodes updater avant d'ajouter la ligne au jeu de résultats.  
  
 Seuls les updater, getter et [insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md) méthodes peuvent être appelées lorsque le curseur se trouve sur la ligne d’insertion. Toutes les colonnes dans un jeu de résultats doivent être une valeur chaque fois que cette méthode est appelée et avant d’appeler insertRow. Une méthode updater doit être appelée pour qu'une méthode getter puisse être elle-même appelée sur une valeur de colonne.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
