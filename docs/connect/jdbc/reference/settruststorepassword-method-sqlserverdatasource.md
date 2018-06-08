---
title: Méthode setTrustStorePassword (SQLServerDataSource) | Documents Microsoft
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
- setTrustStorePassword Method (SQLServerDataSource)
apilocation:
- setTrustStorePassword Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: fa87cbde-71cc-4f21-bc07-f8ba2b6a0a3f
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2dbd22d01c5c4523fdd04a12d68b9c716fd8643d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="settruststorepassword-method-sqlserverdatasource"></a>Méthode setTrustStorePassword (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le mot de passe utilisé pour vérifier l'intégrité des données trustStore.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setTrustStorePassword(java.lang.String trustStorePassword)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *trustStorePassword*  
  
 A **chaîne** qui contient le mot de passe qui est utilisé pour vérifier l’intégrité des données trustStore.  
  
## <a name="remarks"></a>Notes  
 La propriété trustStorePassword peut être spécifiée avec la propriété trustStore et sa valeur est utilisée pour vérifier l'intégrité du fichier trustStore.  
  
 Si la propriété trustStore est définie mais que la propriété trustStorePassword n'est pas définie, l'intégrité du trustStore n'est pas vérifiée.  
  
 Lorsque les propriétés trustStore et trustStorePassword ne sont pas spécifiées, le pilote utilise les propriétés système JVM (Java Virtual Machine) « javax.net.ssl.trustStore » et « javax.net.ssl.trustStorePassword ». Si la propriété système « javax.net.ssl.trustStorePassword » n'est pas spécifiée, l'intégrité du trustStore n'est pas vérifiée.  
  
 Si la propriété trustStore n'est pas définie mais que la propriété trustStorePassword est définie, le pilote JDBC utilise le fichier spécifié par la propriété « javax.net.ssl.trustStore » comme magasin d'approbations et l'intégrité du magasin d'approbations est vérifiée à l'aide de la propriété trustStorePassword spécifiée. Cela peut être nécessaire lorsque l'application cliente ne souhaite pas stocker le mot de passe dans la propriété système JVM.  
  
 Pour plus d’informations, consultez [définissant les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 Depuis la version 3.0 du pilote JDBC, si vous définissez SQLServerDataSource.setTrustStorePassword avant de lier les propriétés de la source de données, vous devez appeler SQLServerDataSource.setTrustStorePassword avant d'obtenir la connexion. Pour plus d’informations, consultez [SQLServerDataSource.getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
