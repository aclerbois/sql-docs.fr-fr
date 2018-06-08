---
title: Modification des résultats du jeu de données exemple | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b5ae54dc-2a79-4664-bb21-cacdb7d745e1
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 47b2f8566607ec119aaa61a29468c7fca6388b5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modifying-result-set-data-sample"></a>Modification de l'exemple de données du jeu de résultats
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] exemple d’application montre comment récupérer un jeu de mise à jour des données d’une [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données. Puis, à l’aide des méthodes de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet, il insère, modifie et supprime une ligne de données à partir de l’ensemble de données.  
  
 Le fichier de code de cet exemple est appelé updateRS.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\resultsets  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d'application, vous devez définir l'instruction classpath de façon à inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Vous devrez également accéder à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Puis, à l’aide d’une instruction SQL avec la [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) de l’objet, il exécute l’instruction SQL et place les données qu’il renvoie dans un objet SQLServerResultSet être mis à jour.  
  
 Ensuite, l’exemple de code utilise le [moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md) méthode pour déplacer le jeu de résultats curseur à la ligne d’insertion, utilise une série de [updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md) méthodes pour insérer des données dans la nouvelle ligne, puis appelle la [insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md) méthode pour conserver la nouvelle ligne de données vers la base de données.  
  
 Après avoir inséré la nouvelle ligne de données, l’exemple de code utilise une instruction SQL pour extraire la ligne précédemment insérée et utilise ensuite la combinaison d’updateString et [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) sauvegarder des méthodes pour mettre à jour la ligne de données et la remettre à la base de données.  
  
 Enfin, l’exemple de code récupère la ligne précédemment mise à jour de données et supprime ensuite de la base de données à l’aide de la [deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md) (méthode).  
  
```java
import java.sql.*;  
  
public class updateRS {  
  
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
  
         // Create and execute an SQL statement, retrieving an updateable result set.  
         String SQL = "SELECT * FROM HumanResources.Department;";  
         stmt = con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE);  
         rs = stmt.executeQuery(SQL);  
  
         // Insert a row of data.  
         rs.moveToInsertRow();  
         rs.updateString("Name", "Accounting");  
         rs.updateString("GroupName", "Executive General and Administration");  
         rs.updateString("ModifiedDate", "08/01/2006");  
         rs.insertRow();  
  
         // Retrieve the inserted row of data and display it.  
         SQL = "SELECT * FROM HumanResources.Department WHERE Name = 'Accounting';";  
         rs = stmt.executeQuery(SQL);  
         displayRow("ADDED ROW", rs);  
  
         // Update the row of data.  
         rs.first();  
         rs.updateString("GroupName", "Finance");  
         rs.updateRow();  
  
         // Retrieve the updated row of data and display it.  
         rs = stmt.executeQuery(SQL);  
         displayRow("UPDATED ROW", rs);  
  
         // Delete the row of data.  
         rs.first();  
         rs.deleteRow();  
         System.out.println("ROW DELETED");  
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
            System.out.println(rs.getString("Name") + " : " + rs.getString("GroupName"));  
            System.out.println();  
         }  
      } catch (Exception e) {  
         e.printStackTrace();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de jeux de résultats](../../../connect/jdbc/working-with-result-sets.md)  
  
  
