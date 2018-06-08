---
title: Prise en main de l’intégration du CLR | Documents Microsoft
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
dev_langs:
- TSQL
- VB
- CSharp
helpviewer_keywords:
- database objects [CLR integration]
- namespaces [CLR integration]
- database objects [CLR integration], about CLR integration
- stored procedures [CLR integration]
- common language runtime [SQL Server], about CLR integration
- building database objects [CLR integration], about CLR integration
- common language runtime [SQL Server], stored procedures
- common language runtime [SQL Server], namespaces
- Hello World example [CLR integration]
- library [CLR integration]
ms.assetid: c73e628a-f54a-411a-bfe3-6dae519316cc
caps.latest.revision: 62
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: d79bafbd781b34d3f6fa4908f998775f37548cd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-clr-integration"></a>Mise en route avec l'intégration du CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique fournit une vue d’ensemble des espaces de noms et des bibliothèques requis pour compiler des objets de base de données à l’aide de la [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intégration avec le common language runtime (CLR) du .NET Framework. La rubrique vous indique également comment écrire, compiler et exécuter une procédure stockée CLR simple écrite dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#.  
  
## <a name="required-namespaces"></a>Espaces de noms requis  
 Les composants requis pour développer des objets de base de données CLR base sont installés avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les fonctionnalités d'intégration du CLR sont exposées dans un assembly appelé system.data.dll, qui fait partie du .NET Framework. Cet assembly se trouve dans le Global Assembly Cache (GAC), ainsi que dans le répertoire .NET Framework. Une référence à cet assembly est ajoutée en général automatiquement par les outils en ligne de commande et [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio, afin qu'il ne soit pas nécessaire de l'ajouter manuellement.  
  
 L'assembly system.data.dll contient les espaces de noms suivants, qui sont requis pour compiler les objets de base de données CLR :  
  
 `System.Data`  
  
 `System.Data.Sql`  
  
 `Microsoft.SqlServer.Server`  
  
 `System.Data.SqlTypes`  
  
## <a name="writing-a-simple-hello-world-stored-procedure"></a>Écriture d'une procédure stockée "Hello World" simple  
 Copiez et collez le code Visual C# ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic suivant dans un éditeur de texte et enregistrez-le dans un fichier nommé "helloworld.cs" ou "helloworld.vb".  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Server;  
using System.Data.SqlTypes;  
  
public class HelloWorldProc  
{  
    [Microsoft.SqlServer.Server.SqlProcedure]  
    public static void HelloWorld(out string text)  
    {  
        SqlContext.Pipe.Send("Hello world!" + Environment.NewLine);  
        text = "Hello world!";  
    }  
}  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports Microsoft.SqlServer.Server  
Imports System.Data.SqlTypes  
Imports System.Runtime.InteropServices  
  
Public Class HelloWorldProc  
    \<Microsoft.SqlServer.Server.SqlProcedure> _   
    Public Shared  Sub HelloWorld(\<Out()> ByRef text as String)  
        SqlContext.Pipe.Send("Hello world!" & Environment.NewLine)  
        text = "Hello world!"  
    End Sub  
End Class  
  
```  
  
 Ce programme simple contient une méthode statique unique sur une classe publique. Cette méthode utilise deux nouvelles classes, **[SqlContext](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.aspx)** et  **[SqlPipe](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx)**, création managé vers la sortie d’un message textuel simple, les objets de base de données. La méthode affecte également la chaîne « Hello world ! » en tant que valeur de paramètre de sortie. Cette méthode peut être déclarée comme une procédure stockée dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], puis s'exécuter de la même manière qu'une procédure stockée [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Compiler ce programme comme une bibliothèque, le charger dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], et l’exécuter comme une procédure stockée.  
  
## <a name="compile-the-hello-world-stored-procedure"></a>Compiler la procédure « Hello World » stockées  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe par défaut les fichiers de redistribution [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. Ces fichiers incluent csc.exe et vbc.exe, les compilateurs de ligne de commande pour les programmes Visual C# et Visual Basic. Pour compiler notre exemple, vous devez modifier votre variable de chemin d'accès pour pointer sur le répertoire qui contient csc.exe ou vbc.exe. Le chemin d'installation par défaut du .NET Framework est le suivant :  
  
```  
C:\Windows\Microsoft.NET\Framework\(version)  
```  
  
 La version contient le numéro de version du programme redistribuable .NET Framework installé. Exemple :  
  
```  
C:\Windows\Microsoft.NET\Framework\v4.6.1  
```  
  
 Une fois que vous avez ajouté le répertoire .NET Framework à votre chemin d'accès, vous pouvez compiler l'exemple de procédure stockée en assembly à l'aide de la commande ci-dessous. Le **/target** option vous permet de compiler dans un assembly.  
  
 Pour les fichiers sources Visual C# :  
  
```  
csc /target:library helloworld.cs   
```  
  
 Pour les fichiers sources Visual Basic :  
  
```  
vbc /target:library helloworld.vb  
```  
  
 Ces commandes lancent le compilateur Visual C# ou Visual Basic en utilisant l'option /target pour spécifier la génération d'une DLL de bibliothèque.  
  
## <a name="loading-and-running-the-hello-world-stored-procedure-in-sql-server"></a>Chargement et exécution de la procédure stockée "Hello World" dans SQL Server  
 Une fois que l'exemple de procédure a été compilé avec succès, vous pouvez le tester dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour cela, ouvrez [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et créez une nouvelle requête, en vous connectant à une base de données de test appropriée (par exemple, l'exemple de base de données AdventureWorks).  
  
 La fonctionnalité d'exécution du code CLR est désactivée par défaut dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le code CLR peut être activé à l’aide de la **sp_configure** procédure stockée système. Pour plus d’informations, consultez [Activation de l’intégration du CLR](../../../relational-databases/clr-integration/clr-integration-enabling.md).  
  
 Il est nécessaire de créer l'assembly afin de pouvoir accéder à la procédure stockée. Pour cet exemple, nous supposerons que vous avez créé l'assembly helloworld.dll dans le répertoire C:\. Ajoutez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante à votre requête.  
  
```  
CREATE ASSEMBLY helloworld from 'c:\helloworld.dll' WITH PERMISSION_SET = SAFE  
```  
  
 Une fois l'assembly créé, il est possible d'accéder à la méthode HelloWorld en utilisant l'instruction CREATE PROCEDURE. Nous appellerons la procédure stockée "hello" :  
  
```  
  
CREATE PROCEDURE hello  
@i nchar(25) OUTPUT  
AS  
EXTERNAL NAME helloworld.HelloWorldProc.HelloWorld  
-- if the HelloWorldProc class is inside a namespace (called MyNS),  
-- the last line in the create procedure statement would be  
-- EXTERNAL NAME helloworld.[MyNS.HelloWorldProc].HelloWorld  
```  
  
 Une fois la procédure créée, elle peut être exécutée tout comme une procédure stockée normale écrite dans [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Exécutez la commande suivante :  
  
```  
DECLARE @J nchar(25)  
EXEC hello @J out  
PRINT @J  
```  
  
 Cela doit générer la sortie ci-dessous dans la fenêtre des messages [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
```  
Hello world!  
Hello world!  
```  
  
## <a name="removing-the-hello-world-stored-procedure-sample"></a>Suppression de l'exemple de procédure stockée "Hello World"  
 Lorsque vous avez terminé d'exécuter l'exemple de procédure stockée, vous pouvez supprimer la procédure et l'assembly de votre base de données de test.  
  
 En premier lieu, supprimez la procédure à l'aide de la commande drop procedure.  
  
```  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'hello')  
   drop procedure hello  
```  
  
 Une fois la procédure supprimée, vous pouvez supprimer l'assembly qui contient votre exemple de code.  
  
```  
IF EXISTS (SELECT name FROM sys.assemblies WHERE name = 'helloworld')  
   drop assembly helloworld  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées CLR](http://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33)   
 [Extensions spécifiques du In-Process SQL Server pour ADO.NET](../../../relational-databases/clr-integration-data-access-in-process-ado-net/sql-server-in-process-specific-extensions-to-ado-net.md)   
 [Débogage d’objets de base de données CLR](../../../relational-databases/clr-integration/debugging-clr-database-objects.md)   
 [Sécurité d’intégration du CLR](../../../relational-databases/clr-integration/security/clr-integration-security.md)  
  
  
