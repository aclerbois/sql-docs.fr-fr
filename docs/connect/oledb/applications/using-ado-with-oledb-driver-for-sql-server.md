---
title: À l’aide d’ADO avec le pilote OLE DB pour SQL Server | Documents Microsoft
description: À l’aide d’ADO avec le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, ADO
- data access [OLE DB Driver for SQL Server], ADO
- ADO [OLE DB Driver for SQL Server]
- MSOLEDBSQL, ADO
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: c1244669bcfeea5008640ed00b6f8e579ce71139
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-ado-with-ole-db-driver-for-sql-server"></a>À l’aide d’ADO avec le pilote OLE DB pour SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Pour tirer parti des nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] tels que des jeux de résultats actifs multiples (MARS), les notifications de requête, les types définis par l’utilisateur (UDT) ou le nouveau **xml** type de données, les applications existantes qui utilisent les contrôles ActiveX Data Objects (ADO) doit utiliser le pilote OLE DB pour SQL Server en tant que leur fournisseur d’accès aux données.  
  
 Pour permettre à ADO d’utiliser les nouvelles fonctionnalités des versions récentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], certaines améliorations ont été apportées au pilote OLE DB pour SQL Server qui étend les fonctionnalités principales d’OLE DB. Ces améliorations permettent aux applications ADO d’utiliser plus récente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fonctionnalités et d’utiliser deux types de données introduits dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]: **xml** et **udt**. Ces améliorations exploitent également des améliorations apportées à la **varchar**, **nvarchar**, et **varbinary** des types de données. Pilote OLE DB pour SQL Server ajoute la propriété d’initialisation SSPROP_INIT_DATATYPECOMPATIBILITY à la propriété DBPROPSET_SQLSERVERDBINIT définie pour une utilisation par les applications ADO, afin que les nouveaux types de données sont exposées de manière compatible avec ADO. En outre, le pilote OLE DB pour SQL Server définit également un mot-clé de chaîne de connexion nommé **DataTypeCompatibility** qui est définie dans la chaîne de connexion.  

> [!NOTE]  
>  Les applications ADO existantes peuvent accéder et mettre à jour des valeurs de champ binaire et texte XML, UDT et de grande valeur à l'aide du fournisseur SQLOLEDB. Le nouveau supérieure **varchar (max)**, **nvarchar (max)**, et **varbinary (max)** des types de données sont retournées en tant que types ADO **adLongVarChar**, **adLongVarWChar** et **adLongVarBinary** respectivement. Colonnes XML sont retournées en tant que **adLongVarChar**, et les colonnes UDT sont retournées en tant que **adVarBinary**. Toutefois, si vous utilisez le pilote OLE DB pour SQL Server (MSOLEDBSQL) au lieu de SQLOLEDB, vous devez vous assurer d’affecter le **DataTypeCompatibility** mot clé « 80 » afin que les nouveaux types de données mappent correctement aux types de données ADO.  

## <a name="enabling-ole-db-driver-for-sql-server-from-ado"></a>L’activation du pilote OLE DB pour SQL Server à partir d’ADO  
 Pour activer l’utilisation du pilote OLE DB pour SQL Server, les applications ADO doivent implémenter les mots clés suivants dans leurs chaînes de connexion :  

-   `Provider=MSOLEDBSQL`  

-   `DataTypeCompatibility=80`  

 Pour plus d’informations sur le ADO de chaîne de connexion mots clés pris en charge dans le pilote OLE DB pour SQL Server, consultez [à l’aide de mots clés de chaîne de connexion avec le pilote OLE DB pour SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

 Voici un exemple de l’établissement d’une chaîne de connexion ADO est entièrement compatible avec le pilote OLE DB pour SQL Server, y compris l’activation de la fonctionnalité MARS :  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  
```  

## <a name="examples"></a>Exemples  
 Les sections suivantes fournissent des exemples de la façon dont vous pouvez utiliser ADO avec le pilote OLE DB pour SQL Server.  

### <a name="retrieving-xml-column-data"></a>Extraction de données de colonnes XML  
 Dans cet exemple, un jeu d’enregistrements est utilisé pour récupérer et afficher les données d’une colonne XML dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **AdventureWorks** base de données exemple.  

```  
Dim con As New ADODB.Connection  
Dim rst As New ADODB.Recordset  
Dim sXMLResult As String  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _   
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the xml data as a recordset.  
Set rst.ActiveConnection = con  
rst.Source = "SELECT AdditionalContactInfo FROM Person.Contact " _  
   & "WHERE AdditionalContactInfo IS NOT NULL"  
rst.Open  

' Display the data in the recordset.  
While (Not rst.EOF)  
   sXMLResult = rst.Fields("AdditionalContactInfo").Value  
   Debug.Print (sXMLResult)  
   rst.MoveNext  
End While  

con.Close  
Set con = Nothing  
```  

> [!NOTE]  
>  Le filtrage de jeu d'enregistrements n'est pas pris en charge avec les colonnes XML. S'il est utilisé, une erreur est retournée.  

### <a name="retrieving-udt-column-data"></a>Extraction de données de colonnes UDT  
 Dans cet exemple, un **commande** objet est utilisé pour exécuter une requête SQL qui retourne un type UDT, les données de l’UDT sont mis à jour, puis les nouvelles données sont réinsérées dans la base de données. Cet exemple suppose que le **Point** UDT a déjà été inscrit dans la base de données.  

```  
Dim con As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
Dim strOldUDT As String  
Dim strNewUDT As String  
Dim aryTempUDT() As String  
Dim strTempID As String  
Dim i As Integer  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;"  

con.Open  

' Get the UDT value.  
Set cmd.ActiveConnection = con  
cmd.CommandText = "SELECT ID, Pnt FROM dbo.Points.ToString()"  
Set rst = cmd.Execute  
strTempID = rst.Fields(0).Value  
strOldUDT = rst.Fields(1).Value  

' Do something with the UDT by adding i to each point.  
arytempUDT = Split(strOldUDT, ",")  
i = 3  
strNewUDT = LTrim(Str(Int(aryTempUDT(0)) + i)) + "," + _  
   LTrim(Str(Int(aryTempUDT(1)) + i))  

' Insert the new value back into the database.  
cmd.CommandText = "UPDATE dbo.Points SET Pnt = '" + strNewUDT + _  
   "' WHERE ID = '" + strTempID + "'"  
cmd.Execute  

con.Close  
Set con = Nothing  
```  

### <a name="enabling-and-using-mars"></a>Activation et utilisation de MARS  
 Dans cet exemple, la chaîne de connexion est construite pour activer MARS par le pilote OLE DB pour SQL Server, puis les deux objets recordset sont créés pour l’exécuter à l’aide de la même connexion.  

```  
Dim con As New ADODB.Connection  

con.ConnectionString = "Provider=MSOLEDBSQL;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  

Dim recordset1 As New ADODB.Recordset  
Dim recordset2 As New ADODB.Recordset  

Dim recordsaffected As Integer  
Set recordset1 =  con.Execute("SELECT * FROM Table1", recordsaffected, adCmdText)  
Set recordset2 =  con.Execute("SELECT * FROM Table2", recordsaffected, adCmdText)  

con.Close  
Set con = Nothing  
```  

 Dans les versions antérieures du fournisseur OLE DB, ce code provoquait la création d'une connexion implicite lors de la deuxième exécution car un seul jeu de résultats actif par connexion pouvait être ouvert. La connexion implicite n'étant pas mise dans le pool de connexions OLE DB, cela provoquait une charge supplémentaire. Avec la fonctionnalité MARS exposée par le pilote OLE DB pour SQL Server, vous obtenez des résultats actifs multiples sur une connexion.  

## <a name="see-also"></a>Voir aussi  
 [Génération d’applications avec OLE DB Driver pour SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
