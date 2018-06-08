---
title: À l’aide de la copie en bloc avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 21e19635-340d-49bb-b39d-4867102fb5df
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6daf0ae2773d8a99e4f9264c05024a86fcd79926
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-bulk-copy-with-the-jdbc-driver"></a>Utilisation de la copie en bloc avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Microsoft SQL Server inclut un utilitaire de ligne de commande connu nommé **bcp** pour rapidement copier en bloc des fichiers volumineux dans des tables ou vues dans les bases de données SQL Server. La classe SQLServerBulkCopy vous permet d’écrire des solutions de code en Java, qui fournissent des fonctionnalités similaires. Il existe autres façons de charger des données dans une table SQL Server (instructions INSERT, par exemple) mais SQLServerBulkCopy présente un avantage significative des performances.  
  
 La classe SQLServerBulkCopy peut être utilisée pour écrire des données uniquement dans des tables SQL Server. Toutefois, la source de données n'est pas limitée à SQL Server : toute source de données peut être utilisée, tant que les données sont lisibles avec une implémentation ResultSet, RowSet ou ISQLServerBulkRecord.  
  
 La classe SQLServerBulkCopy vous permet d'effectuer les opérations suivantes :  
  
-   Une opération unique de copie en bloc  
  
-   Plusieurs opérations de copie en bloc  
  
-   Une opération de copie en bloc avec une transaction  
  
> [!NOTE]  
>  Quand vous utilisez Microsoft JDBC Driver 4.1 pour SQL Server ou une version antérieure (qui ne prend pas en charge la classe SQLServerBulkCopy), vous pouvez exécuter l'instruction SQL Server Transact-SQL BULK INSERT à la place.  
  
## <a name="bulk-copy-example-setup"></a>Configuration de l'exemple de copie en bloc  
 La classe SQLServerBulkCopy peut être utilisée pour écrire des données uniquement dans des tables SQL Server. Les exemples de code présentés dans cette rubrique utilisent l'exemple de base de données SQL Server AdventureWorks. Pour éviter de modifier les exemples de code des tables existantes, écrivez les données dans des tables que vous aurez préalablement créées.  
  
 Les tables BulkCopyDemoMatchingColumns et BulkCopyDemoDifferentColumns sont basées sur la table AdventureWorks Production.Products. Dans les exemples de code qui utilisent ces tables, les données sont ajoutées à partir de la table Production.Products à l'un de ces exemples de tables. La table BulkCopyDemoDifferentColumns est utilisée quand l'exemple illustre comment mapper des colonnes de données source à la table de destination. BulkCopyDemoMatchingColumns est utilisé pour la plupart des autres exemples.  
  
 Certains exemples de code montrent comment utiliser une classe SQLServerBulkCopy pour écrire dans plusieurs tables. Pour ces exemples, les tables BulkCopyDemoOrderHeader et BulkCopyDemoOrderDetail sont utilisées comme tables de destination. Ces tables sont basées sur les tables Sales.SalesOrderHeader et Sales.SalesOrderDetail dans AdventureWorks.  
  
> [!NOTE]  
>  Les exemples de code SQLServerBulkCopy sont fournis pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
###  <a name="BKMK_TableSetup"></a> Programme d’installation de table  
 Pour créer les tables nécessaires à la bonne exécution des exemples de code, vous devez exécuter les instructions Transact-SQL suivantes dans une base de données SQL Server.  
  
```  
USE AdventureWorks  
  
IF EXISTS (SELECT * FROM dbo.sysobjects   
 WHERE id = object_id(N'[dbo].[BulkCopyDemoMatchingColumns]')   
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoMatchingColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoMatchingColumns]([ProductID] [int] IDENTITY(1,1) NOT NULL,  
    [Name] [nvarchar](50) NOT NULL,  
    [ProductNumber] [nvarchar](25) NOT NULL,  
 CONSTRAINT [PK_ProductID] PRIMARY KEY CLUSTERED   
(  
    [ProductID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects   
 WHERE id = object_id(N'[dbo].[BulkCopyDemoDifferentColumns]')   
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoDifferentColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoDifferentColumns]([ProdID] [int] IDENTITY(1,1) NOT NULL,  
    [ProdNum] [nvarchar](25) NOT NULL,  
    [ProdName] [nvarchar](50) NOT NULL,  
 CONSTRAINT [PK_ProdID] PRIMARY KEY CLUSTERED   
(  
    [ProdID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects   
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderHeader]')   
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderHeader]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderHeader]([SalesOrderID] [int] IDENTITY(1,1) NOT NULL,  
    [OrderDate] [datetime] NOT NULL,  
    [AccountNumber] [nvarchar](15) NULL,  
 CONSTRAINT [PK_SalesOrderID] PRIMARY KEY CLUSTERED   
(  
    [SalesOrderID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects   
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderDetail]')   
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderDetail]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderDetail]([SalesOrderID] [int] NOT NULL,  
    [SalesOrderDetailID] [int] NOT NULL,  
    [OrderQty] [smallint] NOT NULL,  
    [ProductID] [int] NOT NULL,  
    [UnitPrice] [money] NOT NULL,  
 CONSTRAINT [PK_LineNumber] PRIMARY KEY CLUSTERED   
(  
    [SalesOrderID] ASC,  
    [SalesOrderDetailID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
```  
  
## <a name="single-bulk-copy-operations"></a>Opérations uniques de copie en bloc  
 L'approche la plus simple pour effectuer une opération de copie en bloc SQL Server est d'effectuer une opération unique sur une base de données. Par défaut, une opération de copie en bloc est effectuée comme une opération isolée : l'opération de copie se produit de façon non transactionnelle, sans possibilité de restauration.  
  
> [!NOTE]  
>  Si vous devez restaurer tout ou partie de la copie en bloc lorsqu’une erreur se produit, vous pouvez utiliser une transaction gérée par SQLServerBulkCopy ou effectuer l’opération de copie en bloc dans une transaction existante.  
>   
>  Pour plus d’informations, consultez [opérations de copie en bloc et Transaction](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TransactionBulk)  
  
 Les étapes générales pour effectuer une opération de copie en bloc sont les suivantes :  
  
1.  Connectez-vous au serveur source et obtenez les données à copier. Les données peuvent également provenir d'autres sources, si elles peuvent être récupérées à partir d'un objet ResultSet ou d'une implémentation ISQLServerBulkRecord.  
  
2.  Se connecter au serveur de destination (à moins que vous souhaitiez **SQLServerBulkCopy** pour établir une connexion pour vous).  
  
3.  Créer un objet SQLServerBulkCopy, en définissant les propriétés requises via **setBulkCopyOptions**.  
  
4.  Appelez le **setDestinationTableName** opération d’insertion de méthode pour indiquer la table cible pour le bloc.  
  
5.  Appelez l’une de le **writeToServer** méthodes.  
  
6.  Si vous le souhaitez, mettez à jour des propriétés via **setBulkCopyOptions** et appelez **writeToServer** si nécessaire.  
  
7.  Appelez **fermer**, ou encapsuler les opérations de copie en bloc dans une instruction try-with-resources.  
  
> [!CAUTION]  
>  Il est préférable que les types de données des colonnes source et cible correspondent. Si les types de données ne correspondent pas, SQLServerBulkCopy essaie de convertir chaque valeur source en type de données cible. Les conversions peuvent affecter les performances et entraîner des erreurs inattendues. Par exemple, un type de données double peut être converti en type de données décimal la plupart du temps, mais pas toujours.  
  
### <a name="example"></a>Exemple  
 L'application suivante montre comment charger des données à l'aide de la classe SQLServerBulkCopy. Dans cet exemple, un ResultSet est utilisé pour copier des données de la table Production.Product de la base de données SQL Server AdventureWorks dans une table semblable de la même base de données.  
  
> [!IMPORTANT]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TableSetup). Ce code est fourni pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
```  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        try  
        {  
            // Note: if you are not using try-with-resources statements (as here),  
            // you must remember to call close() on any Connection, Statement,   
            // ResultSet, and SQLServerBulkCopy objects that you create.  
  
            // Open a sourceConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection sourceConnection = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = sourceConnection.createStatement())  
                {  
                    // Perform an initial count on the destination table.  
                    long countStart = 0;  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        countStart = rsRowCount.getInt(1);  
                        System.out.println("Starting row count = " + countStart);  
                    }  
  
                    // Get data from the source table as a ResultSet.  
                    try (ResultSet rsSourceData = stmt.executeQuery(  
                            "SELECT ProductID, Name, ProductNumber FROM Production.Product"))  
                    {  
                        // Open the destination connection. In the real world you would    
                        // not use SQLServerBulkCopy to move data from one table to the other    
                        // in the same database. This is for demonstration purposes only.   
                        try (Connection destinationConnection =   
                                DriverManager.getConnection(connectionString))  
                        {  
                            // Set up the bulk copy object.    
                            // Note that the column positions in the source   
                            // table match the column positions in    
                            // the destination table so there is no need to   
                            // map columns.   
                            try (SQLServerBulkCopy bulkCopy =  
                                       new SQLServerBulkCopy(destinationConnection))  
                            {  
                                bulkCopy.setDestinationTableName("dbo.BulkCopyDemoMatchingColumns");  
  
                                try  
                                {  
                                    // Write from the source to the destination.  
                                    bulkCopy.writeToServer(rsSourceData);  
                                }  
                                catch (Exception e)  
                                {  
                                    // Handle any errors that may have occurred.  
                                    e.printStackTrace();  
                                }  
                            }  
  
                            // Perform a final count on the destination    
                            // table to see how many rows were added.  
                            try (ResultSet rsRowCount = stmt.executeQuery(  
                                    "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                            {  
                                rsRowCount.next();  
                                long countEnd = rsRowCount.getInt(1);  
                                System.out.println("Ending row count = " + countEnd);  
                                System.out.println((countEnd - countStart) + " rows were added.");  
                            }  
                        }  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
  
        // Create a variable for the connection string.  
        String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
           "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
```  
  
### <a name="performing-a-bulk-copy-operation-using-transact-sql"></a>Exécution d'une opération de copie en bloc à l'aide de Transact-SQL  
 L'exemple suivant illustre comment utiliser la méthode executeUpdate pour exécuter l'instruction BULK INSERT.  
  
> [!NOTE]  
>  Le chemin d'accès de la source de données est relatif au serveur. Le processus serveur doit avoir accès à ce chemin d'accès pour que l'opération de copie en bloc réussisse.  
  
```  
try (Connection con = DriverManager.getConnection(connectionString))  
{  
    try (Statement stmt = con.createStatement())  
    {  
        // Perform the BULK INSERT  
        stmt.executeUpdate(  
                "BULK INSERT Northwind.dbo.[Order Details] " +  
                        "FROM 'f:\\mydata\\data.tbl' " +  
                        "WITH ( FORMATFILE='f:\\mydata\\data.fmt' )");  
    }  
}  
```  
  
## <a name="multiple-bulk-copy-operations"></a>Plusieurs opérations de copie en bloc  
 Vous pouvez effectuer plusieurs opérations de copie en bloc à l'aide d'une seule instance d'une classe SQLServerBulkCopy. Si les paramètres des opérations changent entre les copies (par exemple, le nom de la table de destination), vous devez les mettre à jour avant d'appeler l'une des méthodes writeToServer, comme illustré dans l'exemple suivant. Sans modification explicite, toutes les valeurs de propriété restent identiques à celles utilisées lors de la précédente opération de copie pour une instance donnée.  
  
> [!NOTE]  
>  L'exécution de plusieurs opérations de copie en bloc à l'aide de la même instance de SQLServerBulkCopy est généralement plus efficace que l'utilisation d'une instance distincte pour chaque opération.  
  
 Si vous effectuez plusieurs opérations de copie en bloc à l'aide du même objet SQLServerBulkCopy, aucune restriction ne s'applique selon que les informations sources ou cibles sont égales ou différentes dans chaque opération. Toutefois, vous devez vous assurer que les informations d'association de colonnes sont définies correctement chaque fois que vous écrivez sur le serveur.  
  
> [!IMPORTANT]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TableSetup). Ce code est fourni pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
```  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        try  
        {  
            // Note: if you are not using try-with-resources statements (as here),  
            // you must remember to call close() on any Connection, Statement,   
            // ResultSet, and SQLServerBulkCopy objects that you create.  
  
            // Open a sourceConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection sourceConnection1 = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = sourceConnection1.createStatement())  
                {  
  
                    // Empty the destination tables.   
                    stmt.executeUpdate("DELETE FROM dbo.BulkCopyDemoOrderHeader;");  
                    stmt.executeUpdate("DELETE FROM dbo.BulkCopyDemoOrderDetail;");  
  
                    // Perform an initial count on the destination   
                    //  table with matching columns.  
                    long countStartHeader = 0;  
                    try (ResultSet rsRowCountHeader = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoOrderHeader;"))  
                    {  
                        rsRowCountHeader.next();  
                        countStartHeader = rsRowCountHeader.getInt(1);  
                        System.out.println("Starting row count for Header table = " + countStartHeader);  
                    }  
  
                    // Perform an initial count on the destination   
                    // table with different column positions.  
                    long countStartDetail = 0;  
                    try (ResultSet rsRowCountDetail = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoOrderDetail;"))  
                    {  
                        rsRowCountDetail.next();  
                        countStartDetail = rsRowCountDetail.getInt(1);  
                        System.out.println("Starting row count for Detail table = " + countStartDetail);  
                    }  
  
                    // Get data from the source table as a ResultSet.   
                    // The Sales.SalesOrderHeader and Sales.SalesOrderDetail   
                    // tables are quite large and could easily cause a timeout   
                    // if all data from the tables is added to the destination.    
                    // To keep the example simple and quick, a parameter is     
                    // used to select only orders for a particular account    
                    // as the source for the bulk insert.  
                    try (PreparedStatement preparedStmt1 = sourceConnection1.prepareStatement(  
                            "SELECT [SalesOrderID], [OrderDate], " +  
                            "[AccountNumber] FROM [Sales].[SalesOrderHeader] " +  
                            "WHERE [AccountNumber] = ?;"))  
                    {  
                        preparedStmt1.setString(1, "10-4020-000034");  
  
                        try (ResultSet rsHeader = preparedStmt1.executeQuery())  
                        {  
                            // Get the Detail data in a separate connection.   
                            try (Connection sourceConnection2 = DriverManager.getConnection(connectionString))  
                            {  
                                try (PreparedStatement preparedStmt2 = sourceConnection2.prepareStatement(  
                                        "SELECT [Sales].[SalesOrderDetail].[SalesOrderID], " +  
                                                "[SalesOrderDetailID], [OrderQty], [ProductID], " +  
                                                "[UnitPrice] FROM [Sales].[SalesOrderDetail] " +  
                                                "INNER JOIN [Sales].[SalesOrderHeader] " +  
                                                "ON [Sales].[SalesOrderDetail].[SalesOrderID] " +  
                                                "= [Sales].[SalesOrderHeader].[SalesOrderID] " +  
                                                "WHERE [AccountNumber] = ?;"))  
                                {  
                                    preparedStmt2.setString(1, "10-4020-000034");  
  
                                    try (ResultSet rsDetail = preparedStmt2.executeQuery())  
                                    {  
                                        // Create the SQLServerBulkCopySQLServerBulkCopy object.    
                                        try (SQLServerBulkCopy bulkCopy =  
                                                   new SQLServerBulkCopy(connectionString))  
                                        {  
                                            bulkCopy.setBulkCopyOptions(copyOptions);  
                                            bulkCopy.setDestinationTableName("dbo.BulkCopyDemoOrderHeader");  
  
                                            // Guarantee that columns are mapped correctly by   
                                            // defining the column mappings for the order.  
                                            bulkCopy.addColumnMapping("SalesOrderID", "SalesOrderID");  
                                            bulkCopy.addColumnMapping("OrderDate", "OrderDate");  
                                            bulkCopy.addColumnMapping("AccountNumber", "AccountNumber");  
  
                                            // Write rsHeader to the destination.   
                                            try  
                                            {  
                                                bulkCopy.writeToServer(rsHeader);  
                                            }  
                                            catch (Exception e)  
                                            {  
                                                // Handle any errors that may have occurred.  
                                                e.printStackTrace();  
                                            }  
  
                                            // Set up the order details destination.   
                                            bulkCopy.setDestinationTableName("dbo.BulkCopyDemoOrderDetail");  
  
                                            // Clear the existing column mappings  
                                            bulkCopy.clearColumnMappings();  
  
                                            // Add order detail column mappings.  
                                            bulkCopy.addColumnMapping("SalesOrderID", "SalesOrderID");  
                                            bulkCopy.addColumnMapping(  
                                                    "SalesOrderDetailID",   
                                                    "SalesOrderDetailID");  
                                            bulkCopy.addColumnMapping("OrderQty", "OrderQty");  
                                            bulkCopy.addColumnMapping("ProductID", "ProductID");  
                                            bulkCopy.addColumnMapping("UnitPrice", "UnitPrice");  
  
                                            // Write rsDetail to the destination.   
                                            try  
                                            {  
                                                bulkCopy.writeToServer(rsDetail);  
                                            }  
                                            catch (Exception e)  
                                            {  
                                                // Handle any errors that may have occurred.  
                                                e.printStackTrace();  
                                            }  
                                        }  
                                    }  
                                }  
                            }  
                        }  
                    }  
  
                    // Perform a final count on the destination   
                    // tables to see how many rows were added.  
                    try (ResultSet rsRowCountHeader = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoOrderHeader;"))  
                    {  
                        rsRowCountHeader.next();  
                        long countEndHeader =  rsRowCountHeader.getInt(1);  
                        System.out.println(  
                                (countEndHeader - countStartHeader) +   
                                " rows were added to the Header table.");  
                    }  
  
                    try (ResultSet rsRowCountDetail = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoOrderDetail;"))  
                    {  
                        rsRowCountDetail.next();  
                        long countEndDetail = rsRowCountDetail.getInt(1);  
                        System.out.println(  
                                (countEndDetail - countStartDetail) +   
                                " rows were added to the Detail table.");  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
        // Create a variable for the connection string.  
        String connectionUrl = "jdbc:sqlserver://localhost:1433;"  
                + "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
  
```  
  
##  <a name="BKMK_TransactionBulk"></a> Transaction et opérations de copie en bloc  
 Les opérations de copie en bloc peuvent être effectuées comme des opérations isolées ou dans le cadre d'une transaction à plusieurs étapes. Cette dernière option permet d'effectuer plusieurs opérations de copie en bloc dans la même transaction ainsi que d'autres opérations de base de données (telles que des insertions, mises à jour et suppressions), tout en étant en mesure de valider ou restaurer toute la transaction.  
  
 Par défaut, une opération de copie en bloc est effectuée comme une opération isolée. L'opération de copie en bloc se produit de façon non transactionnelle, sans possibilité de restauration. Si vous devez restaurer tout ou partie de la copie en bloc en cas d'erreur, vous pouvez utiliser une transaction gérée par SQLServerBulkCopy ou effectuer l'opération de copie en bloc dans une transaction existante.  
  
### <a name="performing-a-non-transacted-bulk-copy-operation"></a>Exécution d'une opération de copie en bloc non transactionnelle  
 L'application suivante montre ce qui se passe quand une opération de copie en bloc non transactionnelle rencontre une erreur au cours de l'opération.  
  
 Dans l’exemple, la table source et la table de destination comportent chacune une colonne d’identité nommée **ProductID**. Le code commence par préparer la table de destination en supprimant toutes les lignes et puis en insérant une simple ligne dont **ProductID** est censé exister dans la table source. Par défaut, une nouvelle valeur pour la colonne d'identité est générée dans la table de destination pour chaque ligne ajoutée. Dans cet exemple, quand la connexion est ouverte, une option est définie qui oblige le processus de chargement en bloc à utiliser à la place les valeurs d'identité de la table source.  
  
 L’opération de copie en bloc est exécutée avec le **BatchSize** propriété définie sur 10. Quand l'opération rencontre la ligne non valide, une exception est levée. Dans ce premier exemple, l'opération de copie en bloc est non transactionnel. Tous les lots copiés avant l'erreur sont validés. Le lot contenant la clé dupliquée est restauré et l'opération de copie en bloc est interrompue avant le traitement des autres lots.  
  
> [!NOTE]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TableSetup). Ce code est fourni pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
```  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        try  
        {  
            // Note: if you are not using try-with-resources statements (as here),  
            // you must remember to call close() on any Connection, Statement,   
            // ResultSet, and SQLServerBulkCopy objects that you create.  
  
            // Open a sourceConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection sourceConnection = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = sourceConnection.createStatement())  
                {  
                    //  Delete all from the destination table.  
                    stmt.executeUpdate("DELETE FROM dbo.BulkCopyDemoMatchingColumns");  
  
                    //  Add a single row that will result in duplicate key            
                    //  when all rows from source are bulk copied.            
                    //  Note that this technique will only be successful in             
                    //  illustrating the point if a row with ProductID = 446              
                    //  exists in the AdventureWorks Production.Products table.             
                    //  If you have made changes to the data in this table, change            
                    //  the SQL statement in the code to add a ProductID that            
                    //  does exist in your version of the Production.Products            
                    //  table. Choose any ProductID in the middle of the table            
                    //  (not first or last row) to best illustrate the result.  
                    stmt.executeUpdate("SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns ON;" +  
                            "INSERT INTO " + "dbo.BulkCopyDemoMatchingColumns " +  
                            "([ProductID], [Name] ,[ProductNumber]) " +  
                            "VALUES(446, 'Lock Nut 23','LN-3416');" +  
                            "SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns OFF");  
  
                    // Perform an initial count on the destination table.  
                    long countStart = 0;  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        countStart = rsRowCount.getInt(1);  
                        System.out.println("Starting row count = " + countStart);  
                    }  
  
                    //  Get data from the source table as a ResultSet.  
                    try (ResultSet rsSourceData = stmt.executeQuery(  
                            "SELECT ProductID, Name, ProductNumber FROM Production.Product"))  
                    {  
  
                        // Set up the bulk copy object using the KeepIdentity option and BatchSize = 10.  
                        SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();  
                        copyOptions.setKeepIdentity(true);  
                        copyOptions.setBatchSize(10);  
  
                        try (SQLServerBulkCopy bulkCopy =   
                                new SQLServerBulkCopy(connectionString))  
                        {  
                            bulkCopy.setBulkCopyOptions(copyOptions);  
                            bulkCopy.setDestinationTableName("dbo.BulkCopyDemoMatchingColumns");  
  
                            // Write from the source to the destination.   
                            // This should fail with a duplicate key error   
                            // after some of the batches have been copied.   
                            try  
                            {  
                                bulkCopy.writeToServer(rsSourceData);  
                            }  
                            catch (Exception e)  
                            {  
                                // Handle any errors that may have occurred.  
                                e.printStackTrace();  
                            }  
                        }  
                    }  
  
                    // Perform a final count on the destination    
                    // table to see how many rows were added.  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        long countEnd = rsRowCount.getInt(1);  
                        System.out.println("Ending row count = " + countEnd);  
                        System.out.println((countEnd - countStart) + " rows were added.");  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
        // Create a variable for the connection String.  
      String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
              "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
```  
  
### <a name="performing-a-dedicated-build-copy-operation-in-a-transaction"></a>Exécution d'une opération de copie en bloc dédiée dans une transaction  
 Par défaut, une opération de copie en bloc est sa propre transaction. Si vous voulez effectuer une opération de copie en bloc dédiée, créez une instance de SQLServerBulkCopy avec une chaîne de connexion. Dans ce scénario, l'opération de copie en bloc crée, puis valide ou restaure la transaction. Vous pouvez spécifier explicitement le **UseInternalTransaction** option **SQLServerBulkCopyOptions** pour provoquer une opération de copie en bloc dans sa propre transaction, chaque lot de l’ensemble l’opération à exécuter dans une transaction distincte de la copie.  
  
> [!NOTE]  
>  Puisque des lots différents sont exécutés dans différentes transactions, si une erreur se produit pendant l'opération de copie en bloc, toutes les lignes du lot actuel sont restaurées, mais les lignes des lots précédents restent dans la base de données.  
  
 L’application suivante est similaire à l’exemple précédent, à une exception près : dans cet exemple, l'opération de copie en bloc gère ses propres transactions. Tous les lots copiés avant l'erreur sont validés. Le lot contenant la clé dupliquée est restauré et l'opération de copie en bloc est interrompue avant le traitement des autres lots.  
  
> [!NOTE]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TableSetup). Ce code est fourni pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
```  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        try  
        {  
            // Note: if you are not using try-with-resources statements (as here),  
            // you must remember to call close() on any Connection, Statement,   
            // ResultSet, and SQLServerBulkCopy objects that you create.  
  
            // Open a sourceConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection sourceConnection = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = sourceConnection.createStatement())  
                {  
                    //  Delete all from the destination table.  
                    stmt.executeUpdate("DELETE FROM dbo.BulkCopyDemoMatchingColumns");  
  
                    //  Add a single row that will result in duplicate key            
                    //  when all rows from source are bulk copied.            
                    //  Note that this technique will only be successful in             
                    //  illustrating the point if a row with ProductID = 446              
                    //  exists in the AdventureWorks Production.Products table.             
                    //  If you have made changes to the data in this table, change            
                    //  the SQL statement in the code to add a ProductID that            
                    //  does exist in your version of the Production.Products            
                    //  table. Choose any ProductID in the middle of the table            
                    //  (not first or last row) to best illustrate the result.  
                    stmt.executeUpdate("SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns ON;" +  
                            "INSERT INTO " + "dbo.BulkCopyDemoMatchingColumns " +  
                            "([ProductID], [Name] ,[ProductNumber]) " +  
                            "VALUES(446, 'Lock Nut 23','LN-3416');" +  
                            "SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns OFF");  
  
                    // Perform an initial count on the destination table.  
                    long countStart = 0;  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        countStart = rsRowCount.getInt(1);  
                        System.out.println("Starting row count = " + countStart);  
                    }  
  
                    //  Get data from the source table as a ResultSet.  
                    try (ResultSet rsSourceData = stmt.executeQuery(  
                            "SELECT ProductID, Name, ProductNumber FROM Production.Product"))  
                    {  
  
                        // Set up the bulk copy object.  
                        // Note that when specifying the UseInternalTransaction   
                        // option, you cannot also use an external transaction.   
                        // Therefore, you must use the SQLServerBulkCopy construct that   
                        // requires a string for the connection, rather than an   
                        // existing Connection object.   
                        SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();  
                        copyOptions.setKeepIdentity(true);  
                        copyOptions.setBatchSize(10);  
                        copyOptions.setUseInternalTransaction(true);  
  
                        try (SQLServerBulkCopy bulkCopy =   
                                new SQLServerBulkCopy(connectionString))  
                        {  
                            bulkCopy.setBulkCopyOptions(copyOptions);  
                            bulkCopy.setDestinationTableName("dbo.BulkCopyDemoMatchingColumns");  
  
                            // Write from the source to the destination.   
                            // This should fail with a duplicate key error   
                            // after some of the batches have been copied.   
                            try  
                            {  
                                bulkCopy.writeToServer(rsSourceData);  
                            }  
                            catch (Exception e)  
                            {  
                                // Handle any errors that may have occurred.  
                                e.printStackTrace();  
                            }  
                        }  
                    }  
  
                    // Perform a final count on the destination    
                    // table to see how many rows were added.  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        long countEnd = rsRowCount.getInt(1);  
                        System.out.println("Ending row count = " + countEnd);  
                        System.out.println((countEnd - countStart) + " rows were added.");  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
        // Create a variable for the connection string.  
        String connectionUrl = "jdbc:sqlserver://localhost:1433;"  
                + "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
```  
  
### <a name="using-existing-transactions"></a>Utilisation de transactions existantes  
 Vous pouvez passer un objet Connection dont les transactions sont activées en tant que paramètre dans un constructeur SQLServerBulkCopy. Dans ce cas, l'opération de copie en bloc est effectuée dans une transaction existante, et aucune modification n'est apportée à l'état de la transaction (autrement dit, elle n'est ni validée ni abandonnée). Cela permet à une application d'inclure l'opération de copie en bloc dans une transaction avec d'autres opérations de base de données. Si vous devez restaurer toute l'opération de copie en bloc en raison d'une erreur, ou si la copie en bloc doit s'exécuter dans le cadre d'un processus plus vaste pouvant être restauré, vous pouvez effectuer la restauration de l'objet Connection à tout moment après l'opération de copie en bloc.  
  
 L'application suivante est similaire au premier exemple (non transactionnelle), à une exception près : dans cet exemple, l'opération de copie en bloc est incluse dans une transaction externe plus vaste. Quand l'erreur de violation de clé primaire se produit, toute la transaction est restaurée et aucune ligne n'est ajoutée à la table de destination.  
  
> [!NOTE]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#BKMK_TableSetup). Ce code est fourni pour illustrer la syntaxe pour l'utilisation de SQLServerBulkCopy uniquement. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d'utiliser une instruction Transact-SQL INSERT ... SELECT pour copier les données.  
  
```  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        try  
        {  
            // Note: if you are not using try-with-resources statements (as here),  
            // you must remember to call close() on any Connection, Statement,   
            // ResultSet, and SQLServerBulkCopy objects that you create.  
  
            // Open a sourceConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection sourceConnection = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = sourceConnection.createStatement())  
                {  
                    //  Delete all from the destination table.  
                    stmt.executeUpdate("DELETE FROM dbo.BulkCopyDemoMatchingColumns");  
  
                    //  Add a single row that will result in duplicate key            
                    //  when all rows from source are bulk copied.            
                    //  Note that this technique will only be successful in             
                    //  illustrating the point if a row with ProductID = 446              
                    //  exists in the AdventureWorks Production.Products table.             
                    //  If you have made changes to the data in this table, change            
                    //  the SQL statement in the code to add a ProductID that            
                    //  does exist in your version of the Production.Products            
                    //  table. Choose any ProductID in the middle of the table            
                    //  (not first or last row) to best illustrate the result.  
                    stmt.executeUpdate("SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns ON;" +  
                            "INSERT INTO " + "dbo.BulkCopyDemoMatchingColumns " +  
                            "([ProductID], [Name] ,[ProductNumber]) " +  
                            "VALUES(446, 'Lock Nut 23','LN-3416');" +  
                            "SET IDENTITY_INSERT dbo.BulkCopyDemoMatchingColumns OFF");  
  
                    // Perform an initial count on the destination table.  
                    long countStart = 0;  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        countStart = rsRowCount.getInt(1);  
                        System.out.println("Starting row count = " + countStart);  
                    }  
  
                    //  Get data from the source table as a ResultSet.  
                    try (ResultSet rsSourceData = stmt.executeQuery(  
                            "SELECT ProductID, Name, ProductNumber FROM Production.Product"))  
                    {  
  
                        // Set up the bulk copy object inside the transaction.  
                        try (Connection destinationConnection =   
                                DriverManager.getConnection(connectionString))  
                        {  
                            destinationConnection.setAutoCommit(false);  
  
                            SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();  
                            copyOptions.setKeepIdentity(true);  
                            copyOptions.setBatchSize(10);  
  
                            try (SQLServerBulkCopy bulkCopy =   
                                    new SQLServerBulkCopy(destinationConnection))  
                            {  
                                bulkCopy.setBulkCopyOptions(copyOptions);  
                                bulkCopy.setDestinationTableName("dbo.BulkCopyDemoMatchingColumns");  
  
                                // Write from the source to the destination.   
                                // This should fail with a duplicate key error.   
                                try  
                                {  
                                    bulkCopy.writeToServer(rsSourceData);  
                                    destinationConnection.commit();  
                                }  
                                catch (Exception e)  
                                {  
                                    // Handle any errors that may have occurred.  
                                    e.printStackTrace();  
                                    destinationConnection.rollback();  
                                }  
                            }  
                        }  
                    }  
  
                    // Perform a final count on the destination    
                    // table to see how many rows were added.  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        long countEnd = rsRowCount.getInt(1);  
                        System.out.println("Ending row count = " + countEnd);  
                        System.out.println((countEnd - countStart) + " rows were added.");  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
        // Create a variable for the connection string.  
        String connectionUrl = "jdbc:sqlserver://localhost:1433;"  
                + "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
  
```  
  
### <a name="bulk-copy-from-a-csv-file"></a>Copie en bloc à partir d'un fichier CSV  
 L'application suivante montre comment charger des données à l'aide de la classe SQLServerBulkCopy. Dans cet exemple, un fichier CSV est utilisé pour copier des données exportées de la table Production.Product de la base de données SQL Server AdventureWorks dans une table semblable de la base de données.  
  
> [!IMPORTANT]  
>  Cet exemple ne s’exécutera pas à moins que vous ayez créé les tables de travail comme décrit dans [le programme d’installation de Table](../../ssms/download-sql-server-management-studio-ssms.md) pour l’obtenir.  
  
1.  Ouvrez **SQL Server Management Studio** et se connecter à SQL Server avec la base de données AdventureWorks.  
  
2.  Développez les bases de données, cliquez avec le bouton droit sur la base de données AdventureWorks, sélectionnez **tâches** et **exporter les données**...  
  
3.  La Source de données, sélectionnez le **source de données** qui vous permet de se connecter à votre serveur SQL Server (par exemple, SQL Server Native Client 11.0), vérifiez la configuration, puis **suivant**  
  
4.  Pour la Destination, sélectionnez le **Destination de fichier plat** et entrez un **nom de fichier** avec une destination de C:\Test\TestBulkCSVExample.csv. Vérifiez que le **Format** est délimité, la **qualificateur de texte** a la valeur none et activer **des noms de colonnes dans la première ligne de données**, puis sélectionnez **suivant**  
  
5.  Sélectionnez **écrire une requête pour spécifier les données à transférer** et **suivant**.  Entrez votre **instruction SQL** sélectionnez ProductID, Name, ProductNumber de Production.Product et **suivant**  
  
6.  Vérifiez la configuration : vous pouvez laisser le séparateur de lignes en tant que {CR} {LF} et la virgule comme séparateur de colonne {,}.  Sélectionnez **modifier les mappages**... et vérifiez que les données **Type** est correct pour chaque colonne (par exemple, un entier pour ProductID et une chaîne Unicode pour les autres).  
  
7.  Passez à le **Terminer** et exécutez l’exportation.  
  
```  
  
import java.sql.*;  
  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCSVFileRecord;  
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;  
  
public class Program  
{  
    public static void main(String[] args)  
    {  
        String connectionString = GetConnectionString();  
        SQLServerBulkCSVFileRecord fileRecord = null;  
        try  
        {              
            // Get data from the source file by loading it into a class that implements ISQLServerBulkRecord.  
            // Here we are using the SQLServerBulkCSVFileRecord implementation to import the example CSV file.  
            fileRecord = new SQLServerBulkCSVFileRecord("C:\\Test\\TestBulkCSVExample.csv", true);      
  
            // Set the metadata for each column to be copied.  
            fileRecord.addColumnMetadata(1, null, java.sql.Types.INTEGER, 0, 0);  
            fileRecord.addColumnMetadata(2, null, java.sql.Types.NVARCHAR, 50, 0);  
            fileRecord.addColumnMetadata(3, null, java.sql.Types.NVARCHAR, 25, 0);  
  
            // Open a destinationConnection to the AdventureWorks database.   
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
            try (Connection destinationConnection = DriverManager.getConnection(connectionString))  
            {  
                try (Statement stmt = destinationConnection.createStatement())  
                {  
                    // Perform an initial count on the destination table.  
                    long countStart = 0;  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        countStart = rsRowCount.getInt(1);  
                        System.out.println("Starting row count = " + countStart);  
                    }  
  
                    // Set up the bulk copy object.    
                    // Note that the column positions in the source   
                    // data reader match the column positions in    
                    // the destination table so there is no need to   
                    // map columns.   
                    try (SQLServerBulkCopy bulkCopy =  
                               new SQLServerBulkCopy(destinationConnection))  
                    {  
                        bulkCopy.setDestinationTableName("dbo.BulkCopyDemoMatchingColumns");  
  
                        try  
                        {  
                            // Write from the source to the destination.  
                            bulkCopy.writeToServer(fileRecord);  
                        }  
                        catch (Exception e)  
                        {  
                            // Handle any errors that may have occurred.  
                            e.printStackTrace();  
                        }  
                    }  
  
                    // Perform a final count on the destination    
                    // table to see how many rows were added.  
                    try (ResultSet rsRowCount = stmt.executeQuery(  
                            "SELECT COUNT(*) FROM dbo.BulkCopyDemoMatchingColumns;"))  
                    {  
                        rsRowCount.next();  
                        long countEnd = rsRowCount.getInt(1);  
                        System.out.println("Ending row count = " + countEnd);  
                        System.out.println((countEnd - countStart) + " rows were added.");  
                    }  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            // Handle any errors that may have occurred.  
            e.printStackTrace();  
        }  
        finally  
        {  
            if (fileRecord != null) try { fileRecord.close(); } catch(Exception e) {}  
        }  
    }  
  
    // To avoid storing the sourceConnection String in your code,    
    // you can retrieve it from a configuration file.   
    private static String GetConnectionString()  
    {  
  
        // Create a variable for the connection string.  
        String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
           "databaseName=AdventureWorks;user=UserName;password=*****";  
  
        return connectionUrl;  
    }  
}  
  
```  
  
### <a name="bulk-copy-with-always-encrypted-columns"></a>Copie en bloc avec des colonnes toujours chiffrées  
 Copie en bloc à partir de Microsoft JDBC Driver 6.0 pour SQL Server, est pris en charge avec des colonnes toujours chiffrées.  
  
 Selon les options de copie en bloc et le chiffrement de type des tables source et de destination, le pilote JDBC peut déchiffrer de manière transparente et chiffrer ensuite les données, ou il peut envoyer les données chiffrées comme. Par exemple, lors de la copie en bloc des données à partir d’une colonne chiffrée à une colonne non chiffrée, le déchiffre de manière transparente données avant d’envoyer à SQL Server. De même lors de la copie en bloc des données à partir d’une colonne non chiffrée (ou à partir d’un fichier CSV) à une colonne chiffrée, le pilote chiffre en toute transparence les données avant d’envoyer à SQL Server. Si les source et destination est chiffrée, puis selon la **allowEncryptedValueModifications** en bloc à l’option de copie, le pilote envoie données est ou serait déchiffrer les données et chiffrer à nouveau avant d’envoyer à SQL Server.  
  
 Pour plus d’informations, consultez la **allowEncryptedValueModifications** en bloc option ci-dessous, de copie et [à l’aide d’Always Encrypted avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md).  
  
> [!IMPORTANT]  
>  Limitation du pilote Microsoft JDBC 6.0 pour SQL Server, si la copie d’un fichier CSV dans des colonnes chiffrées :  
>   
>  Uniquement le format Transact-SQL par défaut chaîne littéral est pris en charge pour les types de date et d’heure  
>   
>  Types de données DATETIME et SMALLDATETIME ne sont pas pris en charge.  
  
## <a name="bulk-copy-api-for-jdbc-driver"></a>API de copie en bloc pour le pilote JDBC  
  
### <a name="sqlserverbulkcopy"></a>SQLServerBulkCopy  
 Vous permet de charger efficacement en bloc une table SQL Server avec des données d'une autre source.  
  
 Microsoft SQL Server inclut un utilitaire de ligne de commande connu nommé bcp pour transférer les données d'une table vers une autre, sur un même serveur ou entre plusieurs serveurs. La classe SQLServerBulkCopy vous permet d'écrire des solutions de code en Java, qui fournissent des fonctionnalités similaires. Il existe d'autres façons de charger des données dans une table SQL Server (instructions INSERT, par exemple) mais SQLServerBulkCopy offre de meilleures performances.  
  
 La classe SQLServerBulkCopy peut être utilisée pour écrire des données uniquement dans des tables SQL Server. Toutefois, la source de données n'est pas limitée à SQL Server : toute source de données peut être utilisée, tant que les données sont lisibles avec une instance ResultSet ou une implémentation ISQLServerBulkRecord.  
  
|Constructeur| Description|  
|-----------------|-----------------|  
|SQLServerBulkCopy(Connection)|Initialise une nouvelle instance de la classe SQLServerBulkCopy à l’aide de l’instance ouverte spécifiée de SQLServerConnection. Si l'instance Connection a des transactions activées, les opérations de copie seront effectuées dans cette transaction.|  
|SQLServerBulkCopy (chaîne connectionURL)|Initialise et ouvre une nouvelle instance de SQLServerConnection en fonction de la valeur de connectionURL fournie. Le constructeur utilise SQLServerConnection pour initialiser une nouvelle instance de la classe SQLServerBulkCopy.|  
  
|Propriété| Description|  
|--------------|-----------------|  
|DestinationTableName de chaîne|Nom de la table de destination sur le serveur.<br /><br /> Si DestinationTableName n'a pas été défini à l'appel de writeToServer, une exception SQLServerException est levée.<br /><br /> DestinationTableName est un nom en trois parties (\<base de données >.\< schéma_propriétaire >. \<nom >). Vous pouvez qualifier le nom de la table avec sa base de données et son schéma propriétaire si vous le souhaitez. Toutefois, si le nom de la table utilise un trait de soulignement (« _ ») ou d'autres caractères spéciaux, vous devez isoler le nom en le plaçant entre crochets. Pour plus d'informations, consultez « Identificateurs » dans la documentation en ligne de SQL Server.|  
|ColumnMappings|Les mappages de colonnes définissent les relations entre les colonnes dans la source de données et dans la destination.<br /><br /> Si les mappages ne sont pas définis, les colonnes sont mappées implicitement selon la position ordinale. Pour que cela fonctionne, les schémas source et cible doivent correspondre. Si ce n’est pas le cas, une Exception est levée.<br /><br /> Si les mappages ne sont pas vides, il n'est pas nécessaire de spécifier toutes les colonnes présentes dans la source de données. Celles qui ne sont pas mappées sont ignorées.<br /><br /> Vous pouvez faire référence aux colonnes source et cible par nom ou par ordinal.|  
  
|Méthode| Description|  
|------------|-----------------|  
|Void addColumnMapping ((sourceColumn d’int, int destinationColumn)|Ajoute un nouveau mappage de colonnes, en utilisant des ordinaux pour spécifier les colonnes source et de destination.|  
|Void addColumnMapping ((int sourceColumn, destinationColumn de la chaîne)|Ajoute un nouveau mappage de colonnes, en utilisant un ordinal pour la colonne source et un nom de colonne pour la colonne de destination.|  
|Void addColumnMapping ((chaîne sourceColumn, int destinationColumn)|Ajoute un nouveau mappage de colonnes, en utilisant un nom de colonne pour décrire la colonne source et un ordinal pour spécifier la colonne de destination.|  
|Void addColumnMapping (sourceColumn de chaîne, chaîne destinationColumn)|Ajoute un nouveau mappage de colonnes, en utilisant des noms de colonne pour spécifier les colonnes source et de destination.|  
|Void clearColumnMappings()|Supprime le contenu des mappages de colonnes.|  
|Close() void|Ferme l’instance de SQLServerBulkCopy.|  
|SQLServerBulkCopyOptions getBulkCopyOptions()|Récupère l'ensemble actuel de SQLServerBulkCopyOptions.|  
|Chaîne getDestinationTableName()|Récupère le nom actuel de la table de destination.|  
|Void setBulkCopyOptions(SQLServerBulkCopyOptions copyOptions)|Met à jour le comportement de l'instance SQLServerBulkCopy en fonction des options fournies.|  
|Void setDestinationTableName(String tableName)|Définit le nom de la table de destination.|  
|Void writeToServer(ResultSet sourceData)|Copie toutes les lignes du ResultSet fourni dans une table de destination spécifiée par la propriété DestinationTableName de l’objet SQLServerBulkCopy.|  
|Void writeToServer(RowSet sourceData)|Copie toutes les lignes du RowSet fourni dans une table de destination spécifiée par la propriété DestinationTableName de l'objet SQLServerBulkCopy.|  
|Void writeToServer(ISQLServerBulkRecord sourceData)|Copie toutes les lignes dans l’implémentation ISQLServerBulkRecord fournie dans une table de destination spécifiée par la propriété DestinationTableName de l’objet SQLServerBulkCopy.|  
  
### <a name="sqlserverbulkcopyoptions"></a>SQLServerBulkCopyOptions  
 Collection de paramètres qui contrôlent le comportement des méthodes writeToServer dans une instance de SQLServerBulkCopy.  
  
|Constructeur| Description|  
|-----------------|-----------------|  
|SQLServerBulkCopyOptions()|Initialise une nouvelle instance de la classe SQLServerBulkCopyOptions à l’aide des valeurs par défaut pour tous les paramètres.|  
  
 Des accesseurs Get et Set existent pour les options suivantes :  
  
|Option| Description|Par défaut|  
|------------|-----------------|-------------|  
|CheckConstraints booléenne|Vérifie les contraintes pendant l'insertion des données.|False : les contraintes ne sont pas vérifiées|  
|FireTriggers booléenne|Si spécifié, entraîne l'exécution par le serveur des déclencheurs d'insertion pour les lignes insérées dans la base de données.|False : aucun déclencheur n'est exécuté|  
|KeepIdentity booléenne|Conserve les valeurs d'identité sources.|False : les valeurs d'identité sont attribuées par la destination|  
|KeepNulls booléenne|Conserve les valeurs null dans la table de destination, indépendamment des paramètres des valeurs par défaut.|False : les valeurs null sont remplacées par les valeurs par défaut, le cas échéant.|  
|TableLock booléenne|Obtient un verrou de mise à jour en bloc pour la durée de l'opération de copie en bloc.|False : les verrous de ligne sont utilisés.|  
|UseInternalTransaction booléenne|Si spécifié, chaque lot de l'opération de copie en bloc se produit dans une transaction. Si SQLServerBulkCopy utilise une connexion existante (comme spécifié par le constructeur), une exception SQLServerException se produit.  Si SQLServerBulkCopy a créé une connexion dédiée, une transaction est activée.|False : aucune transaction|  
|Int BatchSize|Nombre de lignes dans chaque lot. À la fin de chaque lot, les lignes du lot sont envoyées au serveur.<br /><br /> Un lot est terminé quand les lignes BatchSize ont été traitées ou qu'il n'y a plus de ligne à envoyer à la source de données de destination.  Si l'instance de SQLServerBulkCopy a été déclarée sans activer l'option UseInternalTransaction, une ligne BatchSize à la fois est envoyée au serveur, mais aucune action relative à la transaction n'est effectuée. Si UseInternalTransaction est activé, chaque lot de lignes est inséré comme une transaction distincte.|0 : indique que chaque opération writeToServer est un lot unique|  
|Int BulkCopyTimeout|Nombre de secondes pour terminer l'opération avant son expiration. La valeur 0 indique qu'il n'y a aucune limite : la copie en bloc attend indéfiniment.|60 secondes.|  
|AllowEncryptedValueModifications booléennes|Cette option est disponible avec Microsoft JDBC Driver 6.0 (ou version ultérieure) pour SQL Server.<br /><br /> Si spécifié, **allowEncryptedValueModifications** permet la copie en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrage des données. En règle générale, une application sélectionne les données des colonnes chiffrées d’une table sans déchiffrage des données (l’application se connecte à la base de données avec le mot-clé de paramètre de chiffrement de colonne défini sur désactivé) et puis utilisez cette option pour l’insertion en bloc les données, qui est toujours chiffré. Pour plus d’informations, consultez [à l’aide d’Always Encrypted avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md).<br /><br /> Soyez prudent lors de la spécification **allowEncryptedValueModifications** car cela peut endommager la base de données, car le pilote ne vérifie pas si les données sont réellement chiffrées ou si elle est correctement chiffrée à l’aide de la même chiffrement type, algorithme et clé que la colonne cible.||  
  
 Accesseurs Get et Set :  
  
|Méthodes| Description|  
|-------------|-----------------|  
|IsCheckConstraints() booléenne|Indique si les contraintes doivent être vérifiées pendant l’insertion des données ou non.|  
|Void setCheckConstraints(Boolean checkConstraints)|Définit si les contraintes doivent être vérifiées pendant l’insertion des données ou non.|  
|IsFireTriggers() booléenne|Indique si le serveur doit se déclencher les déclencheurs d’insertion pour les lignes insérées dans la base de données.|  
|Void setFireTriggers(Boolean fireTriggers)|Définit si le serveur doit être défini pour activer les déclencheurs pour les lignes insérées dans la base de données.|  
|IsKeepIdentity() booléenne|Indique ou non conserver les valeurs d’identité sources.|  
|Void setKeepIdentity(Boolean keepIdentity)|Définit ou non conserver les valeurs d’identité.|  
|IsKeepNulls() booléenne|Indique s’il faut conserver les valeurs null dans la table de destination, indépendamment des paramètres pour les valeurs par défaut, ou si elles doivent être remplacés par les valeurs par défaut (le cas échéant).|  
|Void setKeepNulls(Boolean keepNulls)|Définit s’il faut conserver les valeurs null dans la table de destination, indépendamment des paramètres pour les valeurs par défaut, ou si elles doivent être remplacés par les valeurs par défaut (le cas échéant).|  
|IsTableLock() booléenne|Indique si SQLServerBulkCopy doit obtenir un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc.|  
|Void setTableLock(Boolean tableLock)|Définit si SQLServerBulkCopy doit obtenir un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc.|  
|IsUseInternalTransaction() booléenne|Indique si chaque lot de l’opération de copie en bloc aura lieu dans une transaction.|  
|Void setUseInternalTranscation(Boolean useInternalTransaction)|Définit si chaque lot des opérations de copie en bloc aura lieu dans une transaction ou non.|  
|Int getBatchSize()|Obtient le nombre de lignes dans chaque lot. À la fin de chaque lot, les lignes dans le lot sont envoyées au serveur|  
|Void setBatchSize(int batchSize)|Définit le nombre de lignes dans chaque lot. À la fin de chaque lot, les lignes du lot sont envoyées au serveur.|  
|Int getBulkCopyTimeout()|Obtient le nombre de secondes pour l’opération à effectuer avant d’expirer.|  
|Void setBulkCopyTimeout(int timeout)|Définit le nombre de secondes pour l’opération avant son expiration.|  
|isAllowEncryptedValueModifications() booléenne|Indique si le paramètre d’allowEncryptedValueModifications est activé ou désactivé.|  
|void setAllowEncryptedValueModifications(boolean allowEncryptedValueModifications)|Configure le paramètre allowEncryptedValueModifications, qui est utilisé pour la copie en bloc avec des colonnes toujours chiffrées.|  
  
### <a name="isqlserverbulkrecord"></a>ISQLServerBulkRecord  
 L'interface ISQLServerBulkRecord peut être utilisée pour créer des classes qui lisent les données de n'importe quelle source (par exemple un fichier) et autorisent une instance de SQLServerBulkCopy à charger en bloc une table SQL Server avec ces données.  
  
|Méthodes d'interface| Description|  
|-----------------------|-----------------|  
|Définissez\<entier > getColumnOrdinals()|Obtient les ordinaux de chacune des colonnes représentées dans cet enregistrement de données.|  
|Chaîne getColumnName(int column)|Obtient le nom de la colonne spécifiée.|  
|Int getColumnType (colonne int)|Obtient le type de données JDBC de la colonne spécifiée.|  
|Int getPrecision (colonne int)|Obtient la précision de la colonne spécifiée.|  
|Objet [] getRowData()|Obtient les données de la ligne active en tant que tableau d'objets.<br /><br /> Chaque objet doit correspondre au type de langage Java utilisé pour représenter le type de données JDBC indiqué pour la colonne spécifiée.  Pour plus d'informations, consultez « Présentation des types de données du pilote JDBC » pour les mappages appropriés.|  
|Int getScale (colonne int)|Obtient l'échelle de la colonne spécifiée.|  
|Boolean isAutoIncrement (colonne int)|Indique si la colonne représente une colonne d'identité.|  
|Next() booléenne|Accède à la ligne de données suivante.|  
  
### <a name="sqlserverbulkcsvfilerecord"></a>SQLServerBulkCSVFileRecord  
 Implémentation simple de l'interface ISQLServerBulkRecord qui peut être utilisée pour lire les types de données Java de base à partir d'un fichier délimité, où chaque ligne représente une ligne de données.  
  
 Notes et limitations de l'implémentation :  
  
1.  la quantité maximale de données autorisées dans une ligne donnée est limitée par la mémoire disponible, car les données sont lues une ligne à la fois.  
  
2.  La diffusion en continu de types de données volumineux comme varchar(max), varbinary(max), nvarchar(max), sqlxml, et ntext n'est pas prise en charge.  
  
3.  Le séparateur spécifié pour le fichier CSV ne doit pas apparaître n'importe où dans les données et doit être placé correctement dans une séquence d'échappement s'il s'agit d'un caractère restreint dans les expressions Java régulières.  
  
4.  Dans l'implémentation de fichier CSV, les guillemets doubles sont traités comme faisant partie des données. Par exemple, la ligne hello,”world”,”hello,world” est considérée comme ayant quatre colonnes avec les valeurs hello, “world”, “hello et world” si le séparateur est une virgule.  
  
5.  Les nouveaux caractères de ligne sont utilisés comme indicateurs de fin de ligne et ne sont pas autorisés n'importe où dans les données.  
  
|Constructeur| Description|  
|-----------------|-----------------|  
|SQLServerBulkCSVFileRecord (String fileToParse, encodage de chaîne, un délimiteur de chaîne, booléen firstLineIsColumnNamesSQLServerBulkCSVFileRecord (String, String, String, boolean)|Initialise une nouvelle instance de la classe SQLServerBulkCSVFileRecord qui analyse chaque ligne du fileToParse avec le séparateur et le codage fournis. Si firstLineIsColumnNames a la valeur True, la première ligne du fichier est analysée en tant que noms de colonne.  Si le codage est NULL, le codage par défaut est utilisé.|  
|SQLServerBulkCSVFileRecord (String fileToParse, chaîne de codage, Boolean firstLineIsColumnNamesSQLServerBulkCSVFileRecord (String, String, boolean)|Initialise une nouvelle instance de la classe SQLServerBulkCSVFileRecord qui analyse chaque ligne de fileToParse avec une virgule comme séparateur et le codage fourni. Si firstLineIsColumnNames a la valeur True, la première ligne du fichier est analysée en tant que noms de colonne.  Si le codage est NULL, le codage par défaut est utilisé.|  
|SQLServerBulkCSVFileRecord (String fileToParse, Boolean firstLineIsColumnNamesSQLServerBulkCSVFileRecord (String, boolean)|Initialise une nouvelle instance de la classe SQLServerBulkCSVFileRecord qui analyse chaque ligne de fileToParse avec une virgule comme séparateur et l'encodage par défaut. Si firstLineIsColumnNames a la valeur True, la première ligne dans le fichier sera être analysée en tant que noms de colonnes.|  
  
|Méthode| Description|  
|------------|-----------------|  
|Void addColumnMetadata (int positionInFile, nom de colonne de chaîne, jdbcType d’int, int précision, échelle d’int)|Ajoute des métadonnées pour la colonne indiquée dans le fichier.|  
|Close() void|Libère les ressources associées au lecteur de fichier.|  
|Void setTimestampWithTimezoneFormat (DateTim eFormatter dateTimeFormatter|Définit le format pour l'analyse des données d'horodatage du fichier en tant que java.sql.Types.TIMESTAMP_WITH_TIMEZONE.|  
|Void setTimestampWithTimezoneFormat(String dateTimeFormat)setTimeWithTimezoneFormat(DateTimeFormatter)|Définit le format pour l'analyse des données d'heure du fichier en tant que java.sql.Types.TIME_WITH_TIMEZONE.|  
|Void setTimeWithTimezoneFormat (dateTimeFormatter d’atter DateTimeForm)|Définit le format pour l'analyse des données d'heure du fichier en tant que java.sql.Types.TIME_WITH_TIMEZONE.|  
|Void setTimeWithTimezoneFormat(String timeFormat)|Définit le format pour l'analyse des données d'heure du fichier en tant que java.sql.Types.TIME_WITH_TIMEZONE.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
