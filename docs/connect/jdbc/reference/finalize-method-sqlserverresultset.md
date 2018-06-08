---
title: Finalize (méthode) (SQLServerResultSet) | Documents Microsoft
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
- SQLServerResultSet.finalize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 49bc879d-822b-42da-bc20-2394865f1f0f
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 56c7131bfc0b5f1bb293b7a697f090495f0938f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="finalize-method-sqlserverresultset"></a>Finalize (méthode) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cela ferme explicitement [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void finalize()  
```  
  
## <a name="remarks"></a>Notes  
 Ferme le jeu de résultats si l'application omet de le faire. Cette méthode existe uniquement pour la conformité avec la spécification JDBC. Comme la machine virtuelle Java (JVM) ne garantit pas lorsqu'un finaliseur aura la chance de s'exécuter, les applications qui négligent de fermer explicitement leurs jeux de résultats pourront toujours effectuer un interblocage sur une autre instruction utilisant la même connexion et bloquée sur une ressource de serveur commune, telle que des verrous de ligne.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
