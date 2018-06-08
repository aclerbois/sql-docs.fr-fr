---
title: Méthode getApplicationName (SQLServerDataSource) | Documents Microsoft
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
- SQLServerDataSource.getApplicationName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f71e501c-ccd7-4a1e-b6ea-4d47a81c18c6
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0239d7d251b4104000646a8f98a3cae0be656853
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getapplicationname-method-sqlserverdatasource"></a>Méthode getApplicationName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Renvoie le nom de l'application.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getApplicationName()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 A **chaîne** qui contient le nom de l’application, ou «[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]» si aucune valeur n’est définie.  
  
## <a name="remarks"></a>Notes  
 Le nom de l’application est utilisé pour identifier l’application dans différentes [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] profilage et outils de journalisation. Si le nom de l’application n’est pas défini, la méthode getApplicationName retourne la chaîne non localisée «[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]».  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
