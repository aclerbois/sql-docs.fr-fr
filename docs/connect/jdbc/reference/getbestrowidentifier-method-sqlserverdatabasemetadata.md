---
title: Méthode getBestRowIdentifier (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getBestRowIdentifier
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c19e9ca6-2a53-4a0c-91ab-80090c3f7229
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c36b69543f5253a4d62c1d4b149933febcf0b4b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getbestrowidentifier-method-sqlserverdatabasemetadata"></a>Méthode getBestRowIdentifier (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description du jeu optimal de colonnes d'une table qui identifie une ligne de façon unique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getBestRowIdentifier(java.lang.String catalog,  
                                               java.lang.String schema,  
                                               java.lang.String table,  
                                               int scope,  
                                               boolean nullable)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 A **chaîne** qui contient le nom du catalogue.  
  
 *schema*  
  
 A **chaîne** qui contient le nom du schéma.  
  
 *table*  
  
 A **chaîne** qui contient le nom de table.  
  
 *portée*  
  
 Un **int** qui indique l’étendue d’intérêt. Les valeurs peuvent être les suivants :  
  
 bestRowTemporary (0)  
  
 bestRowTransaction (1)  
  
 bestRowSession (2)  
  
 *Accepte les valeurs null*  
  
 **true** pour inclure les colonnes autorisant des valeurs NULL. Dans le cas contraire, la valeur est **false**.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBestRowIdentifier est spécifiée par la méthode getBestRowIdentifier dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getBestRowIdentifier contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|SCOPE|short|Étendue des résultats retournés. Il peut prendre l’une des valeurs suivantes :<br /><br /> bestRowTemporary (0)<br /><br /> bestRowTransaction (1)<br /><br /> bestRowSession (2)|  
|COLUMN_NAME|Chaîne|Nom de la colonne.|  
|DATA_TYPE|short|Type de données SQL de java.sql.Types.|  
|TYPE_NAME|Chaîne|Nom du type de données.|  
|COLUMN_SIZE|int|La précision de la colonne.|  
|BUFFER_LENGTH|int|Longueur du tampon.|  
|DECIMAL_DIGITS|short|L’échelle de la colonne.|  
|PSEUDO_COLUMN|short|Indique si la colonne est une pseudo-colonne. Il peut prendre l’une des valeurs suivantes :<br /><br /> bestRowUnknown (0)<br /><br /> bestRowNotPseudo (1)<br /><br /> bestRowPseudo (2)|  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getBestRowIdentifier pour renvoyer des informations sur le meilleur identificateur de ligne pour la table Person.Contact dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetBestRowIdentifier(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getBestRowIdentifier(null, "Person", "Contact", 0, true);  
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
  
  
