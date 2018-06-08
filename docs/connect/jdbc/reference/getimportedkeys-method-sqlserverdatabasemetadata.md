---
title: Méthode getImportedKeys (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getImportedKeys
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: dc8c1a5e-700e-4059-a5ed-5013bbb87fb6
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: de5ae39ff10c32737df637c9a6f29aab19d27574
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getimportedkeys-method-sqlserverdatabasemetadata"></a>Méthode getImportedKeys (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des colonnes de clés primaires référencées par les colonnes de clés étrangères dans une table.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getImportedKeys(java.lang.String cat,  
                                          java.lang.String schema,  
                                          java.lang.String table)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *CAT*  
  
 A **chaîne** qui contient le nom du catalogue.  
  
 *schema*  
  
 A **chaîne** qui contient le nom du schéma.  
  
 *table*  
  
 A **chaîne** qui contient le nom de table.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getImportedKeys est spécifiée par la méthode getImportedKeys dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getImportedKeys contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|PKTABLE_CAT|**String**|Nom du catalogue qui contient la table de clés primaires.|  
|PKTABLE_SCHEM|**String**|Nom du schéma de la table de clés primaires.|  
|PKTABLE_NAME|**String**|Nom de la table de clés primaires.|  
|PKCOLUMN_NAME|**String**|Nom de colonne de la clé primaire.|  
|FKTABLE_CAT|**String**|Nom du catalogue qui contient la table de clés étrangères.|  
|FKTABLE_SCHEM|**String**|Nom du schéma de la table de clés étrangères.|  
|FKTABLE_NAME|**String**|Nom de la table de clés étrangères.|  
|FKCOLUMN_NAME|**String**|Nom de colonne de la clé étrangère.|  
|KEY_SEQ|**courte**|Numéro séquentiel de la colonne dans une clé primaire multicolonne.|  
|UPDATE_RULE|**courte**|Action appliquée à la clé étrangère lorsque l'opération SQL correspond à une mise à jour. Il peut prendre l’une des valeurs suivantes :<br /><br /> importedKeyNoAction (3)<br /><br /> importedKeyCascade (0)<br /><br /> importedKeySetNull (2)<br /><br /> importedKeySetDefault (4)<br /><br /> importedKeyRestrict (1)|  
|DELETE_RULE|**courte**|Action appliquée à la clé étrangère lorsque l'opération SQL correspond à une suppression. Il peut prendre l’une des valeurs suivantes :<br /><br /> importedKeyNoAction (3)<br /><br /> importedKeyCascade (0)<br /><br /> importedKeySetNull (2)<br /><br /> importedKeySetDefault (4)<br /><br /> importedKeyRestrict (1)|  
|FK_NAME|**String**|Nom de la clé étrangère.|  
|PK_NAME|**String**|Nom de la clé primaire.|  
|DEFERRABILITY|**courte**|Indique si l'évaluation de la contrainte de clé étrangère peut être différée jusqu'à une opération de validation. Il peut prendre l’une des valeurs suivantes :<br /><br /> importedKeyInitiallyDeferred (5)<br /><br /> importedKeyInitiallyImmediate (6)<br /><br /> importedKeyNotDeferrable (7)|  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getImportedKeys, consultez « sp_fkeys (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getImportedKeys pour renvoyer des informations sur toutes les clés primaires qui référencent les clés étrangères de la table Person.Address dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetImportedKeys(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getImportedKeys("AdventureWorks", "Person", "Address");  
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
  
  
