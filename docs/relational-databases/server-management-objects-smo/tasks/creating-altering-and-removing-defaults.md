---
title: Création, modification et suppression des valeurs par défaut | Documents Microsoft
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
- defaults [SMO]
ms.assetid: c30ac3b9-8150-4264-ba4c-c549f44261ab
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 615d56e469773817b7e43a2dcc80f13602b802dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-altering-and-removing-defaults"></a>Création, modification et suppression des valeurs par défaut
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Dans SMO ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Objects), la contrainte par défaut est représentée par l'objet <xref:Microsoft.SqlServer.Management.Smo.Default>.  
  
 La propriété <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Default> est utilisée pour définir la valeur à insérer. Il peut s'agir d'une constante ou d'une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] qui retourne une valeur constante, telle que GETDATE(). La propriété <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A> ne peut pas être modifiée en utilisant la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.Alter%2A>. L'objet <xref:Microsoft.SqlServer.Management.Smo.Default> doit d'abord être supprimé, puis recréé.  
  
## <a name="example"></a>Exemple  
 Pour utiliser un exemple de code fourni, vous devrez sélectionner l'environnement, le modèle et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-altering-and-removing-a-default-in-visual-basic"></a>Création, modification et suppression d'une valeur par défaut en Visual Basic  
 Cet exemple de code montre comment créer une valeur par défaut qui est du texte simple et une autre valeur par défaut qui est une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La valeur par défaut doit être attachée à la colonne à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.BindToColumn%2A>, puis détachée à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.UnbindFromColumn%2A>.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define a Default object variable by supplying the parent database and the default name 
'in the constructor.
Dim def As [Default]
def = New [Default](db, "Test_Default2")
'Set the TextHeader and TextBody properties that define the default.
def.TextHeader = "CREATE DEFAULT [Test_Default2] AS"
def.TextBody = "GetDate()"
'Create the default on the instance of SQL Server.
def.Create()
'Declare a Column object variable and reference a column in the AdventureWorks2012 database.
Dim col As Column
col = db.Tables("SpecialOffer", "Sales").Columns("StartDate")
'Bind the default to the column.
def.BindToColumn("SpecialOffer", "StartDate", "Sales")
'Unbind the default from the column and remove it from the database.
def.UnbindFromColumn("SpecialOffer", "StartDate", "Sales")
def.Drop()
```
  
## <a name="creating-altering-and-removing-a-default-in-visual-c"></a>Création, modification et suppression d'une valeur par défaut en Visual C#  
 Cet exemple de code montre comment créer une valeur par défaut qui est du texte simple et une autre valeur par défaut qui est une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La valeur par défaut doit être attachée à la colonne à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.BindToColumn%2A>, puis détachée à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.UnbindFromColumn%2A>.  
  
```csharp  
{  
  
          Server srv = new Server();  
  
            //Reference the AdventureWorks2012 database.   
            Database  db = srv.Databases["AdventureWorks2012"];  
  
            //Define a Default object variable by supplying the parent database and the default name   
            //in the constructor.   
            Default def = new Default(db, "Test_Default2");  
  
            //Set the TextHeader and TextBody properties that define the default.   
            def.TextHeader = "CREATE DEFAULT [Test_Default2] AS";  
            def.TextBody = "GetDate()";  
  
            //Create the default on the instance of SQL Server.   
            def.Create();  
  
            //Bind the default to a column in a table in AdventureWorks2012  
            def.BindToColumn("SpecialOffer", "StartDate", "Sales");  
  
            //Unbind the default from the column and remove it from the database.   
            def.UnbindFromColumn("SpecialOffer", "StartDate", "Sales");  
            def.Drop();  
        }  
```  
  
## <a name="creating-altering-and-removing-a-default-in-powershell"></a>Création, modification et suppression d'une valeur par défaut dans PowerShell  
 Cet exemple de code montre comment créer une valeur par défaut qui est du texte simple et une autre valeur par défaut qui est une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La valeur par défaut doit être attachée à la colonne à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.BindToColumn%2A>, puis détachée à l'aide de la méthode <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.UnbindFromColumn%2A>.  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
#Define a Default object variable by supplying the parent database and the default name in the constructor.  
$def = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Default `  
-argumentlist $db, "Test_Default2"  
  
#Set the TextHeader and TextBody properties that define the default.   
$def.TextHeader = "CREATE DEFAULT [Test_Default2] AS"  
$def.TextBody = "GetDate()"  
  
#Create the default on the instance of SQL Server.   
$def.Create()  
  
#Bind the default to the column.   
$def.BindToColumn("SpecialOffer", "StartDate", "Sales")  
  
#Unbind the default from the column and remove it from the database.   
$def.UnbindFromColumn("SpecialOffer", "StartDate", "Sales")  
$def.Drop()  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.SqlServer.Management.Smo.Default>  
  
  
