---
title: Connexion à une Instance de SQL Server | Documents Microsoft
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
- SQL Server Management Objects, connections
- connections [SMO]
- instances of SQL Server, connections
- SMO [SQL Server], connections
ms.assetid: ad3cf354-b2e3-468b-b986-1232e375fd84
caps.latest.revision: 50
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4b1e22218bc0d30bcb06b5d170c2403e4e5785e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-an-instance-of-sql-server"></a>Connexion à une instance de SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  La première étape de programmation un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les application Management Objects (SMO) consiste à créer une instance de la <xref:Microsoft.SqlServer.Management.Smo.Server> objet et pour établir sa connexion à une instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Vous pouvez créer une instance de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server> et établir une connexion avec l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de trois manières. La première est d'utiliser une variable objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> pour fournir les informations de connexion. La deuxième est de fournir les informations de connexion en définissant explicitement les propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. La troisième est de passer le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. 
  
 **À l’aide d’un objet ServerConnection**  
  
 L'avantage de l'utilisation de la variable objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> est que vous pouvez réutiliser les informations de connexion. Déclarez une variable objet <xref:Microsoft.SqlServer.Management.Smo.Server>, puis un objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> et définissez les propriétés à l'aide des informations de connexion, telles que le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et le mode d'authentification. Passez ensuite la variable objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> en tant que paramètre au constructeur d'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. Le partage simultané de connexions entre différents objets serveur n'est pas recommandé. Utilisez la méthode <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> pour vous procurer une copie des paramètres de connexion existants.  
  
 **Définition des propriétés de l’objet serveur explicite**  
  
 Vous pouvez également déclarer la variable objet <xref:Microsoft.SqlServer.Management.Smo.Server> et appeler le constructeur par défaut. En l'état, l'objet <xref:Microsoft.SqlServer.Management.Smo.Server> tente de se connecter à l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec tous les paramètres de connexion par défaut.  
  
 **En fournissant le nom d’instance de SQL Server dans le constructeur d’objet serveur**  
  
 Déclarez la variable objet <xref:Microsoft.SqlServer.Management.Smo.Server> et passez le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en tant que paramètre de chaîne dans le constructeur. L'objet <xref:Microsoft.SqlServer.Management.Smo.Server> établit une connexion à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec les paramètres de connexion par défaut.  
  
## <a name="connection-pooling"></a>Regroupement de connexions  
 En règle générale, il n'est pas nécessaire d'appeler la méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> de l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>. SMO établit automatiquement une connexion lorsque cela est nécessaire et libère la connexion vers le groupement de connexions une fois les opérations terminées. Lorsque vous appelez la méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A>, la connexion n'est pas libérée vers le groupement. Un appel explicite à la méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Disconnect%2A> est nécessaire pour libérer la connexion vers le groupement. Qui plus est, vous pouvez demander une connexion non regroupée en définissant la propriété <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.NonPooledConnection%2A> de l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
## <a name="multithreaded-applications"></a>Applications multithread  
 Pour les applications multithread, un objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> distinct doit être utilisé dans chaque thread.  
  
## <a name="connecting-to-an-instance-of-sql-server-for-rmo"></a>Connexion à une instance de SQL Server pour RMO  
 RMO (Replication Management Objects) utilise une méthode légèrement différente de celle de SMO pour se connecter à un serveur de réplication.  
  
 Objets de programmation RMO exigent qu’une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de la <xref:Microsoft.SqlServer.Management.Common.ServerConnection> objet implémenté par le **Microsoft.SqlServer.Management.Common** espace de noms. Cette connexion au serveur est établie indépendamment d'un objet de programmation RMO. Elle est ensuite transmise à l'objet RMO lors de la création de l'instance ou par affectation à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> de l'objet. De cette manière, un objet de programmation RMO et les instances d'objet de connexion peuvent être créés et gérés séparément, et un objet de connexion peut être réutilisé avec plusieurs objets de programmation RMO. Les règles suivantes s'appliquent aux connexions à un serveur de réplication :  
  
-   Toutes les propriétés de la connexion sont définies pour un objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> défini.  
  
-   Chaque connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit posséder son propre objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   Toutes les informations d'authentification nécessaires pour l'établissement de la connexion et l'ouverture d'une session sur le serveur sont fournies dans l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   Par défaut, les connexions sont établies au moyen de l'authentification Microsoft Windows. Pour l'utilisation de l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la propriété <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.LoginSecure%2A> doit avoir la valeur False et <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Login%2A> et <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Password%2A> doivent être définis avec un nom d'ouverture de session et un mot de passe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valides. Informations d’identification de sécurité doivent toujours être stockées et gérées de manière sécurisée et fournies au moment de l’exécution chaque fois que possible.  
  
-   La méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> doit être appelée avant de passer la connexion à n'importe quel objet de programmation RMO.  
  
## <a name="examples"></a>Exemples  
Pour utiliser un exemple de code fourni, vous devrez sélectionner l'environnement, le modèle et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="connecting-to-the-local-instance-of-sql-server-by-using-windows-authentication-in-visual-basic"></a>Connexion à l'instance locale de SQL Server via l'authentification Windows en Visual Basic  
 La connexion à l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'exige pas beaucoup de code. À la place, elle se base sur les paramètres par défaut pour la méthode d'authentification et le serveur. La première opération nécessitant la récupération des données entraîne la création d'une connexion.  
 
Cet exemple est le code Visual Basic .NET qui se connecte à l’instance locale de SQL Server à l’aide de l’authentification Windows. 

```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'The connection is established when a property is requested.
Console.WriteLine(srv.Information.Version)
'The connection is automatically disconnected when the Server variable goes out of scope.
```
  
## <a name="connecting-to-the-local-instance-of-sql-server-by-using-windows-authentication-in-visual-c"></a>Connexion à l'instance locale de SQL Server via l'authentification Windows en Visual C#  
 La connexion à l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'exige pas beaucoup de code. À la place, elle se base sur les paramètres par défaut pour la méthode d'authentification et le serveur. La première opération nécessitant la récupération des données entraîne la création d'une connexion.  
  
 Cet exemple de code Visual C# .NET permet de se connecter à l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via l'authentification Windows.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//The connection is established when a property is requested.   
Console.WriteLine(srv.Information.Version);   
}   
//The connection is automatically disconnected when the Server variable goes out of scope.  
```  
  
## <a name="connecting-to-a-remote-instance-of-sql-server-by-using-windows-authentication-in-visual-basic"></a>Connexion à une instance distante de SQL Server via l'authentification Windows en Visual Basic  
 Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au moyen de l'authentification Windows, vous n'avez pas besoin de spécifier le type d'authentification. La méthode par défaut est l'authentification Windows.  
  
 Cet exemple est [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] code .NET qui se connecte à l’instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de l’authentification Windows. La variable de chaîne *strServer* contient le nom de l’instance distante.  
  
```VBNET   
'Connect to a remote instance of SQL Server.
Dim srv As Server
'The strServer string variable contains the name of a remote instance of SQL Server.
srv = New Server(strServer)
'The actual connection is made when a property is retrieved. 
Console.WriteLine(srv.Information.Version)
'The connection is automatically disconnected when the Server variable goes out of scope.
```
  
## <a name="connecting-to-a-remote-instance-of-sql-server-by-using-windows-authentication-in-visual-c"></a>Connexion à une instance distante de SQL Server via l'authentification Windows en Visual C#  
 Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au moyen de l'authentification Windows, vous n'avez pas besoin de spécifier le type d'authentification. La méthode par défaut est l'authentification Windows.  
  
 Cet exemple de code Visual C# .NET permet de se connecter à l'instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via l'authentification Windows. La variable de chaîne *strServer* contient le nom de l’instance distante.  
  
```csharp  
{   
//Connect to a remote instance of SQL Server.   
Server srv;   
//The strServer string variable contains the name of a remote instance of SQL Server.   
srv = new Server(strServer);   
//The actual connection is made when a property is retrieved.   
Console.WriteLine(srv.Information.Version);   
}   
//The connection is automatically disconnected when the Server variable goes out of scope.  
```  
  
## <a name="connecting-to-an-instance-of-sql-server-by-using-sql-server-authentication-in-visual-basic"></a>Connexion à une instance de SQL Server via l'authentification SQL Server en Visual Basic  
 Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous devez préciser le type d'authentification. Cet exemple montre l'autre méthode avec laquelle vous pouvez déclarer une variable objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> qui permet de réutiliser les informations de connexion.  
  
 L’exemple est [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] code .NET qui montre comment se connecter au serveur distant et *vPassword* contiennent l’ouverture de session et le mot de passe.  
  
```VBNET  
' compile with:   
' /r:Microsoft.SqlServer.Smo.dll  
' /r:Microsoft.SqlServer.ConnectionInfo.dll  
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      Dim sqlServerLogin As [String] = "user_id"  
      Dim password As [String] = "pwd"  
      Dim instanceName As [String] = "instance_name"  
      Dim remoteSvrName As [String] = "remote_server_name"  
  
      ' Connecting to an instance of SQL Server using SQL Server Authentication  
      Dim srv1 As New Server()   ' connects to default instance  
      srv1.ConnectionContext.LoginSecure = False   ' set to true for Windows Authentication  
      srv1.ConnectionContext.Login = sqlServerLogin  
      srv1.ConnectionContext.Password = password  
      Console.WriteLine(srv1.Information.Version)   ' connection is established  
  
      ' Connecting to a named instance of SQL Server with SQL Server Authentication using ServerConnection  
      Dim srvConn As New ServerConnection()  
      srvConn.ServerInstance = ".\" & instanceName   ' connects to named instance  
      srvConn.LoginSecure = False   ' set to true for Windows Authentication  
      srvConn.Login = sqlServerLogin  
      srvConn.Password = password  
      Dim srv2 As New Server(srvConn)  
      Console.WriteLine(srv2.Information.Version)   ' connection is established  
  
      ' For remote connection, remote server name / ServerInstance needs to be specified  
      Dim srvConn2 As New ServerConnection(remoteSvrName)  
      srvConn2.LoginSecure = False  
      srvConn2.Login = sqlServerLogin  
      srvConn2.Password = password  
      Dim srv3 As New Server(srvConn2)  
      Console.WriteLine(srv3.Information.Version)   ' connection is established  
   End Sub  
End Class  
```  
  
## <a name="connecting-to-an-instance-of-sql-server-by-using-sql-server-authentication-in-visual-c"></a>Connexion à une instance de SQL Server via l'authentification SQL Server en Visual C#  
 Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous devez préciser le type d'authentification. Cet exemple montre l'autre méthode avec laquelle vous pouvez déclarer une variable objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> qui permet de réutiliser les informations de connexion.  
  
 L’exemple est le code Visual c# .NET montre comment se connecter au serveur distant et *vPassword* contiennent l’ouverture de session et le mot de passe.  
  
```csharp  
// compile with:   
// /r:Microsoft.SqlServer.Smo.dll  
// /r:Microsoft.SqlServer.ConnectionInfo.dll  
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using System;  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Common;  
  
public class A {  
   public static void Main() {   
      String sqlServerLogin = "user_id";  
      String password = "pwd";  
      String instanceName = "instance_name";  
      String remoteSvrName = "remote_server_name";  
  
      // Connecting to an instance of SQL Server using SQL Server Authentication  
      Server srv1 = new Server();   // connects to default instance  
      srv1.ConnectionContext.LoginSecure = false;   // set to true for Windows Authentication  
      srv1.ConnectionContext.Login = sqlServerLogin;  
      srv1.ConnectionContext.Password = password;  
      Console.WriteLine(srv1.Information.Version);   // connection is established  
  
      // Connecting to a named instance of SQL Server with SQL Server Authentication using ServerConnection  
      ServerConnection srvConn = new ServerConnection();  
      srvConn.ServerInstance = @".\" + instanceName;   // connects to named instance  
      srvConn.LoginSecure = false;   // set to true for Windows Authentication  
      srvConn.Login = sqlServerLogin;  
      srvConn.Password = password;  
      Server srv2 = new Server(srvConn);  
      Console.WriteLine(srv2.Information.Version);   // connection is established  
  
      // For remote connection, remote server name / ServerInstance needs to be specified  
      ServerConnection srvConn2 = new ServerConnection(remoteSvrName);  
      srvConn2.LoginSecure = false;  
      srvConn2.Login = sqlServerLogin;  
      srvConn2.Password = password;  
      Server srv3 = new Server(srvConn2);  
      Console.WriteLine(srv3.Information.Version);   // connection is established  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  
