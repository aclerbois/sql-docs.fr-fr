---
title: Exemple d’URL de connexion | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 96fabc42-59d1-4cc0-93c5-db00cbe55e95
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6a6e49fea6fdc9ed6d7e7497b8217bb0a16dc2c1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-url-sample"></a>Exemple d'URL de connexion
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exemple d’application montre comment se connecter à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide d’une URL de connexion. Il montre également comment récupérer des données d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide d’une instruction SQL.  
  
 Le fichier code de cet exemple est appelé connectURL.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\connections  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d’application, vous devez définir l’instruction classpath pour inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Vous devrez également accéder à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple. Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code définit différentes propriétés de connexion de l’URL de connexion, puis appelle la méthode getConnection de la classe DriverManager pour retourner un [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) objet.  
  
 Ensuite, l’exemple de code utilise le [createStatement](../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md) méthode de l’objet SQLServerConnection pour créer un [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) objet, puis le [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) (méthode) est appelé pour exécuter l’instruction SQL.  
  
 Enfin, l’exemple utilise le [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) objet retourné par la méthode executeQuery pour parcourir les résultats retournés par l’instruction SQL.  
  
```java  
import java.sql.*;  
  
public class connectURL {  
  
   public static void main(String[] args) {  
  
      // Create a variable for the connection string.  
      String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
         "databaseName=AdventureWorks;user=UserName;password=*****";  
  
      // Declare the JDBC objects.  
      Connection con = null;  
      Statement stmt = null;  
      ResultSet rs = null;  
  
      try {  
         // Establish the connection.  
         Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
         con = DriverManager.getConnection(connectionUrl);  
  
         // Create and execute an SQL statement that returns some data.  
         String SQL = "SELECT TOP 10 * FROM Person.Contact";  
         stmt = con.createStatement();  
         rs = stmt.executeQuery(SQL);  
  
         // Iterate through the data in the result set and display it.  
         while (rs.next()) {  
            System.out.println(rs.getString(4) + " " + rs.getString(6));  
         }  
      }  
  
      // Handle any errors that may have occurred.  
      catch (Exception e) {  
         e.printStackTrace();  
      }  
      finally {  
         if (rs != null) try { rs.close(); } catch(Exception e) {}  
         if (stmt != null) try { stmt.close(); } catch(Exception e) {}  
         if (con != null) try { con.close(); } catch(Exception e) {}  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion et récupération de données](../../connect/jdbc/connecting-and-retrieving-data.md)  
  
  
