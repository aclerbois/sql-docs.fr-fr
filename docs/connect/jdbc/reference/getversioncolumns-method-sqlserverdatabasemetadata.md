---
title: Méthode getVersionColumns (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getVersionColumns
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6dd275d3-d9b2-4db7-938a-d4406c940a7a
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 722fd20c9210b14cb503ab3a0189815fa3cc83dd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getversioncolumns-method-sqlserverdatabasemetadata"></a>Méthode getVersionColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des colonnes d'une table qui reflète automatiquement la mise à jour d'une valeur d'une ligne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getVersionColumns(java.lang.String catalog,  
                                            java.lang.String schema,  
                                            java.lang.String table)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 A **chaîne** qui contient le nom du catalogue.  
  
 *schema*  
  
 A **chaîne** qui contient le modèle de nom de schéma.  
  
 *table*  
  
 A **chaîne** qui contient le nom de table.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getVersionColumns est spécifiée par la méthode getVersionColumns dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getVersionColumns contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|SCOPE|**courte**|Non pris en charge par le pilote JDBC.|  
|COLUMN_NAME|**String**|Nom de la colonne.|  
|DATA_TYPE|**courte**|Type de données SQL de java.sql.Types.|  
|TYPE_NAME|**String**|Nom du type de données.|  
|COLUMN_SIZE|**int**|La précision de la colonne.|  
|BUFFER_LENGTH|**int**|Longueur de la colonne en octets.|  
|DECIMAL_DIGITS|**courte**|L’échelle de la colonne.|  
|PSEUDO_COLUMN|**courte**|Indique si la colonne est une pseudo-colonne. Il peut prendre l’une des valeurs suivantes :<br /><br /> versionColumnUnknown (0)<br /><br /> versionColumnNotPseudo (1)<br /><br /> versionColumnPseudo (2)|  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getVersionColumns, consultez la rubrique « sp_datatype_info (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getVersionColumns pour renvoyer des informations sur les colonnes qui sont automatiquement mis à jour dans la table Person.Contact dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetVersionColumns(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getVersionColumns("AdventureWorks", "Person", "Contact");  
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
  
  
