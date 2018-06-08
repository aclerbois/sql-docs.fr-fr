---
title: Classe SQLServerException | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: af5ef257-7cf6-4db3-b1ee-07d22d82bef1
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: afaffe76cecaf1b0e6a99eb49c46d8b77c72a746
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverexception-class"></a>Classe SQLServerException
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente une exécution incorrecte ou incomplète d'une instruction SQL.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** java.sql.SQLException  
  
 **Implémente :** java.io.Serializable  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final class SQLServerException  
```  
  
## <a name="remarks"></a>Notes  
 La classe SQLServerException gère les codes d’état SQL 92 et XOPEN. Il est possible de passer de l'un à l'autre à l'aide d'une propriété de connexion spécifiée par l'utilisateur. Les exceptions sont écrites dans les fichiers journaux ouverts qui ont été spécifiés.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerException](../../../connect/jdbc/reference/sqlserverexception-members.md)   
 [Référence d’API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
