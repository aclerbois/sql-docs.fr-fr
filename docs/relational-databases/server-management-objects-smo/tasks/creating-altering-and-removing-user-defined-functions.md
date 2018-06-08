---
title: Création, modification et suppression de fonctions définies par l’utilisateur | Documents Microsoft
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
- user-defined functions [SMO]
ms.assetid: 0ebebd3b-0775-41c2-989d-aa4cf81af12a
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 70176012dc61676a9c6b2193c8ca9034aa871284
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-altering-and-removing-user-defined-functions"></a>Création, modification et suppression de fonctions définies par l'utilisateur
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]
  Le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> objet fournit une fonctionnalité qui permet aux utilisateurs de gérer par programmation des fonctions définies par l’utilisateur dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les fonctions définies par l'utilisateur prennent en charge les paramètres d'entrée et de sortie, ainsi que les références directes aux colonnes de table.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiert des assemblys à inscrire dans une base de données que ceux-ci peuvent être utilisés dans les procédures stockées, les types de données définis par l’utilisateur, les déclencheurs et les fonctions définies par l’utilisateur. SMO prend en charge cette fonctionnalité avec l'objet <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly>.  
  
 Le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> objet fait référence à l’assembly .NET avec le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.AssemblyName%2A>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.ClassName%2A>, et <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.MethodName%2A> propriétés.  
  
 Lorsque le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> objet fait référence à un assembly .NET, vous devez inscrire l’assembly en créant un <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly> objet et en l’ajoutant à la <xref:Microsoft.SqlServer.Management.Smo.SqlAssemblyCollection> objet, qui appartienne à la <xref:Microsoft.SqlServer.Management.Smo.Database> objet.  
  
## <a name="example"></a>Exemple  
 Pour utiliser un exemple de code fourni, vous devrez sélectionner l'environnement, le modèle et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-scalar-user-defined-function-in-visual-basic"></a>Création d'une fonction scalaire définie par l'utilisateur en Visual Basic  
 Cet exemple de code montre comment créer et supprimer une fonction scalaire définie par l’utilisateur qui a une entrée <xref:System.DateTime> paramètre d’objet et un entier type de retour en [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]. La fonction définie par l’utilisateur est créée sur le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données. L'exemple crée une fonction définie par l'utilisateur, ISOweek, qui accepte un argument de date et calcule le numéro de semaine ISO. Pour que ce calcul puisse être correctement réalisé, la valeur 1 doit être affectée à l'option de base de données DATEFIRST avant l'appel de la fonction.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 2008R2 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.
Dim udf As UserDefinedFunction
udf = New UserDefinedFunction(db, "IsOWeek")
'Set the TextMode property to false and then set the other properties.
udf.TextMode = False
udf.DataType = DataType.Int
udf.ExecutionContext = ExecutionContext.Caller
udf.FunctionType = UserDefinedFunctionType.Scalar
udf.ImplementationType = ImplementationType.TransactSql
'Add a parameter.
Dim par As UserDefinedFunctionParameter
par = New UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime)
udf.Parameters.Add(par)
'Set the TextBody property to define the user defined function.
udf.TextBody = "BEGIN  DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"
'Create the user defined function on the instance of SQL Server.
udf.Create()
'Remove the user defined function.
udf.Drop()
``` 
  
## <a name="creating-a-scalar-user-defined-function-in-visual-c"></a>Création d'une fonction scalaire définie par l'utilisateur en Visual C#  
 Cet exemple de code montre comment créer et supprimer une fonction scalaire définie par l’utilisateur qui a une entrée <xref:System.DateTime> paramètre d’objet et un entier type de retour en [!INCLUDE[csprcs](../../../includes/csprcs-md.md)]. La fonction définie par l’utilisateur est créée sur le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données. L'exemple crée la fonction définie par l'utilisateur. `ISOweek`. Cette fonction prend un argument date et calcule le numéro de semaine ISO. Pour que ce calcul puisse être correctement réalisé, la valeur `DATEFIRST` doit être affectée à l'option de base de données `1` avant l'appel de la fonction.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
           Server srv = new Server();  
            //Reference the AdventureWorks2012 database.   
           Database db = srv.Databases["AdventureWorks2012"];  
  
            //Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.   
            UserDefinedFunction udf = new UserDefinedFunction(db, "IsOWeek");  
  
            //Set the TextMode property to false and then set the other properties.   
            udf.TextMode = false;  
            udf.DataType = DataType.Int;  
            udf.ExecutionContext = ExecutionContext.Caller;  
            udf.FunctionType = UserDefinedFunctionType.Scalar;  
            udf.ImplementationType = ImplementationType.TransactSql;  
  
            //Add a parameter.   
  
     UserDefinedFunctionParameter par = new UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime);  
            udf.Parameters.Add(par);  
  
            //Set the TextBody property to define the user-defined function.   
            udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;";  
  
            //Create the user-defined function on the instance of SQL Server.   
            udf.Create();  
  
            //Remove the user-defined function.   
            udf.Drop();  
        }  
```  
  
## <a name="creating-a-scalar-user-defined-function-in-powershell"></a>Création d'une fonction scalaire définie par l'utilisateur dans PowerShell  
 Cet exemple de code montre comment créer et supprimer une fonction scalaire définie par l’utilisateur qui a une entrée <xref:System.DateTime> paramètre d’objet et un entier type de retour en [!INCLUDE[csprcs](../../../includes/csprcs-md.md)]. La fonction définie par l’utilisateur est créée sur le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données. L'exemple crée la fonction définie par l'utilisateur. `ISOweek`. Cette fonction prend un argument date et calcule le numéro de semaine ISO. Pour que ce calcul puisse être correctement réalisé, la valeur `DATEFIRST` doit être affectée à l'option de base de données `1` avant l'appel de la fonction.  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
# Define a user defined function object variable by supplying the parent database and name arguments in the constructor.   
$udf  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunction `  
-argumentlist $db, "IsOWeek"  
  
# Set the TextMode property to false and then set the other properties.   
$udf.TextMode = $false  
$udf.DataType = [Microsoft.SqlServer.Management.SMO.DataType]::Int   
$udf.ExecutionContext = [Microsoft.SqlServer.Management.SMO.ExecutionContext]::Caller  
$udf.FunctionType = [Microsoft.SqlServer.Management.SMO.UserDefinedFunctionType]::Scalar  
$udf.ImplementationType = [Microsoft.SqlServer.Management.SMO.ImplementationType]::TransactSql  
  
# Define a Parameter object variable by supplying the parent function, name and type arguments in the constructor.  
$type = [Microsoft.SqlServer.Management.SMO.DataType]::DateTime  
$par  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunctionParameter `  
-argumentlist $udf, "@DATE",$type  
  
# Add the parameter to the function  
$udf.Parameters.Add($par)  
  
#Set the TextBody property to define the user-defined function.   
$udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"  
  
# Create the user-defined function on the instance of SQL Server.   
$udf.Create()  
  
# Remove the user-defined function.   
$udf.Drop()  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction>  
  
  
