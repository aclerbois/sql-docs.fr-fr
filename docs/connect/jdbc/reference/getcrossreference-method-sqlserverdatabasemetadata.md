---
title: Méthode getCrossReference (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getCrossReference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 099dd0bf-b017-479d-9696-f5b06f4c6bf9
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8876c49e809cf1bd941937c294d6122bb3c7d3f3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getcrossreference-method-sqlserverdatabasemetadata"></a>Méthode getCrossReference (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des colonnes de clés étrangères dans la table de clés étrangères donnée qui référence les colonnes de clés primaires de la table de clés primaires donnée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getCrossReference(java.lang.String cat1,  
                                            java.lang.String schem1,  
                                            java.lang.String tab1,  
                                            java.lang.String cat2,  
                                            java.lang.String schem2,  
                                            java.lang.String tab2)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *cat1*  
  
 A **chaîne** qui contient le nom du catalogue de la table qui contient la clé primaire.  
  
 *schem1*  
  
 A **chaîne** qui contient le nom de schéma de la table qui contient la clé primaire.  
  
 *Tab1*  
  
 A **chaîne** qui contient le nom de la table qui contient la clé primaire de la table.  
  
 *Cat2*  
  
 A **chaîne** qui contient le nom du catalogue de la table qui contient la clé étrangère.  
  
 *schem2*  
  
 A **chaîne** qui contient le nom du schéma de la table qui contient la clé étrangère.  
  
 *Tab2*  
  
 A **chaîne** qui contient le nom de la table de la table qui contient la clé étrangère.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getCrossReference est spécifiée par la méthode getCrossReference dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getCrossReference contient les informations suivantes :  
  
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
>  Pour plus d’informations sur les données retournées par la méthode getCrossReference, consultez « sp_fkeys (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getCrossReference pour retourner des informations sur la relation de clé primaire et étrangère entre les tables Person.Contact et HumanResources.Employee dans la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetCrossReference(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getCrossReference("AdventureWorks", "Person", "Contact", null, "HumanResources", "Employee");  
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
  
  
