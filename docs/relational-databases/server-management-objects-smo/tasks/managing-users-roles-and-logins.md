---
title: Gestion des utilisateurs, des rôles et des connexions | Documents Microsoft
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
- logins [SMO]
- roles [SMO]
- users [SMO]
ms.assetid: 74e411fa-74ed-49ec-ab58-68c250f2280e
caps.latest.revision: 45
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a803aa9d6443c8aa62d7ebca7d7ac5bd3abf990e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="managing-users-roles-and-logins"></a>Gestion des utilisateurs, rôles et connexions
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Dans SMO, les connexions sont représentées par l'objet <xref:Microsoft.SqlServer.Management.Smo.Login>. Lorsque la connexion existe dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], elle peut être ajoutée à un rôle serveur. Le rôle de serveur est représenté par la <xref:Microsoft.SqlServer.Management.Smo.ServerRole> objet. Le rôle de base de données est représenté par l'objet <xref:Microsoft.SqlServer.Management.Smo.DatabaseRole> et le rôle d'application est représenté par l'objet <xref:Microsoft.SqlServer.Management.Smo.ApplicationRole>.  
  
 Privilèges associés au niveau du serveur sont répertoriés en tant que propriétés de la <xref:Microsoft.SqlServer.Management.Smo.ServerPermission> objet. Les privilèges au niveau serveur peuvent être accordés, refusés ou révoqués pour les comptes d'ouverture de session individuels.  
  
 Chaque <xref:Microsoft.SqlServer.Management.Smo.Database> objet a un <xref:Microsoft.SqlServer.Management.Smo.UserCollection> objet qui spécifie tous les utilisateurs dans la base de données. Chaque utilisateur est associé à une ouverture de session. Une ouverture de session peut être associée à plusieurs utilisateurs d'une base de données. Le <xref:Microsoft.SqlServer.Management.Smo.Login> l’objet <xref:Microsoft.SqlServer.Management.Smo.Login.EnumDatabaseMappings%2A> méthode peut être utilisée pour répertorier tous les utilisateurs dans chaque base de données qui est associé à l’ouverture de session. Ou bien, la propriété <xref:Microsoft.SqlServer.Management.Smo.User> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Login> spécifie l'ouverture de session qui est associée à l'utilisateur.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bases de données ont également des rôles qui spécifient un jeu de privilèges de niveau base de données qui permettent à un utilisateur d’effectuer des tâches spécifiques. Contrairement aux rôles de serveur, les rôles de base de données ne sont pas fixes. Ils peuvent être créés, modifiés et supprimés. Les privilèges et utilisateurs peuvent être attribués à un rôle de base de données pour une administration en masse.  
  
## <a name="example"></a>Exemple  
 Dans les exemples de code suivants, vous devez sélectionner l'environnement, le modèle et le langage de programmation à utiliser pour créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="enumerating-logins-and-associated-users-in-visual-c"></a>Énumération des connexions et des utilisateurs associés en Visual C#  
 Chaque utilisateur d'une base de données est associé à une ouverture de session. L'ouverture de session peut être associée à plusieurs utilisateurs dans plusieurs bases de données. L'exemple de code montre comment appeler la méthode <xref:Microsoft.SqlServer.Management.Smo.Login.EnumDatabaseMappings%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Login> pour répertorier tous les utilisateurs de la base de données qui sont associés à l'ouverture de session. L’exemple crée une ouverture de session et l’utilisateur dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données à Assurez-vous qu’il existe des informations de mappage à énumérer.  
  
```csharp  
{   
Server srv = new Server();   
//Iterate through each database and display.   
  
foreach ( Database db in srv.Databases) {   
   Console.WriteLine("========");   
   Console.WriteLine("Login Mappings for the database: " + db.Name);   
   Console.WriteLine(" ");   
   //Run the EnumLoginMappings method and return details of database user-login mappings to a DataTable object variable.   
   DataTable d;  
   d = db.EnumLoginMappings();   
   //Display the mapping information.   
   foreach (DataRow r in d.Rows) {   
      foreach (DataColumn c in r.Table.Columns) {   
         Console.WriteLine(c.ColumnName + " = " + r[c]);   
      }   
      Console.WriteLine(" ");   
   }   
}   
}  
```  
  
## <a name="enumerating-logins-and-associated-users-in-powershell"></a>Énumération des connexions et des utilisateurs associés dans PowerShell  
 Chaque utilisateur d'une base de données est associé à une ouverture de session. L'ouverture de session peut être associée à plusieurs utilisateurs dans plusieurs bases de données. L'exemple de code montre comment appeler la méthode <xref:Microsoft.SqlServer.Management.Smo.Login.EnumDatabaseMappings%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Login> pour répertorier tous les utilisateurs de la base de données qui sont associés à l'ouverture de session. L’exemple crée une ouverture de session et l’utilisateur dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données à Assurez-vous qu’il existe des informations de mappage à énumérer.  
  
```powershell  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\Default\Databases  
  
#Iterate through all databases  
 foreach ($db in Get-ChildItem)  
 {  
 "====="  
 "Login Mappings for the database: "+ $db.Name  
  
 #get the datatable containing the mapping from the smo database oject  
 $dt = $db.EnumLoginMappings()  
  
 #display the results  
 foreach($row in $dt.Rows)  
     {  
        foreach($col in $row.Table.Columns)  
      {  
        $col.ColumnName + "=" + $row[$col]  
       }  
  
     }  
 }  
```  
  
## <a name="managing-roles-and-users"></a>Gestion des rôles et des utilisateurs  
 Cet exemple montre comment gérer les rôles et les utilisateurs. Pour exécuter cet exemple, vous devrez référencer les assemblys suivants :  
  
-   Microsoft.SqlServer.Smo.dll  
  
-   Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
-   Microsoft.SqlServer.ConnectionInfo.dll  
  
-   Microsoft.SqlServer.SqlEnum.dll  
  
```csharp  
using Microsoft.SqlServer.Management.Smo;  
using System;  
  
public class A {  
   public static void Main() {  
      Server svr = new Server();  
      Database db = new Database(svr, "TESTDB");  
      db.Create();  
  
      // Creating Logins  
      Login login = new Login(svr, "Login1");  
      login.LoginType = LoginType.SqlLogin;  
      login.Create("password@1");  
  
      Login login2 = new Login(svr, "Login2");  
      login2.LoginType = LoginType.SqlLogin;  
      login2.Create("password@1");  
  
      // Creating Users in the database for the logins created  
      User user1 = new User(db, "User1");  
      user1.Login = "Login1";  
      user1.Create();  
  
      User user2 = new User(db, "User2");  
      user2.Login = "Login2";  
      user2.Create();  
  
      // Creating database permission Sets  
      DatabasePermissionSet dbPermSet = new DatabasePermissionSet(DatabasePermission.AlterAnySchema);  
      dbPermSet.Add(DatabasePermission.AlterAnyUser);  
  
      DatabasePermissionSet dbPermSet2 = new DatabasePermissionSet(DatabasePermission.CreateType);  
      dbPermSet2.Add(DatabasePermission.CreateSchema);  
      dbPermSet2.Add(DatabasePermission.CreateTable);  
  
      // Creating Database roles  
      DatabaseRole role1 = new DatabaseRole(db, "Role1");  
      role1.Create();  
  
      DatabaseRole role2 = new DatabaseRole(db, "Role2");  
      role2.Create();  
  
      // Granting Database Permission Sets to Roles  
      db.Grant(dbPermSet, role1.Name);  
      db.Grant(dbPermSet2, role2.Name);  
  
      // Adding members (Users / Roles) to Role  
      role1.AddMember("User1");  
  
      role2.AddMember("User2");  
  
      // Role1 becomes a member of Role2  
      role2.AddMember("Role1");  
  
      // Enumerating through explicit permissions granted to Role1  
      // enumerates all database permissions for the Grantee  
      DatabasePermissionInfo[] dbPermsRole1 = db.EnumDatabasePermissions("Role1");     
      foreach (DatabasePermissionInfo dbp in dbPermsRole1) {  
         Console.WriteLine(dbp.Grantee + " has " + dbp.PermissionType.ToString() + " permission.");  
      }  
      Console.WriteLine(" ");  
   }  
}  
```
  
  
