---
title: Appel de méthodes | Documents Microsoft
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: smo
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- methods [SMO]
- calling methods
- SQL Server Management Objects, method calling
- SMO [SQL Server], method calling
ms.assetid: c88d5c5f-9ff0-4f84-b2b6-24c6b90fa15e
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a3629f2f5d5905ce17df7a5aa15936edaa876ef4
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707707"
---
# <a name="calling-methods"></a>Appel de méthodes
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Les méthodes effectuent des tâches spécifiques liées à l’objet, telles que l’exécution une **point de contrôle** sur une base de données ou de la demande une liste énumérée d’ouvertures de session pour l’instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Les méthodes effectuent une opération sur un objet. Les méthodes peuvent accepter des paramètres et possèdent souvent une valeur de retour. La valeur de retour peut être un type de données simple, un objet complexe ou une structure qui contient de nombreux membres.  
  
 Utilisez la gestion des exceptions pour détecter si la méthode a réussi. Pour plus d’informations, consultez [Handling SMO Exceptions](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-exceptions.md).  
  
## <a name="examples"></a>Exemples  
Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
 
  
## <a name="using-a-simple-smo-method-in-visual-basic"></a>Utilisation d'une méthode SMO simple en Visual Basic  
 Dans cet exemple, la méthode <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> n'accepte aucun paramètre et ne possède aucune valeur de retour.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define a Database object variable by supplying the parent server and the database name arguments in the constructor.
Dim db As Database
db = New Database(srv, "Test_SMO_Database")
'Call the Create method to create the database on the instance of SQL Server. 
db.Create()
```
  
## <a name="using-a-simple-smo-method-in-visual-c"></a>Utilisation d'une méthode SMO simple en Visual C#  
 Dans cet exemple, la méthode <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> n'accepte aucun paramètre et ne possède aucune valeur de retour.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define a Database object variable by supplying the parent server and the database name arguments in the constructor.   
Database db;   
db = new Database(srv, "Test_SMO_Database");   
//Call the Create method to create the database on the instance of SQL Server.   
db.Create();   
```  
  
 }  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-basic"></a>Utilisation d'une méthode SMO avec un paramètre en Visual Basic  
 Le <xref:Microsoft.SqlServer.Management.Smo.Table> objet possède une méthode appelée <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Cette méthode requiert un paramètre numérique qui spécifie le **FillFactor**.  
  
```VBNET
Dim srv As Server  
srv = New Server  
Dim tb As Table  
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources")  
tb.RebuildIndexes(70)  
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-c"></a>Utilisation d'une méthode SMO avec un paramètre en Visual C#  
 Le <xref:Microsoft.SqlServer.Management.Smo.Table> objet possède une méthode appelée <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Cette méthode requiert un paramètre numérique qui spécifie le `FillFactor`.  
  
```csharp  
{   
Server srv = default(Server);   
srv = new Server();   
Table tb = default(Table);   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
}   
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-basic"></a>Utilisation d'une méthode d'énumération qui retourne un objet DataTable en Visual Basic  
 Cette section décrit comment appeler une méthode d’énumération et comment gérer les données dans la liste retournée <xref:System.Data.DataTable> objet.  
  
 La méthode <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> retourne un objet <xref:System.Data.DataTable>, qui requiert une navigation supplémentaire pour accéder à toutes les informations de classement disponibles sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET
'Connect to the local, default instance of SQL Server.  
Dim srv As Server  
srv = New Server  
'Call the EnumCollations method and return collation information to DataTable variable.  
Dim d As DataTable  
'Select the returned data into an array of DataRow.  
d = srv.EnumCollations  
'Iterate through the rows and display collation details for the instance of SQL Server.  
Dim r As DataRow  
Dim c As DataColumn  
For Each r In d.Rows  
    Console.WriteLine("==")  
    For Each c In r.Table.Columns  
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)  
    Next  
Next  
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-c"></a>Utilisation d'une méthode d'énumération qui retourne un objet DataTable en Visual C#  
 Cette section décrit comment appeler une méthode d’énumération et comment gérer les données dans la liste retournée <xref:System.Data.DataTable> objet.  
  
 Le <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> méthode retourne un système <xref:System.Data.DataTable> objet. Le <xref:System.Data.DataTable> objet nécessite davantage de navigation pour accéder à toutes les informations de classement disponibles sur l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumCollations;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=========");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="constructing-an-object-in-visual-basic"></a>Construction d'un objet en Visual Basic  
 Le constructeur d'un objet quelconque peut être appelé au moyen de l'opérateur **New** . Le constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Database> est surchargé et la version du constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Database> utilisée dans l'exemple accepte deux paramètres : l'objet parent <xref:Microsoft.SqlServer.Management.Smo.Server> auquel la base de données appartient et une chaîne qui représente le nom de la nouvelle base de données.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.
Dim d As Database
d = New Database(srv, "Test_SMO_Database")
'Create the database on the instance of SQL Server.
d.Create()
Console.WriteLine(d.Name)
```
  
## <a name="constructing-an-object-in-visual-c"></a>Construction d'un objet en Visual C#  
 Le constructeur d'un objet quelconque peut être appelé au moyen de l'opérateur **New** . Le constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Database> est surchargé et la version du constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Database> utilisée dans l'exemple accepte deux paramètres : l'objet parent <xref:Microsoft.SqlServer.Management.Smo.Server> auquel la base de données appartient et une chaîne qui représente le nom de la nouvelle base de données.  
  
```csharp  
{   
Server srv;   
srv = new Server();   
Table tb;   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.   
Database d;   
d = new Database(srv, "Test_SMO_Database");   
//Create the database on the instance of SQL Server.   
d.Create();   
Console.WriteLine(d.Name);   
}  
```  
  
## <a name="copying-an-smo-object-in-visual-basic"></a>Copie d'un objet SMO en Visual Basic  
 Cet exemple de code utilise le <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> méthode pour créer une copie de la <xref:Microsoft.SqlServer.Management.Smo.Server> objet. Le <xref:Microsoft.SqlServer.Management.Smo.Server> objet représente une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET  
'Connect to the local, default instance of SQL Server.
Dim srv1 As Server
srv1 = New Server()
'Modify the default database and the timeout period for the connection.
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012"
srv1.ConnectionContext.ConnectTimeout = 30
'Make a second connection using a copy of the ConnectionContext property and verify settings.
Dim srv2 As Server
srv2 = New Server(srv1.ConnectionContext.Copy)
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString)
```
  
## <a name="copying-an-smo-object-in-visual-c"></a>Copie d'un objet SMO en Visual C#  
 Cet exemple de code utilise le <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> méthode pour créer une copie de la <xref:Microsoft.SqlServer.Management.Smo.Server> objet. Le <xref:Microsoft.SqlServer.Management.Smo.Server> objet représente une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv1;   
srv1 = new Server();   
//Modify the default database and the timeout period for the connection.   
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012";   
srv1.ConnectionContext.ConnectTimeout = 30;   
//Make a second connection using a copy of the ConnectionContext property and verify settings.   
Server srv2;   
srv2 = new Server(srv1.ConnectionContext.Copy);   
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString);   
}  
```  
  
## <a name="monitoring-server-processes-in-visual-basic"></a>Surveillance des processus serveur en Visual Basic  
 Vous pouvez obtenir les informations de type d’état actuelles sur l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via les méthodes d’énumération. L'exemple de code utilise la méthode <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> pour découvrir des informations sur les processus en cours. Il montre également comment utiliser les colonnes et les lignes incluses dans l'objet <xref:System.Data.DataTable> retourné.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Call the EnumCollations method and return collation information to DataTable variable.
Dim d As DataTable
'Select the returned data into an array of DataRow.
d = srv.EnumProcesses
'Iterate through the rows and display collation details for the instance of SQL Server.
Dim r As DataRow
Dim c As DataColumn
For Each r In d.Rows
    Console.WriteLine("============================================")
    For Each c In r.Table.Columns
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)
    Next
Next
```
  
## <a name="monitoring-server-processes-in-visual-c"></a>Surveillance des processus serveur en Visual C#  
 Vous pouvez obtenir les informations de type d’état actuelles sur l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via les méthodes d’énumération. L'exemple de code utilise la méthode <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> pour découvrir des informations sur les processus en cours. Il montre également comment utiliser les colonnes et les lignes incluses dans l'objet <xref:System.Data.DataTable> retourné.  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumProcesses;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=====");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  
