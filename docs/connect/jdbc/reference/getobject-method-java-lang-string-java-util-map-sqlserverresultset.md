---
title: Méthode getObject (java.lang.String, java.util.Map) | Documents Microsoft
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
- SQLServerResultSet.getObject (java.lang.String, java.util.Map)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8104406b-417d-4ff5-9aca-183ee0f76762
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3286cd54429926df0906394c9cd06feccc7b6af6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getobject-method-javalangstring-javautilmap-sqlserverresultset"></a>Méthode getObject (java.lang.String, java.util.Map) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtient la valeur du nom de colonne désigné dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet comme un objet dans le langage de programmation, à l’aide de l’objet donné de la carte.  
  
> [!NOTE]  
>  Cette méthode n’est pas actuellement pris en charge par le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Lorsqu'elle est utilisée, cette méthode retourne toujours le mappage par défaut.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.Object getObject(java.lang.String colName,  
                                  java.util.Map map)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Nom de colonne*  
  
 A **chaîne** qui contient le nom de colonne.  
  
 *carte*  
  
 Un objet de mappage.  
  
## <a name="return-value"></a>Valeur retournée  
 Un **objet** valeur.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getObject est spécifiée par la méthode getObject dans l’interface java.sql.ResultSet.  
  
 Cette méthode retourne la valeur de la colonne fournie en tant qu'objet Java. Le type de l'objet Java sera le type d'objet Java par défaut correspondant au type SQL de la colonne, en suivant le mappage pour les types intégrés indiqué dans la spécification JDBC. Si la valeur est SQL NULL, le pilote retourne une valeur Java NULL.  
  
 Cette méthode peut aussi servir à lire des types de données abstraits spécifiques à une base de données. Dans l’API JDBC 2.0, le comportement de la méthode getObject est étendu afin de matérialiser les données de types SQL définis par l’utilisateur. Lorsqu’une colonne contient une valeur structurée ou distincte, le comportement de cette méthode est comme s’il s’agissait d’un appel à `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 À compter de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] le pilote JDBC version 3.0 :  
  
-   Une valeur de type date est retournée en tant qu'objet java.sql.Date.  
  
-   Une valeur de type time est retournée en tant qu'objet java.sql.Time.  
  
-   Une valeur de type datetime2 est retournée en tant qu'objet java.sql.Timestamp.  
  
-   Une valeur de type datetimeoffset est retournée en tant qu'objet microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getObject &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)   
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
