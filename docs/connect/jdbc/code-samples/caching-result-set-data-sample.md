---
title: Exemple de données du jeu de résultat de la mise en cache | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 13a95ebb-996c-4713-a1bd-5834fe22a334
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f5e76a7d66a2cba66774a27e5d0f0c155bfb92d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="caching-result-set-data-sample"></a>Mise en cache de l'exemple de données du jeu de résultats
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] exemple d’application montre comment récupérer un grand ensemble de données à partir d’une base de données et de contrôler le nombre de lignes de données qui sont mises en cache sur le client à l’aide de la [setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md) méthode de la [ SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
> [!NOTE]  
>  Limiter le nombre de lignes mises en cache sur le client et limiter le nombre total de lignes qu'un jeu de résultats peut contenir sont deux choses différentes. Pour contrôler le nombre total de lignes qui sont contenus dans un jeu de résultats, utilisez le [setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet, qui est héritée par les deux le [ SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) objets.  
  
 Pour définir une limite du nombre de lignes mises en cache sur le client, vous devez d’abord utiliser un curseur côté serveur lorsque vous créez un des objets de l’instruction en indiquant le type de curseur à utiliser lors de la création de l’objet d’instruction de manière spécifique. Par exemple, le pilote JDBC fournit le type de curseur TYPE_SS_SERVER_CURSOR_FORWARD_ONLY, qui est une avance rapide uniquement, curseur côté serveur en lecture seule pour une utilisation avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] bases de données.  
  
> [!NOTE]  
>  Une alternative à l'utilisation d'un type de curseur spécifique à SQL Server est l'utilisation de la propriété de chaîne de connexion selectMethod, en définissant sa valeur sur « curseur ». Pour plus d’informations sur les types de curseurs pris en charge par le pilote JDBC, consultez [présentation des Types de curseur](../../../connect/jdbc/understanding-cursor-types.md).  
  
 Une fois que vous avez exécuté la requête contenue dans l’objet d’instruction et les données sont retournées au client en conséquence ensemble, vous pouvez appeler la méthode setFetchSize pour contrôler la quantité de données à récupérer à partir de la base de données en même temps. Par exemple, si une table contient 100 lignes de données et si vous définissez la taille d'extraction à 10, seules 10 lignes de données seront mises en cache sur le client à tout moment. Même si ceci ralentit la vitesse de traitement des données, cela présente l'avantage d'utiliser moins de mémoire sur le client, ce qui peut être particulièrement utile lorsque vous devez traiter de grandes quantités de données.  
  
 Le fichier de code de cet exemple est appelé cacheRS.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\resultsets  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d'application, vous devez définir l'instruction classpath de façon à inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Vous devrez également accéder à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal_md.md)] base de données exemple. Ensuite, il utilise une instruction SQL avec la [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet, spécifie le type de curseur côté serveur, puis exécute l’instruction SQL et place les données qu’il renvoie dans un objet SQLServerResultSet.  
  
 Ensuite, l’exemple de code appelle la méthode de timerTest personnalisé, en passant la taille d’extraction en tant qu’arguments, à utiliser et le jeu de résultats. La méthode timerTest définit la taille d’extraction du jeu de résultats en utilisant la méthode setFetchSize, définit l’heure de début du test, puis itère ensuite au jeu de résultats avec un `While` boucle. Dès que le `While` boucle est fermée, le code définit l’heure d’arrêt du test et affiche le résultat du test, y compris la taille d’extraction, le nombre de lignes traitées, et le temps requis pour exécuter le test.  
  
```java
import java.sql.*;  
import com.microsoft.sqlserver.jdbc.SQLServerResultSet;  
  
public class cacheRS {  
  
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
  
         // Create and execute an SQL statement that returns a large  
         // set of data and then display it.  
         String SQL = "SELECT * FROM Sales.SalesOrderDetail;";  
         stmt = con.createStatement(SQLServerResultSet.TYPE_SS_SERVER_CURSOR_FORWARD_ONLY, +  
               SQLServerResultSet.CONCUR_READ_ONLY);  
  
         // Perform a fetch for every row in the result set.  
         rs = stmt.executeQuery(SQL);  
         timerTest(1, rs);  
         rs.close();  
  
         // Perform a fetch for every tenth row in the result set.  
         rs = stmt.executeQuery(SQL);  
         timerTest(10, rs);  
         rs.close();  
  
         // Perform a fetch for every 100th row in the result set.  
         rs = stmt.executeQuery(SQL);  
         timerTest(100, rs);  
         rs.close();  
  
         // Perform a fetch for every 1000th row in the result set.  
         rs = stmt.executeQuery(SQL);  
         timerTest(1000, rs);  
         rs.close();  
  
         // Perform a fetch for every 128th row (the default) in the result set.  
         rs = stmt.executeQuery(SQL);  
         timerTest(0, rs);  
         rs.close();  
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
  
   private static void timerTest(int fetchSize, ResultSet rs) {  
      try {  
  
         // Declare the variables for tracking the row count and elapsed time.  
         int rowCount = 0;  
         long startTime = 0;  
         long stopTime = 0;  
         long runTime = 0;  
  
         // Set the fetch size then iterate through the result set to  
         // cache the data locally.  
         rs.setFetchSize(fetchSize);  
         startTime = System.currentTimeMillis();  
         while (rs.next()) {  
            rowCount++;  
         }  
         stopTime = System.currentTimeMillis();  
         runTime = stopTime - startTime;  
  
         // Display the results of the timer test.  
         System.out.println("FETCH SIZE: " + rs.getFetchSize());  
         System.out.println("ROWS PROCESSED: " + rowCount);  
         System.out.println("TIME TO EXECUTE: " + runTime);  
         System.out.println();  
  
      } catch (Exception e) {  
         e.printStackTrace();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de jeux de résultats](../../../connect/jdbc/working-with-result-sets.md)  
  
  
