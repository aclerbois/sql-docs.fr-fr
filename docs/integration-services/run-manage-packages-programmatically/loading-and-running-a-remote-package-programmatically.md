---
title: Chargement et exécution d’un package distant par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: run-manage-packages-programmatically
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Integration Services packages, running
- packages [Integration Services], running
- remote packages [Integration Services]
ms.assetid: 9f6ef376-3408-46bf-b5fa-fc7b18c689c9
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 60d811ccb738d3b9cabbb85118c99fc50fe01cb0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="loading-and-running-a-remote-package-programmatically"></a>Chargement et exécution d'un package distant par programme
  Pour exécuter des packages distants à partir d'un ordinateur local sur lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'est pas installé, démarrez les packages afin qu'ils s'exécutent sur l'ordinateur distant sur lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est installé. Pour cela, vous faites utiliser par l'ordinateur local l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un service Web ou un composant distant pour démarrer les packages sur l'ordinateur distant. Si vous essayez de démarrer directement les packages distants à partir de l'ordinateur local, les packages se chargeront sur l'ordinateur local et essayeront de s'exécuter à partir de ce dernier. Si [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'est pas installé sur l'ordinateur local, les packages ne s'exécuteront pas.  
  
> [!NOTE]  
>  Vous ne pouvez pas exécuter de packages en dehors de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] sur un ordinateur client sur lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’est pas installé, et les termes de votre licence [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vous permettent peut-être pas d’installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur d’autres ordinateurs. ([!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est un composant serveur et n'est pas redistribuable aux ordinateurs clients.)  
  
 Alternativement, vous pouvez exécuter un package distant à partir d'un ordinateur local sur lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est installé. Pour plus d’informations, consultez [Chargement et exécution d’un package local par programmation](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md).  
  
##  <a name="top"></a> Exécution d’un package distant sur l’ordinateur distant  
 Comme mentionné précédemment, il existe plusieurs manières d'exécuter un package distant sur un serveur distant :  
  
-   [Utiliser SQL Server Agent pour exécuter le package distant par programmation](#agent)  
  
-   [Utiliser un service web ou un composant distant pour exécuter le package distant par programmation](#service)  
  
 Presque toutes les méthodes utilisées dans cette rubrique pour charger et enregistrer des packages nécessitent une référence à l’assembly **Microsoft.SqlServer.ManagedDTS**. L’approche ADO.NET décrite dans cette rubrique pour exécuter la procédure stockée **sp_start_job**, laquelle nécessite uniquement une référence à **System.Data**, constitue la seule exception. Après avoir ajouté la référence à l’assembly **Microsoft.SqlServer.ManagedDTS** dans un nouveau projet, importez l’espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> avec une instruction **using** ou **Imports**.  
  
###  <a name="agent"></a> Utilisation de SQL Server Agent pour exécuter par programmation un package distant sur le serveur  
 L'exemple de code suivant montre comment utiliser par programme l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter un package distant sur le serveur. L’exemple de code appelle la procédure stockée système, **sp_start_job**, qui lance un travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Le travail que la procédure lance est nommé `RunSSISPackage`, et ce travail se trouve sur l'ordinateur distant. Le travail `RunSSISPackage` exécute alors le package sur l'ordinateur distant.  
  
> [!NOTE]  
>  La valeur retournée par la procédure stockée **sp_start_job** indique si la procédure stockée a pu démarrer correctement le travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. La valeur de retour n'indique pas si le package a réussi ou a échoué.  
  
 Pour plus d’informations sur la résolution des problèmes liés aux packages que vous exécutez à partir des travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez l’article Microsoft intitulé [Un package SSIS n’est pas exécuté quand vous appelez le package SSIS à partir d’une étape de travail de SQL Server Agent](http://support.microsoft.com/kb/918760).  
  
### <a name="sample-code"></a>Exemple de code  
  
```vb  
Imports System.Data  
Imports System.Data.SqlClient  
  
Module Module1  
  
  Sub Main()  
  
    Dim jobConnection As SqlConnection  
    Dim jobCommand As SqlCommand  
    Dim jobReturnValue As SqlParameter  
    Dim jobParameter As SqlParameter  
    Dim jobResult As Integer  
  
    jobConnection = New SqlConnection("Data Source=(local);Initial Catalog=msdb;Integrated Security=SSPI")  
    jobCommand = New SqlCommand("sp_start_job", jobConnection)  
    jobCommand.CommandType = CommandType.StoredProcedure  
  
    jobReturnValue = New SqlParameter("@RETURN_VALUE", SqlDbType.Int)  
    jobReturnValue.Direction = ParameterDirection.ReturnValue  
    jobCommand.Parameters.Add(jobReturnValue)  
  
    jobParameter = New SqlParameter("@job_name", SqlDbType.VarChar)  
    jobParameter.Direction = ParameterDirection.Input  
    jobCommand.Parameters.Add(jobParameter)  
    jobParameter.Value = "RunSSISPackage"  
  
    jobConnection.Open()  
    jobCommand.ExecuteNonQuery()  
    jobResult = DirectCast(jobCommand.Parameters("@RETURN_VALUE").Value, Integer)  
    jobConnection.Close()  
  
    Select Case jobResult  
      Case 0  
        Console.WriteLine("SQL Server Agent job, RunSISSPackage, started successfully.")  
      Case Else  
        Console.WriteLine("SQL Server Agent job, RunSISSPackage, failed to start.")  
    End Select  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
```csharp  
using System;  
using System.Data;  
using System.Data.SqlClient;  
  
namespace LaunchSSISPackageAgent_CS  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      SqlConnection jobConnection;  
      SqlCommand jobCommand;  
      SqlParameter jobReturnValue;  
      SqlParameter jobParameter;  
      int jobResult;  
  
      jobConnection = new SqlConnection("Data Source=(local);Initial Catalog=msdb;Integrated Security=SSPI");  
      jobCommand = new SqlCommand("sp_start_job", jobConnection);  
      jobCommand.CommandType = CommandType.StoredProcedure;  
  
      jobReturnValue = new SqlParameter("@RETURN_VALUE", SqlDbType.Int);  
      jobReturnValue.Direction = ParameterDirection.ReturnValue;  
      jobCommand.Parameters.Add(jobReturnValue);  
  
      jobParameter = new SqlParameter("@job_name", SqlDbType.VarChar);  
      jobParameter.Direction = ParameterDirection.Input;  
      jobCommand.Parameters.Add(jobParameter);  
      jobParameter.Value = "RunSSISPackage";  
  
      jobConnection.Open();  
      jobCommand.ExecuteNonQuery();  
      jobResult = (Int32)jobCommand.Parameters["@RETURN_VALUE"].Value;  
      jobConnection.Close();  
  
      switch (jobResult)  
      {  
        case 0:  
          Console.WriteLine("SQL Server Agent job, RunSISSPackage, started successfully.");  
          break;  
        default:  
          Console.WriteLine("SQL Server Agent job, RunSISSPackage, failed to start.");  
          break;  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
###  <a name="service"></a> Utilisation d’un service web ou d’un composant distant pour exécuter un package distant par programmation  
 La solution précédente pour l'exécution de packages par programme sur le serveur ne requiert pas de code personnalisé sur le serveur. Toutefois, vous pouvez préférer une solution qui ne compte pas sur l'Agent SQL Server pour exécuter des packages. L'exemple suivant présente un service Web qui peut être créé sur le serveur pour démarrer des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] localement, ainsi qu'une application de test qui peut être utilisée pour appeler le service Web à partir d'un ordinateur client. Si vous préférez créer un composant distant au lieu d’un service web, vous pouvez utiliser la même logique de code avec très peu de changements dans un composant distant. Toutefois, un composant distant risque de requérir une configuration plus approfondie qu'un service Web.  
  
> [!IMPORTANT]  
>  Avec ses paramètres par défaut pour l'authentification et l'autorisation, un service Web ne dispose généralement pas des autorisations suffisantes pour accéder à SQL Server ou au système de fichiers afin de charger et d'exécuter des packages. Vous risquez de devoir attribuer les autorisations appropriées au service web en configurant ses paramètres d’authentification et d’autorisation dans le fichier **web.config** et en attribuant les autorisations d’accès appropriées à la base de données et au système de fichiers. La description complète des autorisations Web, de base de données et de système de fichiers sort de la portée de cette rubrique.  
  
> [!IMPORTANT]  
>  Les méthodes de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> qui permettent d’utiliser le magasin de packages SSIS prennent uniquement en charge « . », localhost ou le nom du serveur local. Vous ne pouvez pas utiliser « (local) ».  
  
### <a name="sample-code"></a>Exemple de code  
 Les exemples de code suivants indiquent comment créer et tester le service Web.  
  
#### <a name="creating-the-web-service"></a>Création du service Web  
 Un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut être chargé directement à partir d'un fichier, directement à partir de SQL Server ou à partir du magasin de packages SSIS, lequel gère le stockage des packages dans SQL Server et des dossiers spéciaux du système de fichiers. Cet exemple prend en charge toutes les options disponibles en utilisant une construction **Select Case** ou **switch** pour sélectionner la syntaxe appropriée au démarrage du package et pour concaténer les arguments d’entrée convenablement. La méthode de service Web LaunchPackage renvoie le résultat de l'exécution du package sous la forme d'un entier plutôt que d'une valeur <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> afin que les ordinateurs clients ne requièrent pas de référence aux assemblys [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
###### <a name="to-create-a-web-service-to-run-packages-on-the-server-programmatically"></a>Pour créer un service Web afin d'exécuter par programme des packages sur le serveur  
  
1.  Ouvrez Visual Studio et créez un projet de service Web dans votre langage de programmation préféré. L'exemple de code utilise le nom LaunchSSISPackageService pour le projet.  
  
2.  Ajoutez une référence à **Microsoft.SqlServer.ManagedDTS** et ajoutez une instruction **Imports** ou **using** au fichier de code de l’espace de noms **Microsoft.SqlServer.Dts.Runtime**.  
  
3.  Collez l'exemple de code pour la méthode de service Web LaunchPackage dans la classe. (L'exemple présente tout le contenu de la fenêtre de code.)  
  
4.  Générez et testez le service Web en fournissant un jeu de valeurs valides pour les arguments d'entrée de la méthode LaunchPackage qui pointent vers un package existant. Par exemple, si package1.dtsx est stocké sur le serveur dans C:\My Packages, passez « file » comme valeur de sourceType, « C:\My Packages » comme valeur de sourceLocation et « package1 » (sans l'extension) comme valeur de packageName.  
  
```vb  
Imports System.Web  
Imports System.Web.Services  
Imports System.Web.Services.Protocols  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.IO  
  
<WebService(Namespace:="http://dtsue/")> _  
<WebServiceBinding(ConformsTo:=WsiProfiles.BasicProfile1_1)> _  
<Global.Microsoft.VisualBasic.CompilerServices.DesignerGenerated()> _  
Public Class LaunchSSISPackageService  
  Inherits System.Web.Services.WebService  
  
  ' LaunchPackage Method Parameters:  
  ' 1. sourceType: file, sql, dts  
  ' 2. sourceLocation: file system folder, (none), logical folder  
  ' 3. packageName: for file system, ".dtsx" extension is appended  
  
  <WebMethod()> _  
  Public Function LaunchPackage( _  
    ByVal sourceType As String, _  
    ByVal sourceLocation As String, _  
    ByVal packageName As String) As Integer 'DTSExecResult  
  
    Dim packagePath As String  
    Dim myPackage As Package  
    Dim integrationServices As New Application  
  
    ' Combine path and file name.  
    packagePath = Path.Combine(sourceLocation, packageName)  
  
    Select Case sourceType  
      Case "file"  
        ' Package is stored as a file.  
        ' Add extension if not present.  
        If String.IsNullOrEmpty(Path.GetExtension(packagePath)) Then  
          packagePath = String.Concat(packagePath, ".dtsx")  
        End If  
        If File.Exists(packagePath) Then  
          myPackage = integrationServices.LoadPackage(packagePath, Nothing)  
        Else  
          Throw New ApplicationException( _  
            "Invalid file location: " & packagePath)  
        End If  
      Case "sql"  
        ' Package is stored in MSDB.  
        ' Combine logical path and package name.  
        If integrationServices.ExistsOnSqlServer(packagePath, ".", String.Empty, String.Empty) Then  
          myPackage = integrationServices.LoadFromSqlServer( _  
            packageName, "(local)", String.Empty, String.Empty, Nothing)  
        Else  
          Throw New ApplicationException( _  
            "Invalid package name or location: " & packagePath)  
        End If  
      Case "dts"  
        ' Package is managed by SSIS Package Store.  
        ' Default logical paths are File System and MSDB.  
        If integrationServices.ExistsOnDtsServer(packagePath, ".") Then  
          myPackage = integrationServices.LoadFromDtsServer(packagePath, "localhost", Nothing)  
        Else  
          Throw New ApplicationException( _  
            "Invalid package name or location: " & packagePath)  
        End If  
      Case Else  
        Throw New ApplicationException( _  
          "Invalid sourceType argument: valid values are 'file', 'sql', and 'dts'.")  
    End Select  
  
    Return myPackage.Execute()  
  
  End Function  
  
End Class  
```  
  
```csharp  
using System;  
using System.Web;  
using System.Web.Services;  
using System.Web.Services.Protocols;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.IO;  
  
[WebService(Namespace = "http://dtsue/")]  
[WebServiceBinding(ConformsTo = WsiProfiles.BasicProfile1_1)]  
public class LaunchSSISPackageServiceCS : System.Web.Services.WebService  
{  
  public LaunchSSISPackageServiceCS()  
  {  
    }  
  
  // LaunchPackage Method Parameters:  
  // 1. sourceType: file, sql, dts  
  // 2. sourceLocation: file system folder, (none), logical folder  
  // 3. packageName: for file system, ".dtsx" extension is appended  
  
  [WebMethod]  
  public int LaunchPackage(string sourceType, string sourceLocation, string packageName)  
  {   
  
    string packagePath;  
    Package myPackage;  
    Application integrationServices = new Application();  
  
    // Combine path and file name.  
    packagePath = Path.Combine(sourceLocation, packageName);  
  
    switch(sourceType)  
    {  
      case "file":  
        // Package is stored as a file.  
        // Add extension if not present.  
        if (String.IsNullOrEmpty(Path.GetExtension(packagePath)))  
        {  
          packagePath = String.Concat(packagePath, ".dtsx");  
        }  
        if (File.Exists(packagePath))  
        {  
          myPackage = integrationServices.LoadPackage(packagePath, null);  
        }  
        else  
        {  
          throw new ApplicationException("Invalid file location: "+packagePath);  
        }  
        break;  
      case "sql":  
        // Package is stored in MSDB.  
        // Combine logical path and package name.  
        if (integrationServices.ExistsOnSqlServer(packagePath, ".", String.Empty, String.Empty))  
        {  
          myPackage = integrationServices.LoadFromSqlServer(packageName, "(local)", String.Empty, String.Empty, null);  
        }  
        else  
        {  
          throw new ApplicationException("Invalid package name or location: "+packagePath);  
        }  
        break;  
      case "dts":  
        // Package is managed by SSIS Package Store.  
        // Default logical paths are File System and MSDB.  
        if (integrationServices.ExistsOnDtsServer(packagePath, "."))  
        {  
          myPackage = integrationServices.LoadFromDtsServer(packagePath, "localhost", null);  
        }  
        else  
        {  
          throw new ApplicationException("Invalid package name or location: "+packagePath);  
        }  
        break;  
      default:  
        throw new ApplicationException("Invalid sourceType argument: valid values are 'file', 'sql', and 'dts'.");  
    }  
  
    return (Int32)myPackage.Execute();  
  
  }  
  
}  
```  
  
#### <a name="testing-the-web-service"></a>Test du service Web  
 L'exemple d'application console suivant utilise le service Web pour exécuter un package. La méthode LaunchPackage du service Web renvoie le résultat de l'exécution du package sous la forme d'un entier plutôt que d'une valeur <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> afin que les ordinateurs clients ne requièrent pas de référence aux assemblys [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. L'exemple crée une énumération privée dont les valeurs reflètent les valeurs <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> pour rapporter les résultats de l'exécution.  
  
###### <a name="to-create-a-console-application-to-test-the-web-service"></a>Pour créer une application console afin de tester le service Web  
  
1.  Dans Visual Studio, ajoutez une nouvelle application console, à l'aide de votre langage de programmation préféré, à la même solution que celle qui contient le projet de service Web. L'exemple de code utilise le nom LaunchSSISPackageTest pour le projet.  
  
2.  Définissez la nouvelle application console en tant que projet de démarrage dans la solution.  
  
3.  Ajoutez une référence Web pour le projet de service Web. Si nécessaire, ajustez la déclaration de variable dans l'exemple de code pour le nom que vous attribuez à l'objet proxy de service Web.  
  
4.  Collez l'exemple de code pour la routine principale et l'énumération privée dans le code. (L'exemple présente tout le contenu de la fenêtre de code.)  
  
5.  Modifiez la ligne de code qui appelle la méthode LaunchPackage afin de fournir un jeu de valeurs valides pour les arguments d'entrée qui pointent vers un package existant. Par exemple, si package1.dtsx est stocké sur le serveur dans C:\My Packages, passez « file » comme valeur de `sourceType`, « C:\My Packages » comme valeur de `sourceLocation` et « package1 » (sans l'extension) comme valeur de `packageName`.  
  
```vb  
Module LaunchSSISPackageTest  
  
  Sub Main()  
  
    Dim launchPackageService As New LaunchSSISPackageService.LaunchSSISPackageService  
    Dim packageResult As Integer  
  
    Try  
      packageResult = launchPackageService.LaunchPackage("sql", String.Empty, "SimpleTestPackage")  
    Catch ex As Exception  
      ' The type of exception returned by a Web service is:  
      '  System.Web.Services.Protocols.SoapException  
      Console.WriteLine("The following exception occurred: " & ex.Message)  
    End Try  
  
    Console.WriteLine(CType(packageResult, PackageExecutionResult).ToString)  
    Console.ReadKey()  
  
  End Sub  
  
  Private Enum PackageExecutionResult  
    PackageSucceeded  
    PackageFailed  
    PackageCompleted  
    PackageWasCancelled  
  End Enum  
  
End Module  
```  
  
```csharp  
using System;  
  
namespace LaunchSSISPackageSvcTestCS  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      LaunchSSISPackageServiceCS.LaunchSSISPackageServiceCS launchPackageService = new LaunchSSISPackageServiceCS.LaunchSSISPackageServiceCS();  
      int packageResult = 0;  
  
      try  
      {  
        packageResult = launchPackageService.LaunchPackage("sql", String.Empty, "SimpleTestPackage");  
      }  
      catch (Exception ex)  
      {  
        // The type of exception returned by a Web service is:  
        //  System.Web.Services.Protocols.SoapException  
        Console.WriteLine("The following exception occurred: " + ex.Message);  
      }  
  
      Console.WriteLine(((PackageExecutionResult)packageResult).ToString());  
      Console.ReadKey();  
  
    }  
  
    private enum PackageExecutionResult  
    {  
      PackageSucceeded,  
      PackageFailed,  
      PackageCompleted,  
      PackageWasCancelled  
    };  
  
  }  
}  
```  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Vidéo, [Procédure : automatiser l’exécution du package SSIS à l’aide de SQL Server Agent (vidéo de SQL Server)](http://technet.microsoft.com/sqlserver/ff686764.aspx), sur technet.microsoft.com  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des différences entre l’exécution locale et l’exécution distante](../../integration-services/run-manage-packages-programmatically/understanding-the-differences-between-local-and-remote-execution.md)   
 [Chargement et exécution d’un package local par programmation](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md)   
 [Chargement de la sortie d’un package local](../../integration-services/run-manage-packages-programmatically/loading-the-output-of-a-local-package.md)  
  
  
