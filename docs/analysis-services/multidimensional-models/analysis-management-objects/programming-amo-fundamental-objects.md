---
title: Programmation d’objets fondamentaux AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7d1a41e3682023862c69cc74bf961f279f99689e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="programming-amo-fundamental-objects"></a>Programmation d'objets fondamentaux AMO
  Les objets fondamentaux sont généralement des objets simples et rudimentaires. En règle générale, ces objets sont créés et instanciés puis, une fois qu'ils n'ont plus d'utilité, l'utilisateur s'en déconnecte. Les classes fondamentales se composent des objets suivants : <xref:Microsoft.AnalysisServices.Server>, <xref:Microsoft.AnalysisServices.Database>, <xref:Microsoft.AnalysisServices.DataSource> et <xref:Microsoft.AnalysisServices.DataSourceView>. Parmi les objets fondamentaux AMO, le seul objet complexe est <xref:Microsoft.AnalysisServices.DataSourceView>. Celui-ci exige de nombreux détails pour générer le modèle abstrait qui représente la vue de source de données.  
  
 Les objets <xref:Microsoft.AnalysisServices.Server> et <xref:Microsoft.AnalysisServices.Database> doivent généralement utiliser les objets contenus en tant qu'objets OLAP ou objets d'exploration de données.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Objets serveur](#ServerObjects)  
  
-   [Objets d’Exception AMOException](#AMO)  
  
-   [Objets de base de données](#DatabaseObjects)  
  
-   [Objets de source de données](#DataSource)  
  
-   [Objets DataSourceView](#DSV)  
  
##  <a name="ServerObjects"></a> Objets serveur  
 L'utilisation d'un objet <xref:Microsoft.AnalysisServices.Server> passe par les étapes suivantes : connexion au serveur, vérification que l'objet <xref:Microsoft.AnalysisServices.Server> est bien connecté au serveur et, le cas échéant, déconnexion du <xref:Microsoft.AnalysisServices.Server> du serveur.  
  
### <a name="connecting-to-the-server-object"></a>Connexion à l'objet Server  
 Se connecter au serveur revient à disposer de la chaîne de connexion adéquate.  
  
 Le code suivant exemple retourne un <xref:Microsoft.AnalysisServices.Server> si la connexion a réussi, ou retourne l’objet **null** si une erreur se produit. Les erreurs qui se produisent lors du processus de connexion sont gérées dans une construction try/catch. Les erreurs AMO sont interceptées à l'aide de la classe d'exception <xref:Microsoft.AnalysisServices.AmoException>. Dans cet exemple, l'erreur est révélée à l'utilisateur dans un message.  
  
```  
static Server ServerConnect( String strStringConnection)  
{  
    string methodCaption = "ServerConnect method";  
    Server svr = new Server();  
    try  
    {  
        svr.Connect(strStringConnection);  
    }  
    #region ErrorHandling  
    catch (AmoException e)  
    {  
        MessageBox.Show( "AMO exception " + e.ToString());  
        svr = null;  
    }  
    catch (Exception e)  
    {  
        MessageBox.Show("General exception " + e.ToString());  
        svr = null;  
    }  
    #endregion  
  
    return svr;  
}  
```  
  
 La chaîne de connexion présente la structure suivante :  
  
 «**Source de données =**\<nom du serveur > ».  
  
 Pour plus d'informations sur la chaîne de connexion, consultez <xref:Microsoft.SqlServer.Management.Common.OlapConnectionInfo.ConnectionString%2A>.  
  
### <a name="validating-the-connection"></a>Validation de la connexion  
 Avant de programmer les objets <xref:Microsoft.AnalysisServices.Server>, vous devez vérifier que vous êtes toujours connecté au serveur. L'exemple de code suivant montre comment procéder. L'exemple suppose que `svr` est un objet <xref:Microsoft.AnalysisServices.Server> qui figure dans votre code.  
  
```  
if ( (svr != null) && ( svr.Connected))  
{  
    // Do what it is needed if connection is good  
}  
```  
  
### <a name="disconnecting-from-the-server"></a>Déconnexion du serveur  
 Une fois que vous avez terminé, vous pouvez vous déconnecter du serveur en utilisant la méthode Disconnect. L'exemple de code suivant montre comment procéder. L'exemple suppose que `svr` est un objet <xref:Microsoft.AnalysisServices.Server> qui figure dans votre code.  
  
```  
if ( (svr != null) && ( svr.Connected))  
{  
    svr.Disconnect()  
}  
```  
  
###  <a name="AMO"></a> Objets d’Exception AmoException  
 AMO lève des exceptions lorsque certains problèmes sont rencontrés. Pour obtenir une explication détaillée des exceptions, consultez [méthodes et Classes autres AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-other-classes-and-methods.md). L'exemple de code suivant présente la méthode à employer pour capturer des exceptions dans AMO :  
  
```  
try  
{  
   //... some AMO code in here  
}  
  
catch (  OutOfSynchException e)  
{  
   // error handling code for OutOfSynchException   
}  
  
catch (  OperationException e)  
{  
   // error handling code for OperationException   
}   
  
catch (  ResponseFormatException e)  
  
{  
   // error handling code for ResponseFormatException   
}   
  
catch (  ConnectionException e)  
  
{  
   // error handling code for ConnectionException   
}  
  
catch (  AMOException e)  
  
{  
   //... here is the place where you end if it is an AMO exception, but none of the previous exceptions  
   // if you start with AMOException in the first catch you will never see any one of the previous exceptions  
}  
```  
  
##  <a name="DatabaseObjects"></a> Objets de base de données  
 L'utilisation d'un objet <xref:Microsoft.AnalysisServices.Database> est très simple et directe. Vous obtenez une base de données existante à partir de la collection de bases de données de l'objet <xref:Microsoft.AnalysisServices.Server>.  
  
### <a name="creating-dropping-and-finding-a-database"></a>Création, suppression et recherche d'une base de données  
 L'exemple de code suivant indique comment créer une base de données à partir d'un nom de base de données. Avant de créer la base de données, interrogez l'objet <xref:Microsoft.AnalysisServices.DatabaseCollection> du serveur pour déterminer si la base de données existe. Si elle existe, elle est supprimée puis créée ; si elle n'existe pas, elle est créée. Si la base de données doit être supprimée, elle est d'abord acquise auprès de la collection de bases de données.  
  
```  
static Database CreateDatabase(Server svr, String DatabaseName)  
{  
    Database db = null;  
    if ( (svr != null) && ( svr.Connected))  
    {  
        // Drop the database if it already exists  
        db = svr.Databases.FindByName(DatabaseName);  
        if (db != null)  
        {  
            db.Drop();  
        }  
  
        // Create the database  
        db = svr.Databases.Add(DatabaseName);  
        db.Update();  
    }  
  
    return db;  
}  
```  
  
 Pour déterminer si une base de données existe dans la collection de bases de données, il convient d'utiliser la méthode FindByName. Si la base de données existe, la méthode retourne l'objet base de données trouvé. Dans le cas contraire, elle retourne un objet null.  
  
 Dès que l'objet <xref:Microsoft.AnalysisServices.Database> est ajouté à la collection de bases de données, le serveur doit être mis à jour par le biais de sa méthode Update. En cas d'échec de la mise à jour du serveur, l'objet <xref:Microsoft.AnalysisServices.Database> n'est pas créé dans le serveur.  
  
### <a name="processing-a-database"></a>Traitement d'une base de données  
 Le traitement d'une base de données, avec tous les objets enfants associés, s'avère très simple du fait de la présence d'une méthode Process dans l'objet <xref:Microsoft.AnalysisServices.Database>.  
  
 La méthode Process peut inclure des paramètres, mais ceux-ci ne sont pas obligatoires. Si aucun paramètre n’est spécifié, tous les objets enfants sont traités avec leur **ProcessDefault** option. Pour plus d’informations sur les options de traitement, consultez <xref:Microsoft.AnalysisServices.Database>.  
  
1.  L'exemple de code suivant traite une base de données d'après sa valeur par défaut.  
  
```  
static Database ProcessDatabase(Database db, ProcessType pt)  
{  
    db.Process( pt);  
    return db;  
}  
```  
  
##  <a name="DataSource"></a>Objets de source de données  
 Un objet <xref:Microsoft.AnalysisServices.DataSource> est le lien entre [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] et la base de données où résident les données. Le schéma qui représente le modèle sous-jacent pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] est défini par l'objet <xref:Microsoft.AnalysisServices.DataSourceView>. Un objet <xref:Microsoft.AnalysisServices.DataSource> peut être envisagé comme une chaîne de connexion à la base de données où résident les données.  
  
 L'exemple de code suivant montre comment créer un objet <xref:Microsoft.AnalysisServices.DataSource>. L'exemple vérifie que le serveur existe toujours, que l'objet <xref:Microsoft.AnalysisServices.Server> est connecté et que la base de données existe. Si l'objet <xref:Microsoft.AnalysisServices.DataSource> existe, il est supprimé et recréé. L'objet <xref:Microsoft.AnalysisServices.DataSource> est créé avec les mêmes nom et ID interne. Dans cet exemple, aucune vérification n'est effectuée au niveau de la chaîne de connexion.  
  
```  
static string CreateDataSource(Database db, string strDataSourceName, string strConnectionString)  
{  
        Server svr = db.Parent;  
        DataSource ds = db.DataSources.FindByName(strDataSourceName);  
        if (ds != null)  
            ds.Drop();  
        // Create the data source  
        ds = db.DataSources.Add(strDataSourceName, strDataSourceName);  
        ds.ConnectionString = strConnectionString;  
  
        // Send the data source definition to the server.  
        ds.Update();  
  
        return ds.Name;  
}  
```  
  
##  <a name="DSV"></a> Objets DataSourceView  
 L'objet <xref:Microsoft.AnalysisServices.DataSourceView> est chargé de conserver le modèle de schéma pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Pour que l'objet <xref:Microsoft.AnalysisServices.DataSourceView> conserve le schéma, ce dernier doit d'abord être construit. Les schémas sont construits sur les objets DataSet, à partir de l'espace de noms System.Data.  
  
 L'exemple de code suivant crée une partie du schéma inclus dans l'exemple de projet Analysis Services basé sur AdventureWorks. L'exemple crée des définitions de schéma pour les tables, les colonnes calculées, les relations et les relations composites. Les schémas sont des jeux de données persistantes.  
  
 L'exemple de code effectue les opérations suivantes :  
  
1.  Création d'un objet <xref:Microsoft.AnalysisServices.DataSourceView>.  
  
     Vérifiez tout d’abord si le <xref:Microsoft.AnalysisServices.DataSource> objet existe ; si **true**, puis supprimez le <xref:Microsoft.AnalysisServices.DataSource> et créez-le. Si <xref:Microsoft.AnalysisServices.DataSource> n'existe pas, créez-le.  
  
2.  Ouverture d'une connexion à la base de données à l'aide de la chaîne de connexion <xref:Microsoft.AnalysisServices.DataSource>.  
  
3.  Création du schéma.  
  
     Le schéma se compose des éléments suivants :  
  
    -   une définition de table, méthode `AddTable()` ;  
  
    -   un ensemble facultatif de colonnes calculées, méthode `AddComputedColumn()` ;  
  
    -   un ensemble facultatif de relations, `AddRelation` ;  
  
    -   un ensemble facultatif de relations composites, `AddCompositeRelations`.  
  
4.  Mise à jour du serveur.  
  
> [!NOTE]  
>  L'exemple de code suivant a été abrégé dans un souci de lisibilité ; le code complet est inclus à la fin de cette rubrique.  
  
> [!NOTE]  
>  L'exemple de code comprend les méthodes suivantes : `AddTable`, `AddComputedColumn`, `AddRelation` et `AddCompositeRelation`.  
  
> [!NOTE]  
>  La clause `'WHERE 1=0'` consiste à empêcher la requête de retourner des lignes à la **DataSet** objet.  
  
```  
        static DataSourceView CreateDataSourceView(Database db, string strDataSourceName)  
        {  
            // Create the data source view  
            DataSourceView dsv = db.DataSourceViews.FindByName(strDataSourceName);  
            if ( dsv != null)  
               dsv.Drop();  
            dsv = db.DataSourceViews.Add(strDataSourceName);  
            dsv.DataSourceID = strDataSourceName;  
            dsv.Schema = new DataSet();  
            dsv.Schema.Locale = CultureInfo.CurrentCulture;  
  
            // Open a connection to the data source  
            OleDbConnection connection  
                = new OleDbConnection(dsv.DataSource.ConnectionString);  
            connection.Open();  
  
            #region Create tables  
  
            // Add the DimTime table  
            AddTable(dsv, connection, "DimTime");  
            AddComputedColumn(dsv, connection, "DimTime", "SimpleDate", "DATENAME(mm, FullDateAlternateKey) + ' ' + DATENAME(dd, FullDateAlternateKey) + ',' + ' ' + DATENAME(yy, FullDateAlternateKey)");  
  
            // Add the DimProductCategory table  
            AddTable(dsv, connection, "DimProductCategory");  
  
            // Add the DimProductSubcategory table  
            AddTable(dsv, connection, "DimProductSubcategory");  
            AddRelation(dsv, "DimProductSubcategory", "ProductCategoryKey", "DimProductCategory", "ProductCategoryKey");  
  
            // Add the FactInternetSales table  
            AddTable(dsv, connection, "FactInternetSales");  
"DimTime", "TimeKey");  
            AddRelation(dsv, "FactInternetSales", "ShipDateKey", "DimTime", "TimeKey");  
            AddRelation(dsv, "FactInternetSales", "DueDateKey", "DimTime", "TimeKey");  
  
            // Add the FactInternetSalesReason table  
            AddTable(dsv, connection, "FactInternetSalesReason");  
            AddCompositeRelation(dsv, "FactInternetSalesReason", "FactInternetSales", "SalesOrderNumber", "SalesOrderLineNumber");  
            dsv.Update();  
  
            #endregion  
  
            // Send the data source view definition to the server  
            dsv.Update();  
  
            return dsv;  
        }  
  
        static void AddTable(DataSourceView dsv, OleDbConnection connection, String tableName)  
        {  
            string strSelectText = "SELECT * FROM [dbo].[" + tableName + "] WHERE 1=0";  
            OleDbDataAdapter adapter = new OleDbDataAdapter(strSelectText, connection);  
            DataTable[] dataTables = adapter.FillSchema(dsv.Schema,  
                SchemaType.Mapped, tableName);  
            DataTable dataTable = dataTables[0];  
  
            dataTable.ExtendedProperties.Add("TableType", "Table");  
            dataTable.ExtendedProperties.Add("DbSchemaName", "dbo");  
            dataTable.ExtendedProperties.Add("DbTableName", tableName);  
            dataTable.ExtendedProperties.Add("FriendlyName", tableName);  
  
            dataTable = null;  
            dataTables = null;  
            adapter = null;  
        }  
  
        static void AddComputedColumn(DataSourceView dsv, OleDbConnection connection, String tableName, String computedColumnName, String expression)  
        {  
            DataSet tmpDataSet = new DataSet();  
            tmpDataSet.Locale = CultureInfo.CurrentCulture;  
            OleDbDataAdapter adapter = new OleDbDataAdapter("SELECT ("  
                + expression + ") AS [" + computedColumnName + "] FROM [dbo].["  
                + tableName + "] WHERE 1=0", connection);  
            DataTable[] dataTables = adapter.FillSchema(tmpDataSet,  
                SchemaType.Mapped, tableName);  
            DataTable dataTable = dataTables[0];  
            DataColumn dataColumn = dataTable.Columns[computedColumnName];  
  
            dataTable.Constraints.Clear();  
            dataTable.Columns.Remove(dataColumn);  
  
            dataColumn.ExtendedProperties.Add("DbColumnName", computedColumnName);  
            dataColumn.ExtendedProperties.Add("ComputedColumnExpression",  
                expression);  
            dataColumn.ExtendedProperties.Add("IsLogical", "True");  
  
            dsv.Schema.Tables[tableName].Columns.Add(dataColumn);  
  
            dataColumn = null;  
            dataTable = null;  
            dataTables = null;  
            adapter = null;  
            tmpDataSet = null;  
        }  
  
        static void AddRelation(DataSourceView dsv, String fkTableName, String fkColumnName, String pkTableName, String pkColumnName)  
        {  
            DataColumn fkColumn  
                = dsv.Schema.Tables[fkTableName].Columns[fkColumnName];  
            DataColumn pkColumn  
                = dsv.Schema.Tables[pkTableName].Columns[pkColumnName];  
            dsv.Schema.Relations.Add("FK_" + fkTableName + "_"  
                + fkColumnName, pkColumn, fkColumn, true);  
        }  
  
        static void AddCompositeRelation(DataSourceView dsv, String fkTableName, String pkTableName, String columnName1, String columnName2)  
        {  
            DataColumn[] fkColumns = new DataColumn[2];  
            fkColumns[0] = dsv.Schema.Tables[fkTableName].Columns[columnName1];  
            fkColumns[1] = dsv.Schema.Tables[fkTableName].Columns[columnName2];  
  
            DataColumn[] pkColumns = new DataColumn[2];  
            pkColumns[0] = dsv.Schema.Tables[pkTableName].Columns[columnName1];  
            pkColumns[1] = dsv.Schema.Tables[pkTableName].Columns[columnName2];  
  
            dsv.Schema.Relations.Add("FK_" + fkTableName + "_" + columnName1  
                + "_" + columnName2, pkColumns, fkColumns, true);  
        }  
  
```  
  
 Dans l’exemple de code, la `AddTable` et `AddComputedColumn` méthodes utilisent le `FillSchema` méthode de la **DataAdapter** objet à ajouter un **DataTable** à un **DataSet** et de configurer le schéma à celui de la source de données. Les propriétés étendues ajoutent les informations nécessaires à la configuration du schéma pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 Dans l'exemple de code, les méthodes `AddRelation` et `AddCompositeRelation` ajoutent les colonnes de relation en fonction du schéma existant et des colonnes existantes dans le modèle. Pour que ces méthodes fonctionnent, les colonnes doivent faire partie des tables définies dans le schéma.  
  
 Voici l'exemple de code dans sa version complète :  
  
```  
static DataSourceView CreateDataSourceView(Database db, string strDataSourceName)  
{  
    // Create the data source view  
    DataSourceView dsv = db.DataSourceViews.FindByName(strDataSourceName);  
    if ( dsv != null)  
       dsv.Drop();  
    dsv = db.DataSourceViews.Add(strDataSourceName);  
    dsv.DataSourceID = strDataSourceName;  
    dsv.Schema = new DataSet();  
    dsv.Schema.Locale = CultureInfo.CurrentCulture;  
  
    // Open a connection to the data source  
    OleDbConnection connection  
        = new OleDbConnection(dsv.DataSource.ConnectionString);  
    connection.Open();  
  
    #region Create tables  
  
    // Add the DimTime table  
    AddTable(dsv, connection, "DimTime");  
    AddComputedColumn(dsv, connection, "DimTime", "SimpleDate", "DATENAME(mm, FullDateAlternateKey) + ' ' + DATENAME(dd, FullDateAlternateKey) + ',' + ' ' + DATENAME(yy, FullDateAlternateKey)");  
    AddComputedColumn(dsv, connection, "DimTime", "CalendarYearDesc", "'CY' + ' ' + CalendarYear");  
    AddComputedColumn(dsv, connection, "DimTime", "CalendarSemesterDesc", "CASE WHEN CalendarSemester = 1 THEN 'H1'+' '+ 'CY' +' '+ CONVERT(CHAR (4), CalendarYear) ELSE 'H2'+' '+ 'CY' +' '+ CONVERT(CHAR (4), CalendarYear) END");  
    AddComputedColumn(dsv, connection, "DimTime", "CalendarQuarterDesc", "'Q' + CONVERT(CHAR (1), CalendarQuarter) +' '+ 'CY' +' '+ CONVERT(CHAR (4), CalendarYear)");  
    AddComputedColumn(dsv, connection, "DimTime", "MonthName", "EnglishMonthName+' '+ CONVERT(CHAR (4), CalendarYear)");  
    AddComputedColumn(dsv, connection, "DimTime", "FiscalYearDesc", "'FY' + ' ' + FiscalYear");  
    AddComputedColumn(dsv, connection, "DimTime", "FiscalSemesterDesc", "CASE WHEN FiscalSemester = 1 THEN 'H1'+' '+ 'FY' +' '+ CONVERT(CHAR (4), FiscalYear) ELSE 'H2'+' '+ 'FY' +' '+ CONVERT(CHAR (4), FiscalYear) END");  
    AddComputedColumn(dsv, connection, "DimTime", "FiscalQuarterDesc", "'Q' + CONVERT(CHAR (1), FiscalQuarter) +' '+ 'FY' +' '+ CONVERT(CHAR (4), FiscalYear)");  
    AddComputedColumn(dsv, connection, "DimTime", "FiscalMonthNumberOfYear", "CASE WHEN MonthNumberOfYear = '1'  THEN CONVERT(int,'7') WHEN MonthNumberOfYear = '2'  THEN CONVERT(int,'8') WHEN MonthNumberOfYear = '3'  THEN CONVERT(int,'9') WHEN MonthNumberOfYear = '4'  THEN CONVERT(int,'10') WHEN MonthNumberOfYear = '5'  THEN CONVERT(int,'11') WHEN MonthNumberOfYear = '6'  THEN CONVERT(int,'12') WHEN MonthNumberOfYear = '7'  THEN CONVERT(int,'1') WHEN MonthNumberOfYear = '8'  THEN CONVERT(int,'2') WHEN MonthNumberOfYear = '9'  THEN CONVERT(int,'3') WHEN MonthNumberOfYear = '10' THEN CONVERT(int,'4') WHEN MonthNumberOfYear = '11' THEN CONVERT(int,'5') WHEN MonthNumberOfYear = '12' THEN CONVERT(int,'6') END");  
    dsv.Update();  
  
    // Add the DimGeography table  
    AddTable(dsv, connection, "DimGeography");  
  
    // Add the DimProductCategory table  
    AddTable(dsv, connection, "DimProductCategory");  
  
    // Add the DimProductSubcategory table  
    AddTable(dsv, connection, "DimProductSubcategory");  
    AddRelation(dsv, "DimProductSubcategory", "ProductCategoryKey", "DimProductCategory", "ProductCategoryKey");  
  
    // Add the DimProduct table  
    AddTable(dsv, connection, "DimProduct");  
    AddComputedColumn(dsv, connection, "DimProduct", "ProductLineName", "CASE ProductLine WHEN 'M' THEN 'Mountain' WHEN 'R' THEN 'Road' WHEN 'S' THEN 'Accessory' WHEN 'T' THEN 'Touring' ELSE 'Components' END");  
    AddRelation(dsv, "DimProduct", "ProductSubcategoryKey", "DimProductSubcategory", "ProductSubcategoryKey");  
    dsv.Update();  
  
    // Add the DimCustomer table  
    AddTable(dsv, connection, "DimCustomer");  
    AddComputedColumn(dsv, connection, "DimCustomer", "FullName", "CASE WHEN MiddleName IS NULL THEN FirstName + ' ' + LastName ELSE FirstName + ' ' + MiddleName + ' ' + LastName END");  
    AddComputedColumn(dsv, connection, "DimCustomer", "GenderDesc", "CASE WHEN Gender = 'M' THEN 'Male' ELSE 'Female' END");  
    AddComputedColumn(dsv, connection, "DimCustomer", "MaritalStatusDesc", "CASE WHEN MaritalStatus = 'S' THEN 'Single' ELSE 'Married' END");  
    AddRelation(dsv, "DimCustomer", "GeographyKey", "DimGeography", "GeographyKey");  
  
    // Add the DimReseller table  
    AddTable(dsv, connection, "DimReseller");  
    AddComputedColumn(dsv, connection, "DimReseller", "OrderFrequencyDesc", "CASE WHEN OrderFrequency = 'A' THEN 'Annual' WHEN OrderFrequency = 'S' THEN 'Bi-Annual' ELSE 'Quarterly' END");  
    AddComputedColumn(dsv, connection, "DimReseller", "OrderMonthDesc", "CASE WHEN OrderMonth = '1' THEN 'January' WHEN OrderMonth = '2' THEN 'February' WHEN OrderMonth = '3' THEN 'March' WHEN OrderMonth = '4' THEN 'April' WHEN OrderMonth = '5' THEN 'May' WHEN OrderMonth = '6' THEN 'June' WHEN OrderMonth = '7' THEN 'July' WHEN OrderMonth = '8' THEN 'August' WHEN OrderMonth = '9' THEN 'September' WHEN OrderMonth = '10' THEN 'October' WHEN OrderMonth = '11' THEN 'November' WHEN OrderMonth = '12' THEN 'December' ELSE 'Never Ordered' END");  
  
    // Add the DimCurrency table  
    AddTable(dsv, connection, "DimCurrency");  
    dsv.Update();  
  
    // Add the DimSalesReason table  
    AddTable(dsv, connection, "DimSalesReason");  
  
    // Add the FactInternetSales table  
    AddTable(dsv, connection, "FactInternetSales");  
    AddRelation(dsv, "FactInternetSales", "ProductKey", "DimProduct", "ProductKey");  
    AddRelation(dsv, "FactInternetSales", "CustomerKey", "DimCustomer", "CustomerKey");  
    AddRelation(dsv, "FactInternetSales", "OrderDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactInternetSales", "ShipDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactInternetSales", "DueDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactInternetSales", "CurrencyKey", "DimCurrency", "CurrencyKey");  
    dsv.Update();  
  
    // Add the FactResellerSales table  
    AddTable(dsv, connection, "FactResellerSales");  
    AddRelation(dsv, "FactResellerSales", "ProductKey", "DimProduct", "ProductKey");  
    AddRelation(dsv, "FactResellerSales", "ResellerKey", "DimReseller", "ResellerKey");  
    AddRelation(dsv, "FactResellerSales", "OrderDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactResellerSales", "ShipDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactResellerSales", "DueDateKey", "DimTime", "TimeKey");  
    AddRelation(dsv, "FactResellerSales", "CurrencyKey", "DimCurrency", "CurrencyKey");  
  
    // Add the FactInternetSalesReason table  
    AddTable(dsv, connection, "FactInternetSalesReason");  
    AddCompositeRelation(dsv, "FactInternetSalesReason", "FactInternetSales", "SalesOrderNumber", "SalesOrderLineNumber");  
    dsv.Update();  
  
    // Add the FactCurrencyRate table  
    AddTable(dsv, connection, "FactCurrencyRate");  
    AddRelation(dsv, "FactCurrencyRate", "CurrencyKey", "DimCurrency", "CurrencyKey");  
    AddRelation(dsv, "FactCurrencyRate", "TimeKey", "DimTime", "TimeKey");  
  
    #endregion  
  
    // Send the data source view definition to the server  
    dsv.Update();  
  
    return dsv;  
}  
  
static void AddTable(DataSourceView dsv, OleDbConnection connection, String tableName)  
{  
    string strSelectText = "SELECT * FROM [dbo].[" + tableName + "] WHERE 1=0";  
    OleDbDataAdapter adapter = new OleDbDataAdapter(strSelectText, connection);  
    DataTable[] dataTables = adapter.FillSchema(dsv.Schema,  
        SchemaType.Mapped, tableName);  
    DataTable dataTable = dataTables[0];  
  
    dataTable.ExtendedProperties.Add("TableType", "Table");  
    dataTable.ExtendedProperties.Add("DbSchemaName", "dbo");  
    dataTable.ExtendedProperties.Add("DbTableName", tableName);  
    dataTable.ExtendedProperties.Add("FriendlyName", tableName);  
  
    dataTable = null;  
    dataTables = null;  
    adapter = null;  
}  
  
static void AddComputedColumn(DataSourceView dsv, OleDbConnection connection, String tableName, String computedColumnName, String expression)  
{  
    DataSet tmpDataSet = new DataSet();  
    tmpDataSet.Locale = CultureInfo.CurrentCulture;  
    OleDbDataAdapter adapter = new OleDbDataAdapter("SELECT ("  
        + expression + ") AS [" + computedColumnName + "] FROM [dbo].["  
        + tableName + "] WHERE 1=0", connection);  
    DataTable[] dataTables = adapter.FillSchema(tmpDataSet,  
        SchemaType.Mapped, tableName);  
    DataTable dataTable = dataTables[0];  
    DataColumn dataColumn = dataTable.Columns[computedColumnName];  
  
    dataTable.Constraints.Clear();  
    dataTable.Columns.Remove(dataColumn);  
  
    dataColumn.ExtendedProperties.Add("DbColumnName", computedColumnName);  
    dataColumn.ExtendedProperties.Add("ComputedColumnExpression",  
        expression);  
    dataColumn.ExtendedProperties.Add("IsLogical", "True");  
  
    dsv.Schema.Tables[tableName].Columns.Add(dataColumn);  
  
    dataColumn = null;  
    dataTable = null;  
    dataTables = null;  
    adapter = null;  
    tmpDataSet = null;  
}  
  
static void AddRelation(DataSourceView dsv, String fkTableName, String fkColumnName, String pkTableName, String pkColumnName)  
{  
    DataColumn fkColumn  
        = dsv.Schema.Tables[fkTableName].Columns[fkColumnName];  
    DataColumn pkColumn  
        = dsv.Schema.Tables[pkTableName].Columns[pkColumnName];  
    dsv.Schema.Relations.Add("FK_" + fkTableName + "_"  
        + fkColumnName, pkColumn, fkColumn, true);  
}  
  
static void AddCompositeRelation(DataSourceView dsv, String fkTableName, String pkTableName, String columnName1, String columnName2)  
{  
    DataColumn[] fkColumns = new DataColumn[2];  
    fkColumns[0] = dsv.Schema.Tables[fkTableName].Columns[columnName1];  
    fkColumns[1] = dsv.Schema.Tables[fkTableName].Columns[columnName2];  
  
    DataColumn[] pkColumns = new DataColumn[2];  
    pkColumns[0] = dsv.Schema.Tables[pkTableName].Columns[columnName1];  
    pkColumns[1] = dsv.Schema.Tables[pkTableName].Columns[columnName2];  
  
    dsv.Schema.Relations.Add("FK_" + fkTableName + "_" + columnName1  
        + "_" + columnName2, pkColumns, fkColumns, true);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Classes fondamentales AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-fundamental-classes.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
