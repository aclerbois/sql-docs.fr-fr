---
title: Méthode getProcedureColumns (SQLServerDatabaseMetaData) | Documents Microsoft
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
- SQLServerDatabaseMetaData.getProcedureColumns
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4f0df8fe-3cd6-46e4-ae3c-dc23c35676b2
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d323f4ba70e439eb5ec6ee1ec6178c8d8ec69dc1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getprocedurecolumns-method-sqlserverdatabasemetadata"></a>Méthode getProcedureColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des paramètres de procédure stockée et les colonnes de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getProcedureColumns(java.lang.String sCatalog,  
                                              java.lang.String sSchema,  
                                              java.lang.String proc,  
                                              java.lang.String col)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sCatalog*  
  
 A **chaîne** qui contient le nom du catalogue. La spécification d'une valeur Null pour ce paramètre indique que le nom du catalogue n'a pas besoin d'être utilisé.  
  
 *%SL*  
  
 A **chaîne** qui contient le modèle de nom de schéma. La spécification d'une valeur Null pour ce paramètre indique que le nom du schéma n'a pas besoin d'être utilisé.  
  
 *proc*  
  
 A **chaîne** qui contient le modèle de nom de procédure.  
  
 *col*  
  
 A **chaîne** qui contient le modèle de nom de colonne. La spécification d'une valeur Null pour ce paramètre retourne une ligne pour chaque colonne.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getProcedureColumns est spécifiée par la méthode getProcedureColumns dans l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getProcedureColumns contient les informations suivantes :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|PROCEDURE_CAT|**String**|Nom de la base de données qui contient la procédure stockée spécifiée.|  
|PROCEDURE_SCHEM|**String**|Schéma pour la procédure stockée.|  
|PROCEDURE_NAME|**String**|Le nom de la procédure stockée.|  
|COLUMN_NAME|**String**|Nom de la colonne.|  
|COLUMN_TYPE|**courte**|Type de la colonne. Il peut prendre l’une des valeurs suivantes :<br /><br /> procedureColumnUnknown (0)<br /><br /> procedureColumnIn (1)<br /><br /> procedureColumnInOut (2)<br /><br /> procedureColumnOut (4)<br /><br /> procedureColumnReturn (5)<br /><br /> procedureColumnResult (3)|  
|DATA_TYPE|**smallint**|Type de données SQL de java.sql.Types.|  
|TYPE_NAME|**String**|Nom du type de données.|  
|PRECISION|**int**|Nombre total de chiffres significatifs.|  
|LENGTH|**int**|La longueur des données en octets.|  
|SCALE|**courte**|Le nombre de chiffres à droite de la virgule décimale.|  
|RADIX|**courte**|Base pour les types numériques.|  
|NULLABLE|**courte**|Indique si la colonne peut contenir une valeur Null. Il peut prendre l’une des valeurs suivantes :<br /><br /> procedureNoNulls (0)<br /><br /> procedureNullable (1)<br /><br /> procedureNullableUnknown (2)|  
|REMARKS|**String**|Description de la colonne de la procédure.<br /><br /> <br /><br /> **Remarque :** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] ne retourne pas une valeur pour cette colonne.|  
|COLUMN_DEF|**String**|La valeur par défaut de la colonne.|  
|SQL_DATA_TYPE|**smallint**|Cette colonne est le même que le **DATA_TYPE** colonne, à l’exception de la **datetime** et ISO **intervalle** des types de données.|  
|SQL_DATETIME_SUB|**smallint**|Le **datetime** ISO **intervalle** sous-code si la valeur de **SQL_DATA_TYPE** est **SQL_DATETIME** ou **SQL_INTERVAL**. Pour les données les types autres que **datetime** et ISO **intervalle**, cette colonne est NULL.|  
|CHAR_OCTET_LENGTH|**int**|Nombre maximal d'octets dans la colonne.|  
|ORDINAL_POSITION|**int**|Index de la colonne dans la table.|  
|IS_NULLABLE|**String**|Indique si la colonne autorise les valeurs Null.|  
|SS_TYPE_CATALOG_NAME|**String**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_TYPE_SCHEMA_NAME|**String**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_UDT_CATALOG_NAME|**String**|Type défini par l'utilisateur (UDT) du nom complet.|  
|SS_UDT_SCHEMA_NAME|**String**|Nom du catalogue dans lequel un nom de collection de schémas XML est défini. Si le nom du catalogue est introuvable, cette variable contient une chaîne vide.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**String**|Nom du schéma dans lequel un nom de collection de schémas XML est défini. Si le nom du schéma est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**String**|Nom d'une collection de schémas XML. Si le nom est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**String**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_XML_SCHEMACOLLECTION_NAME|**String**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_DATA_TYPE|**tinyint**|Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de données qui est utilisé par les procédures stockées étendues.<br /><br /> <br /><br /> **Remarque :** pour plus d’informations sur les types de données retournés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], consultez « Types de données (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.|  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getProcedureColumns, consultez « sp_sproc_columns (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode getProcedureColumns pour renvoyer des informations sur la procédure stockée uspGetBillOfMaterials dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple.  
  
```  
public static void executeGetProcedureColumns(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getProcedureColumns(null, null, "uspGetBillOfMaterials", null);  
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
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
