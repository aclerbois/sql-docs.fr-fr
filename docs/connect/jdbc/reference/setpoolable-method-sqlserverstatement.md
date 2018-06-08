---
title: setPoolable (méthode) (SQLServerStatement) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f0f798c8-cafb-4acc-b85d-2e0059c91d92
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1cc4938abaf5c281da0bae0c14d86b85883d4062
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setpoolable-method-sqlserverstatement"></a>setPoolable (méthode) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Demande qu'une instruction soit regroupée ou non.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setPoolable(boolean poolable) throws SQLException  
```  
  
#### <a name="parameters"></a>Paramètres  
 *peut être regroupé*  
  
 Si **true**, demande que l’instruction soit regroupée. Si **false**, demande que l’instruction ne soit ne pas regroupée.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 La valeur spécifiée dans le *peut être regroupé* paramètre est un indicateur à l’implémentation de pool d’instruction qui indique si l’application souhaite que l’instruction soit regroupée. Le gestionnaire de regroupement d'instructions décide s'il va utiliser cette indication.  
  
 La valeur de regroupement d'une instruction s'applique à la fois aux caches d'instructions internes implémentés par le pilote et aux caches d'instructions externes implémentés par des serveurs d'applications et d'autres applications.  
  
 Par défaut, un objet SQLServerStatement n’est pas regroupé. Objets SQLServerPreparedStatement et SQLServerCallableStatement sont à leur création.  
  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) est levée si cette méthode est appelée sur une instruction fermée.  
  
 [isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md) retourne une valeur indiquant si l’objet est regroupé.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
