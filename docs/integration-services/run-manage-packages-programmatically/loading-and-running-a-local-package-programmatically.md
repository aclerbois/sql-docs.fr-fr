---
title: Chargement et exécution d’un package local par programmation | Microsoft Docs
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
dev_langs:
- VB
helpviewer_keywords:
- Integration Services packages, running
- events [Integration Services], capturing
- packages [Integration Services], running
- loading packages programmatically
- Visual Basic [Integration Services]
- running packages [Integration Services]
- programmatically load and run packages [SSIS]
ms.assetid: 2f9fc1a8-a001-4c54-8c64-63b443725422
caps.latest.revision: 60
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cc12741e0cbd00439a4c83ecb0cf683f7198738d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="loading-and-running-a-local-package-programmatically"></a>Chargement et exécution d'un package local par programme
  Vous pouvez exécuter des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] selon les besoins ou à des moments prédéterminés à l’aide des méthodes décrites dans [Exécution de packages](https://msdn.microsoft.com/library/ms141708(v=sql.110).aspx). Toutefois, quelques lignes de code vous suffisent pour exécuter également un package à partir d'une application personnalisée, telle qu'une application Windows Forms, une application console, un ASP.NET Web Form, un service Web ASP.NET ou un service Windows.  
  
 Cette rubrique traite des points suivants :  
  
-   Chargement d’un package par programmation  
  
-   Exécution d'un package par programme  
  
 Toutes les méthodes utilisées dans cette rubrique pour charger et exécuter des packages nécessitent une référence à l’assembly **Microsoft.SqlServer.ManagedDTS**. Après avoir ajouté la référence dans un nouveau projet, importez l’espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> à l’aide d’une instruction **using** ou **Imports**.  
  
## <a name="loading-a-package-programmatically"></a>Chargement d'un package par programme  
 Pour charger par programme un package sur l'ordinateur local, que le package soit stocké localement ou à distance, appelez l'une des méthodes suivantes :  
  
|Emplacement de stockage|Méthode à appeler|  
|----------------------|--------------------|  
|Fichier|<xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadPackage%2A>|  
|Magasin de packages SSIS|<xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromDtsServer%2A>|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|<xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromSqlServer%2A>|  
  
> [!IMPORTANT]  
>  Les méthodes de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> qui permettent d'utiliser le magasin de packages SSIS prennent uniquement en charge « . », localhost ou le nom du serveur local. Vous ne pouvez pas utiliser « (local) ».  
  
## <a name="running-a-package-programmatically"></a>Exécution d’un package par programmation  
 Le développement d'une application personnalisée dans du code managé qui exécute un package sur l'ordinateur local requiert la procédure suivante. Les étapes résumées ci-dessous sont présentées dans l'exemple d'application console qui suit.  
  
#### <a name="to-run-a-package-on-the-local-computer-programmatically"></a>Pour exécuter par programme un package sur l'ordinateur local  
  
1.  Démarrez l'environnement de développement Visual Studio et créez une application dans votre langage de développement par défaut. Cet exemple utilise une application console. Cependant, vous pouvez également exécuter un package à partir d'une application Windows Forms, un ASP.NET Web Form, un service Web ASP.NET ou un service Windows.  
  
2.  Dans le menu **Projet**, cliquez sur **Ajouter une référence**, puis ajoutez une référence à **Microsoft.SqlServer.ManagedDTS.dll**. Cliquez sur **OK**.  
  
3.  Utilisez l’instruction Visual Basic **Imports** ou l’instruction C# **using** pour importer l’espace de noms **Microsoft.SqlServer.Dts.Runtime**.  
  
4.  Ajoutez le code suivant dans la routine principale. L'application console terminée doit ressembler à l'exemple suivant.  
  
    > [!NOTE]  
    >  L'exemple de code montre le chargement du package à partir du système de fichiers à l'aide de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadPackage%2A>. Toutefois, vous pouvez également charger le package à partir de la base de données MSDB en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromSqlServer%2A>, ou à partir du magasin de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromDtsServer%2A>.  
  
5.  Exécutez le projet. L’exemple de code exécute l’exemple de package CalculatedColumns installé avec les exemples [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le résultat de l'exécution du package est affiché dans la fenêtre de la console.  
  
### <a name="sample-code"></a>Exemple de code  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkgLocation As String  
    Dim pkg As New Package  
    Dim app As New Application  
    Dim pkgResults As DTSExecResult  
  
    pkgLocation = _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CalculatedColumns Sample\CalculatedColumns\CalculatedColumns.dtsx"  
    pkg = app.LoadPackage(pkgLocation, Nothing)  
    pkgResults = pkg.Execute()  
  
    Console.WriteLine(pkgResults.ToString())  
    Console.ReadKey()  
  
  End Sub  
  
End Module  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace RunFromClientAppCS  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string pkgLocation;  
      Package pkg;  
      Application app;  
      DTSExecResult pkgResults;  
  
      pkgLocation =  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CalculatedColumns Sample\CalculatedColumns\CalculatedColumns.dtsx";  
      app = new Application();  
      pkg = app.LoadPackage(pkgLocation, null);  
      pkgResults = pkg.Execute();  
  
      Console.WriteLine(pkgResults.ToString());  
      Console.ReadKey();  
    }  
  }  
}  
```  
  
## <a name="capturing-events-from-a-running-package"></a>Capture d'événements à partir d'un package en cours d'exécution  
 Lorsque vous exécutez un package par programme comme dans l'exemple précédent, vous pouvez également capturer les erreurs et les autres événements qui se produisent lors de l'exécution du package. Pour cela, vous devez ajouter une classe qui hérite de la classe <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> et passer une référence à cette classe lorsque vous chargez le package. Bien que l'exemple suivant capture uniquement l'événement <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents.OnError%2A>, la classe <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> vous permet de capturer de nombreux autres événements.  
  
#### <a name="to-run-a-package-on-the-local-computer-programmatically-and-capture-package-events"></a>Pour exécuter par programme un package sur l'ordinateur local et capturer les événements du package  
  
1.  Suivez les étapes dans l'exemple précédent pour créer un projet à utiliser dans cet exemple.  
  
2.  Ajoutez le code suivant dans la routine principale. L'application console terminée doit ressembler à l'exemple suivant.  
  
3.  Exécutez le projet. L’exemple de code exécute l’exemple de package CalculatedColumns installé avec les exemples [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le résultat de l'exécution du package est affiché dans la fenêtre de la console, avec toutes les erreurs qui se sont éventuellement produites.  
  
### <a name="sample-code"></a>Exemple de code  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkgLocation As String  
    Dim pkg As New Package  
    Dim app As New Application  
    Dim pkgResults As DTSExecResult  
  
    Dim eventListener As New EventListener()  
  
    pkgLocation = _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CalculatedColumns Sample\CalculatedColumns\CalculatedColumns.dtsx"  
    pkg = app.LoadPackage(pkgLocation, eventListener)  
    pkgResults = pkg.Execute(Nothing, Nothing, eventListener, Nothing, Nothing)  
  
    Console.WriteLine(pkgResults.ToString())  
    Console.ReadKey()  
  
  End Sub  
  
End Module  
  
Class EventListener  
  Inherits DefaultEvents  
  
  Public Overrides Function OnError(ByVal source As Microsoft.SqlServer.Dts.Runtime.DtsObject, _  
    ByVal errorCode As Integer, ByVal subComponent As String, ByVal description As String, _  
    ByVal helpFile As String, ByVal helpContext As Integer, _  
    ByVal idofInterfaceWithError As String) As Boolean  
  
    ' Add application–specific diagnostics here.  
    Console.WriteLine("Error in {0}/{1} : {2}", source, subComponent, description)  
    Return False  
  
  End Function  
  
End Class  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace RunFromClientAppWithEventsCS  
{  
  class MyEventListener : DefaultEvents  
  {  
    public override bool OnError(DtsObject source, int errorCode, string subComponent,   
      string description, string helpFile, int helpContext, string idofInterfaceWithError)  
    {  
      // Add application-specific diagnostics here.  
      Console.WriteLine("Error in {0}/{1} : {2}", source, subComponent, description);  
      return false;  
    }  
  }  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string pkgLocation;  
      Package pkg;  
      Application app;  
      DTSExecResult pkgResults;  
  
      MyEventListener eventListener = new MyEventListener();  
  
      pkgLocation =  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CalculatedColumns Sample\CalculatedColumns\CalculatedColumns.dtsx";  
      app = new Application();  
      pkg = app.LoadPackage(pkgLocation, eventListener);  
      pkgResults = pkg.Execute(null, null, eventListener, null, null);  
  
      Console.WriteLine(pkgResults.ToString());  
      Console.ReadKey();  
    }  
  }  
}  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des différences entre l’exécution locale et l’exécution distante](../../integration-services/run-manage-packages-programmatically/understanding-the-differences-between-local-and-remote-execution.md)   
 [Chargement et exécution d’un package distant par programmation](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-remote-package-programmatically.md)   
 [Chargement de la sortie d’un package local](../../integration-services/run-manage-packages-programmatically/loading-the-output-of-a-local-package.md)  
  
  
