---
title: À l’aide du partitionnement des tables et Index | Documents Microsoft
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
- partitions [SMO]
- storing data [SMO]
- partitioned tables [SQL Server], SMO
- partitioned indexes [SQL Server], SMO
ms.assetid: 0e682d7e-86c3-4d73-950d-aa692d46cb62
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c6dca91e1be799d4fd666432820a7839718d3e28
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-table-and-index-partitioning"></a>Utilisation du partitionnement des tables et des index
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Données peuvent être stockées à l’aide d’algorithmes de stockage fournis par [Partitioned Tables and Indexes](../../../relational-databases/partitions/partitioned-tables-and-indexes.md). Le partitionnement permet de rendre des tables et des index volumineux plus gérables et plus évolutifs.  
  
## <a name="index-and-table-partitioning"></a>Partitionnement des tables et des index  
 Cette fonctionnalité permet de répartir les données des tables et index sur plusieurs groupes de fichiers dans des partitions. Une fonction de partition définit comment les lignes d'une table ou d'un index sont mappées à un ensemble de partitions selon les valeurs de certaines colonnes, appelées « colonnes de partitionnement ». Un schéma de partition mappe chaque partition spécifiée par la fonction de partition avec un groupe de fichiers. Vous pouvez ainsi développer des stratégies d'archivage qui permettent de répartir les tables sur plusieurs groupes de fichiers, et par conséquent sur plusieurs périphériques physiques.  
  
 Le <xref:Microsoft.SqlServer.Management.Smo.Database> objet contient une collection de <xref:Microsoft.SqlServer.Management.Smo.PartitionFunction> objets qui représentent les fonctions de partition implémentées et une collection de <xref:Microsoft.SqlServer.Management.Smo.PartitionScheme> les objets qui décrivent comment les données sont mappées aux groupes de fichiers.  
  
 Chaque objet <xref:Microsoft.SqlServer.Management.Smo.Table> et <xref:Microsoft.SqlServer.Management.Smo.Index> spécifie quel schéma de partition il utilise dans la propriété <xref:Microsoft.SqlServer.Management.Smo.PartitionScheme> et spécifie les colonnes dans la <xref:Microsoft.SqlServer.Management.Smo.PartitionSchemeParameterCollection>.  
  
## <a name="example"></a>Exemple  
 Dans les exemples de code suivants, vous devez sélectionner l'environnement, le modèle et le langage de programmation à utiliser pour créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="setting-up-a-partition-scheme-for-a-table-in-visual-c"></a>Installation d'un schéma de partition pour une table en Visual C#  
 L’exemple de code montre comment créer une fonction de partition et un schéma de partition pour la `TransactionHistory` de table dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données exemple. Les partitions sont organisées par date dans le but de déplacer les enregistrements anciens vers la table `TransactionHistoryArchive` .  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Reference the AdventureWorks2012 database.   
Database db;   
db = srv.Databases("AdventureWorks2012");   
//Define and create three new file groups on the database.   
FileGroup fg2;   
fg2 = new FileGroup(db, "Second");   
fg2.Create();   
FileGroup fg3;   
fg3 = new FileGroup(db, "Third");   
fg3.Create();   
FileGroup fg4;   
fg4 = new FileGroup(db, "Fourth");   
fg4.Create();   
//Define a partition function by supplying the parent database and name arguments in the constructor.   
PartitionFunction pf;   
pf = new PartitionFunction(db, "TransHistPF");   
//Add a partition function parameter that specifies the function uses a DateTime range type.   
PartitionFunctionParameter pfp;   
pfp = new PartitionFunctionParameter(pf, DataType.DateTime);   
pf.PartitionFunctionParameters.Add(pfp);   
//Specify the three dates that divide the data into four partitions.   
object[] val;   
val = new object[] {"1/1/2003", "1/1/2004", "1/1/2005"};   
pf.RangeValues = val;   
//Create the partition function.   
pf.Create();   
//Define a partition scheme by supplying the parent database and name arguments in the constructor.   
PartitionScheme ps;   
ps = new PartitionScheme(db, "TransHistPS");   
//Specify the partition function and the filegroups required by the partition scheme.   
ps.PartitionFunction = "TransHistPF";   
ps.FileGroups.Add("PRIMARY");   
ps.FileGroups.Add("second");   
ps.FileGroups.Add("Third");   
ps.FileGroups.Add("Fourth");   
//Create the partition scheme.   
ps.Create();   
}   
```  
  
## <a name="setting-up-a-partition-scheme-for-a-table-in-powershell"></a>Installation d'un schéma de partition pour une table dans PowerShell  
 L’exemple de code montre comment créer une fonction de partition et un schéma de partition pour la `TransactionHistory` de table dans le [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] base de données exemple. Les partitions sont organisées par date dans le but de déplacer les enregistrements anciens vers la table `TransactionHistoryArchive` .  
  
```powershell  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\default  
  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
$db = $srv.Databases["AdventureWorks"]  
#Create four filegroups  
$fg1 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Filegroup -argumentlist $db, "First"  
$fg2 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Filegroup -argumentlist $db, "Second"  
$fg3 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Filegroup -argumentlist $db, "Third"  
$fg4 = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Filegroup -argumentlist $db, "Fourth"  
  
#Define a partition function by supplying the parent database and name arguments in the constructor.  
$pf =  New-Object -TypeName Microsoft.SqlServer.Management.SMO.PartitionFunction -argumentlist $db, "TransHistPF"  
$T = [Microsoft.SqlServer.Management.SMO.DataType]::DateTime  
$T  
$T.GetType()  
#Add a partition function parameter that specifies the function uses a DateTime range type.  
$pfp =  New-Object -TypeName Microsoft.SqlServer.Management.SMO.PartitionFunctionParameter -argumentlist $pf, $T  
  
#Specify the three dates that divide the data into four partitions.   
#Create an array of type object to hold the partition data  
$val = "1/1/2003"."1/1/2004","1/1/2005"  
$pf.RangeValues = $val  
$pf  
#Create the partition function  
$pf.Create()  
  
#Create partition scheme  
$ps = New-Object -TypeName Microsoft.SqlServer.Management.SMO.PartitionScheme -argumentlist $db, "TransHistPS"  
$ps.PartitionFunction = "TransHistPF"  
  
#add the filegroups to the scheme   
$ps.FileGroups.Add("PRIMARY")  
$ps.FileGroups.Add("Second")  
$ps.FileGroups.Add("Third")  
$ps.FileGroups.Add("Fourth")  
  
#Create it at the server  
$ps.Create()  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et index partitionnés](../../../relational-databases/partitions/partitioned-tables-and-indexes.md)  
  
  
