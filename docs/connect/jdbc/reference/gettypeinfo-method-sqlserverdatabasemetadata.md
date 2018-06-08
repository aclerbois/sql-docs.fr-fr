---
title: Méthode getTypeInfo (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d81b932d536240b01c79e8e4a8e8589efae4d603
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>Méthode getTypeInfo (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description de tous les types SQL standard pris en charge par la base de données actuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getTypeInfo est spécifiée par la méthode getTypeInfo dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getTypeInfo contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|TYPE_NAME|**String**|Nom du type de données.|  
|DATA_TYPE|**courte**|Type de données SQL de java.sql.Types.|  
|PRECISION|**int**|Nombre total de chiffres significatifs.|  
|LITERAL_PREFIX|**String**|Caractère(s) utilisé(s) devant une constante.|  
|LITERAL_SUFFIX|**String**|Caractère(s) utilisé(s) pour terminer une constante.|  
|CREATE_PARAMS|**String**|Description des paramètres de création du type de données.|  
|NULLABLE|**courte**|Indique si la colonne peut contenir une valeur Null. Il peut prendre l’une des valeurs suivantes :<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|Indique si le type de donnée respecte la casse. «**true**« si le type est respecte la casse ; sinon, »**false**».|  
|SEARCHABLE|**courte**|Indique si la colonne désignée peut être utilisée dans une clause SQL WHERE. Il peut prendre l’une des valeurs suivantes :<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|Indique le signe du type de données. «**true**« si le type n’est pas signé ; sinon, »**false**».|  
|FIXED_PREC_SCALE|**boolean**|Indique que le type de données peut être une valeur monétaire. «**true**» si le type de données est de type monnaie ; sinon, «**false**».|  
|AUTO_INCREMENT|**boolean**|Indique que le type de données peut être incrémenté automatiquement. «**true**« si le type peut être incrémenté automatiquement ; sinon, »**false**».|  
|LOCAL_TYPE_NAME|**String**|Nom localisé du type de données.|  
|MINIMUM_SCALE|**courte**|Nombre maximal de chiffres situés à droite de la virgule décimale.|  
|MAXIMUM_SCALE|**courte**|Nombre minimal de chiffres situés à droite de la virgule décimale.|  
|SQL_DATA_TYPE|**int**|Non pris en charge par le pilote JDBC.|  
|SQL_DATETIME_SUB|**int**|Non pris en charge par le pilote JDBC.|  
|NUM_PREC_RADIX|**int**|Nombre de bits ou de chiffres pour le calcul du nombre maximal pouvant être contenu dans la colonne.|  
|INTERVAL_PRECISION|**smallint**|Valeur de la précision d'en-tête de l'intervalle.|  
|USERTYPE|**smallint**|Le **usertype** valeur à partir de la **systypes** table. Pour plus d'informations, consultez la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].|  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getTypeInfo, consultez la rubrique « sp_datatype_info (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getTypeInfo pour retourner des informations sur les types de données utilisés dans un [!INCLUDE[ssVersion2005](../../../includes/ssversion2005_md.md)] (ou version ultérieure) base de données.  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
