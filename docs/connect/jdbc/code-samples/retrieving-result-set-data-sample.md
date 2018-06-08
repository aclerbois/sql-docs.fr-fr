---
title: Exemple de données du jeu de résultat de la récupération des | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1b190c36-3d38-49a2-8599-612329675851
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: abe60fd0d28ddfe46b08490763103d9392ce521d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieving-result-set-data-sample"></a>Extraction de l'exemple de données du jeu de résultats
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] exemple d’application montre comment récupérer un jeu de données d’une [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] de base de données et afficher ces données.  
  
 Le fichier de code de cet exemple est appelé retrieveRS.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\resultsets  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d'application, vous devez définir l'instruction classpath de façon à inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Vous devrez également accéder à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Puis, à l’aide d’une instruction SQL avec la [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) de l’objet, il exécute l’instruction SQL et place les données qu’il renvoie dans un [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
 Ensuite, l’exemple de code appelle la méthode displayRow personnalisée pour parcourir les lignes de données qui sont contenues dans le jeu de résultats et utilise le [getString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md) méthode pour afficher des données qu’il contient.  
  
```java
import java.sql.*;  
  
public class retrieveRS {  
  
   public static void main(String[] args) {  
  
      // Create a variable for the connection string.  
      String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
            "databaseName=AdventureWorks;integratedSecurity=true;";  
  
      // Declare the JDBC objects.  
      Connection con = null;  
      Statement stmt = null;  
      ResultSet rs = null;  
  
      try {  
  
         // Establish the connection.  
         Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
         con = DriverManager.getConnection(connectionUrl);  
  
         // Create and execute an SQL statement that returns a  
         // set of data and then display it.  
         String SQL = "SELECT * FROM Production.Product;";  
         stmt = con.createStatement();  
         rs = stmt.executeQuery(SQL);  
         displayRow("PRODUCTS", rs);  
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
  
   private static void displayRow(String title, ResultSet rs) {  
      try {  
         System.out.println(title);  
         while (rs.next()) {  
            System.out.println(rs.getString("ProductNumber") + " : " + rs.getString("Name"));  
         }  
      } catch (Exception e) {  
         e.printStackTrace();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de jeux de résultats](../../../connect/jdbc/working-with-result-sets.md)  
  
  
