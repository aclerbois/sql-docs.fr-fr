---
title: Méthode getIndexInfo (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getIndexInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8a677cc6-8e33-4e57-8678-0849345aa8d0
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cef7b37818e5bc7bf46c7181a3816edd5bbad860
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getindexinfo-method-sqlserverdatabasemetadata"></a>Méthode getIndexInfo (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des index et statistiques pour la table donnée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getIndexInfo(java.lang.String cat,  
                                       java.lang.String schema,  
                                       java.lang.String table,  
                                       boolean unique,  
                                       boolean approximate)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *CAT*  
  
 A **chaîne** qui contient le nom du catalogue.  
  
 *schema*  
  
 A **chaîne** qui contient le nom du schéma.  
  
 *table*  
  
 A **chaîne** qui contient le nom de table.  
  
 *Unique*  
  
 **true** si seuls les index de valeurs uniques sont retournés. **false** si tous les index sont retournés.  
  
 *approximative*  
  
 **true** si les résultats reflètent des valeurs approximatives ou obsolètes. **false** si les résultats sont précis.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getIndexInfo est spécifiée par la méthode getIndexInfo dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getIndexInfo contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|TABLE_CAT|**String**|Le nom de la base de données dans laquelle réside la table spécifiée.|  
|TABLE_SCHEM|**String**|Schéma de la table.|  
|TABLE_NAME|**String**|Nom de la table.|  
|NON_UNIQUE|**boolean**|Indique si les valeurs d'index peuvent ne pas être uniques.|  
|INDEX_QUALIFIER|**String**|Nom du propriétaire de l’index. Il peut avoir la valeur Null lorsque TYPE correspond à tableIndexStatistic.|  
|INDEX_NAME|**String**|Le nom de l’index.|  
|TYPE|**courte**|Le type de l’index. Il peut prendre l’une des valeurs suivantes :<br /><br /> tableIndexStatistic (0)<br /><br /> tableIndexClustered (1)<br /><br /> tableIndexHashed (2)<br /><br /> tableIndexOther (3)|  
|ORDINAL_POSITION|**courte**|Position ordinale de la colonne dans l'index. La première colonne dans l'index est 1.|  
|COLUMN_NAME|**String**|Nom de la colonne.|  
|ASC_OR_DESC|**String**|Ordre utilisé dans le classement de l'index. Il peut prendre l’une des valeurs suivantes :<br /><br /> A (croissant)<br /><br /> D (décroissant)<br /><br /> NULL (non applicable)<br /><br /> **Remarque :** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] renvoie toujours « A ».  |  
|CARDINALITY|**int**|Nombre de lignes dans la table ou de valeurs uniques dans l'index.|  
|PAGES|**int**|Nombre de pages utilisées pour le stockage de l'index ou de la table.|  
|FILTER_CONDITION|**String**|Condition de filtrage.<br /><br /> **Remarque :** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] retourne toujours null.  |  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getIndexInfo, consultez « sp_indexes (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getIndexInfo pour renvoyer des informations sur les index et les statistiques de la table Person.Contact dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetIndexInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getIndexInfo("AdventureWorks", "Person", "Contact", false, true);  
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
  
  
