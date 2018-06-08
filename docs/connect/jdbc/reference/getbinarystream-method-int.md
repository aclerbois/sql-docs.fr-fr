---
title: Méthode getBinaryStream (int) | Documents Microsoft
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
- SQLServerResultSet.getBinaryStream (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: de22a6c4-1ba3-4ed0-91a2-bf235c2ceec3
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5147934e43cd1e0ae50262aa3de35dee8bc7763c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getbinarystream-method-int"></a>Méthode getBinaryStream (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de colonne désigné dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet en tant que flux binaire d’octets non interprétés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.io.InputStream getBinaryStream(int columnIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 Un **int** qui indique l’index de colonne.  
  
## <a name="return-value"></a>Valeur retournée  
 Un objet InputStream.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBinaryStream est spécifiée par la méthode getBinaryStream dans l’interface java.sql.ResultSet.  
  
 Cette méthode peut être utilisée uniquement avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] de binary, varbinary, varbinary (max) et image, les types de données. Si vous essayez de l'utiliser avec d'autres types de données, une exception est levée.  
  
 Une fois que cette méthode a obtenu la valeur sous forme de flux, cette valeur peut être lue par segments à partir du flux. Cette méthode convient particulièrement à la récupération de grandes valeurs LONGVARBINARY.  
  
> [!NOTE]  
>  Toutes les données figurant dans le flux retourné doivent être lues avant d'obtenir la valeur de toute autre colonne. L'appel suivant à une méthode getter fermera implicitement le flux. En outre, un flux de données peut retourner 0 lors de la méthode InputStream.available est appelée, si les données est disponible ou non.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getBinaryStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)   
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
