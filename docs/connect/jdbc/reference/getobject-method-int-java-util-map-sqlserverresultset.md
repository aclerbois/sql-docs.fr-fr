---
title: "Méthode getObject (int, java.util.Map) (SQLServerResultSet) | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: jdbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
apiname: SQLServerResultSet.getObject (int, java.util.Map)
apilocation: sqljdbc.jar
apitype: Assembly
ms.assetid: df85a514-ab43-4bf6-98dd-f7f37fad1850
caps.latest.revision: "15"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 06724ec0807059fcad7ae5bf26b267a2e8322b86
ms.sourcegitcommit: 2713f8e7b504101f9298a0706bacd84bf2eaa174
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2017
---
# <a name="getobject-method-int-javautilmap-sqlserverresultset"></a>Méthode getObject (int, java.util.Map) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtient la valeur de l’index de colonne désigné dans la ligne actuelle de ce [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet comme un objet dans le langage de programmation, à l’aide de l’objet donné de la carte.  
  
> [!NOTE]  
>  Cette méthode n’est pas actuellement pris en charge par le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Lorsqu'elle est utilisée, cette méthode retourne toujours le mappage par défaut.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.Object getObject(int i,  
                                  java.util.Map map)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *J’ai*  
  
 Un **int** qui indique l’index de colonne.  
  
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
 [Méthode getObject &#40; SQLServerResultSet &#41;](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)   
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
