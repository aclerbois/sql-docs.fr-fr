---
title: À l’aide d’ADO avec SQL Server Native Client | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, ADO
- data access [SQL Server Native Client], ADO
- ADO [SQL Server Native Client]
- SQLNCLI, ADO
ms.assetid: 118a7cac-4c0d-44fd-b63e-3d542932d239
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 88d1c839c460395b80b69f417c50a65dd9319c19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-ado-with-sql-server-native-client"></a>Utilisation d'ADO avec SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Pour tirer parti des nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] tels que des jeux de résultats actifs multiples (MARS), les notifications de requête, les types définis par l’utilisateur (UDT) ou le nouveau **xml** de type de données, les applications existantes qui utilisent ActiveX Data Objects (ADO) doivent utiliser le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client comme leur fournisseur d’accès aux données.  
  
 Si vous n'avez pas besoin d'utiliser ces nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], il n'est pas nécessaire d'utiliser le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ; vous pouvez continuer à utiliser votre fournisseur d'accès aux données actuel, qui est en général SQLOLEDB. Si vous améliorez une application existante et que vous devez utiliser les nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], vous devez utiliser le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
> [!NOTE]  
>  Si vous développez une nouvelle application, il est recommandé d'envisager l'utilisation d'ADO.NET et du fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au lieu de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client afin d'accéder à toutes les nouvelles fonctionnalités des versions récentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d'informations sur le fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez la documentation du kit de développement logiciel SDK .NET Framework pour ADO.NET.  
  
 Pour permettre à ADO d'utiliser les nouvelles fonctionnalités des récentes versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], certaines améliorations ont été apportées au fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client afin d'étendre les fonctionnalités principales d'OLE DB. Ces améliorations permettent aux applications ADO d’utiliser plus récente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fonctionnalités et d’utiliser deux types de données introduits dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]: **xml** et **udt**. Ces améliorations exploitent également des améliorations apportées à la **varchar**, **nvarchar**, et **varbinary** des types de données. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ajoute la propriété d’initialisation SSPROP_INIT_DATATYPECOMPATIBILITY à la propriété DBPROPSET_SQLSERVERDBINIT définie pour une utilisation par les applications ADO, afin que les nouveaux types de données sont exposées de manière compatible avec ADO. En outre, le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB fournisseur définit également un mot-clé de chaîne de connexion nommé **DataTypeCompatibility** qui est définie dans la chaîne de connexion.  
  
> [!NOTE]  
>  Les applications ADO existantes peuvent accéder et mettre à jour des valeurs de champ binaire et texte XML, UDT et de grande valeur à l'aide du fournisseur SQLOLEDB. Le nouveau supérieure **varchar (max)**, **nvarchar (max)**, et **varbinary (max)** des types de données sont retournées en tant que types ADO **adLongVarChar**, **adLongVarWChar** et **adLongVarBinary** respectivement. Colonnes XML sont retournées en tant que **adLongVarChar**, et les colonnes UDT sont retournées en tant que **adVarBinary**. Toutefois, si vous utilisez la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le fournisseur OLE DB Native Client (SQLNCLI11) au lieu de SQLOLEDB, vous devez vous assurer d’affecter le **DataTypeCompatibility** mot clé « 80 » afin que les nouveaux types de données mappent correctement aux types de données ADO.  
  
## <a name="enabling-sql-server-native-client-from-ado"></a>Activation de SQL Server Native Client à partir d'ADO  
 Pour activer l’utilisation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, les applications ADO doivent implémenter les mots clés suivants dans leurs chaînes de connexion :  
  
-   `Provider=SQLNCLI11`  
  
-   `DataTypeCompatibility=80`  
  
 Pour plus d’informations sur le ADO de chaîne de connexion mots clés pris en charge dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Voici un exemple de l’établissement d’une chaîne de connexion ADO est entièrement compatible avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, y compris l’activation de la fonctionnalité MARS :  
  
```  
Dim con As New ADODB.Connection  
  
con.ConnectionString = "Provider=SQLNCLI11;" _  
         & "Server=(local);" _  
         & "Database=AdventureWorks;" _   
         & "Integrated Security=SSPI;" _  
         & "DataTypeCompatibility=80;" _  
         & "MARS Connection=True;"  
con.Open  
```  
  
## <a name="examples"></a>Exemples  
 Les sections suivantes fournissent des exemples de la façon dont vous pouvez utiliser ADO avec le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif.  
  
### <a name="retrieving-xml-column-data"></a>Extraction de données de colonnes XML  
 Dans cet exemple, un jeu d’enregistrements est utilisé pour récupérer et afficher les données d’une colonne XML dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **AdventureWorks** base de données exemple.  
  
```  
Dim con As New ADODB.Connection  
Dim rst As New ADODB.Recordset  
Dim sXMLResult As String  
  
con.ConnectionString = "Provider=SQLNCLI11;" _  
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
  
con.ConnectionString = "Provider=SQLNCLI11;" _  
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
 Dans cet exemple, la chaîne de connexion est construite pour activer MARS par le biais du [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif et puis deux objets recordset sont créés pour l’exécuter à l’aide de la même connexion.  
  
```  
Dim con As New ADODB.Connection  
  
con.ConnectionString = "Provider=SQLNCLI11;" _  
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
  
 Dans les versions antérieures du fournisseur OLE DB, ce code provoquait la création d'une connexion implicite lors de la deuxième exécution car un seul jeu de résultats actif par connexion pouvait être ouvert. La connexion implicite n'étant pas mise dans le pool de connexions OLE DB, cela provoquait une charge supplémentaire. Avec la fonctionnalité MARS exposée par le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif, vous obtenez des résultats actifs multiples sur une connexion.  
  
## <a name="see-also"></a>Voir aussi  
 [Génération d’Applications avec SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
