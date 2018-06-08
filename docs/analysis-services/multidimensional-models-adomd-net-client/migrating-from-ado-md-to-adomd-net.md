---
title: Migration d’ADO MD vers ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e6d0e3acb77657bcac754b9e6b9392dc2971ce9c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="migrating-from-ado-md-to-adomdnet"></a>Migration d'ADO MD vers ADOMD.NET
  La bibliothèque ADOMD.NET est semblable à la bibliothèque ADO MD (ActiveX Data Objects Multidimensional), extension de la bibliothèque ADO (ActiveX Data Objects) utilisée pour accéder aux données multidimensionnelles dans les applications clientes COM (Component Object Model). ADO MD facilite l'accès aux données multidimensionnelles à partir de langages non managés tels que C++ et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic. ADOMD.NET facilite l'accès aux données analytiques (multidimensionnelles et d'exploration de données) à partir de langages managés tels que [!INCLUDE[msCoName](../../includes/msconame-md.md)] C# et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic .NET. En outre, ADOMD.NET fournit un modèle objet de métadonnées amélioré.  
  
 Il est simple de faire migrer des applications clientes existantes d'ADO MD vers ADOMD.NET, mais il existe plusieurs différences importantes sur le plan de la migration :  
  
 **Pour fournir l’accès de connectivité et des données pour les applications clientes**  
 |ADO MD|ADOMD.NET|  
|------------|---------------|  
|Nécessite des références à Adodb.dll et Adomd.dll.|Nécessite une seule référence à Microsoft.AnalysisServices.AdomdClient.dll.|  
  
 La classe <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> fournit une prise en charge de la connectivité, en plus de l'accès aux métadonnées.  
  
 **Pour récupérer des métadonnées pour les objets multidimensionnels**  
 |ADO MD|ADOMD.NET|  
|------------|---------------|  
|Utilisez la classe de catalogue.|Utilise la propriété <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.Cubes%2A> de la classe <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.|  
  
 **Pour exécuter des requêtes et de retourner des objets de l’ensemble de cellules**  
 |ADO MD|ADOMD.NET|  
|------------|---------------|  
|Utilisez la classe de l’ensemble de cellules.|Utilise la classe <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>.|  
  
 **Pour accéder aux métadonnées qui sont utilisée pour afficher un ensemble de cellules**  
 |ADO MD|ADOMD.NET|  
|------------|---------------|  
|Utilisez la classe de Position.|Utilise les objets <xref:Microsoft.AnalysisServices.AdomdClient.Set> et <xref:Microsoft.AnalysisServices.AdomdClient.Tuple>.|  
  
> [!NOTE]  
>  La classe <xref:Microsoft.AnalysisServices.AdomdClient.Position> est prise en charge pour des raisons de compatibilité descendante.  
  
 **Pour récupérer les métadonnées du modèle d’exploration de données**  
 Dans ADO MD aucune classe ne sont disponibles. Dans ADOMD.NET, utilisez une des collections de d’exploration de données :  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningModelCollection> contient une liste de tous les modèles d'exploration de données de la source de données.  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningServiceCollection> fournit des informations sur les algorithmes d'exploration de données disponibles.  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningStructureCollection> expose des informations sur les structures d'exploration de données du serveur.  
  
 Pour mettre en évidence ces différences, l'exemple de migration suivant compare une application ADO MD existante à une application ADOMD.NET équivalente.  
  
## <a name="looking-at-a-migration-example"></a>Exemple de migration  
 L'exemple de code ADO MD et sont équivalent ADOMD.NET présentés dans cette section exécutent les mêmes actions : création d'une connexion, exécution d'une instruction MDX (Multidimensional Expressions) et récupération de métadonnées et de données. Toutefois, ces deux ensembles de code n'utilisent pas les mêmes objets pour effectuer ces tâches.  
  
### <a name="existing-ado-md-code"></a>Code ADO MD existant  
 L’exemple de code suivant, tiré de la documentation ADO MD 2.8, est écrit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic® 6.0 et utilise ADO MD pour illustrer comment se connecter et d’interroger un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données. Cet exemple ADO MD utilise les objets suivants :  
  
-   Crée une connexion entre un **catalogue** objet.  
  
-   Exécute l’instruction MDX (Multidimensional Expressions) en utilisant le **ensemble de cellules** objet.  
  
-   Récupère les métadonnées et données à partir de la **Position** objet, récupéré à partir de la **ensemble de cellules** objet.  
  
```  
Private Sub cmdCellSettoDebugWindow_Click()  
Dim cat As New ADOMD.Catalog  
Dim cst As New ADOMD.Cellset  
Dim i As Integer  
Dim j As Integer  
Dim k As Integer  
Dim strServer As String  
Dim strSource As String  
Dim strColumnHeader As String  
Dim strRowText As String  
  
On Error GoTo Error_cmdCellSettoDebugWindow_Click  
Screen.MousePointer = vbHourglass  
'*-----------------------------------------------------------------------  
'* Set server to local host.  
'*-----------------------------------------------------------------------  
    strServer = "LOCALHOST"  
  
'*-----------------------------------------------------------------------  
'* Set MDX query string source.  
'*-----------------------------------------------------------------------  
    strSource = strSource & "SELECT "  
    strSource = strSource & "{[Measures].members} ON COLUMNS,"  
    strSource = strSource & _  
        "NON EMPTY [Store].[Store City].members ON ROWS"  
    strSource = strSource & " FROM Sales"  
  
'*-----------------------------------------------------------------------  
'* Set active connection.  
'*-----------------------------------------------------------------------  
        cat.ActiveConnection = "Data Source=" & strServer & _  
            ";Provider=msolap;"  
  
'*-----------------------------------------------------------------------  
'* Set cellset source to MDX query string.  
'*-----------------------------------------------------------------------  
        cst.Source = strSource  
  
'*-----------------------------------------------------------------------  
'* Set cellset active connection to current connection  
'*-----------------------------------------------------------------------  
    Set cst.ActiveConnection = cat.ActiveConnection  
  
'*-----------------------------------------------------------------------  
'* Open cellset.  
'*-----------------------------------------------------------------------  
    cst.Open  
  
'*-----------------------------------------------------------------------  
'* Allow space for row header text.  
'*-----------------------------------------------------------------------  
strColumnHeader = vbTab & vbTab & vbTab & vbTab & vbTab & vbTab  
  
'*-----------------------------------------------------------------------  
'* Loop through column headers.  
'*-----------------------------------------------------------------------  
       For i = 0 To cst.Axes(0).Positions.Count - 1  
            strColumnHeader = strColumnHeader & _  
                cst.Axes(0).Positions(i).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
       Next  
       Debug.Print vbTab & strColumnHeader & vbCrLf  
  
'*-----------------------------------------------------------------------  
'* Loop through row headers and provide data for each row.  
'*-----------------------------------------------------------------------  
        strRowText = ""  
        For j = 0 To cst.Axes(1).Positions.Count - 1  
            strRowText = strRowText & _  
                cst.Axes(1).Positions(j).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
            For k = 0 To cst.Axes(0).Positions.Count - 1  
                strRowText = strRowText & cst(k, j).FormattedValue & _  
                    vbTab & vbTab & vbTab & vbTab  
            Next  
            Debug.Print strRowText & vbCrLf  
            strRowText = ""  
        Next  
  
    Screen.MousePointer = vbDefault  
Exit Sub  
  
Error_cmdCellSettoDebugWindow_Click:  
   Beep  
   Screen.MousePointer = vbDefault  
   MsgBox "The following error has occurred:" & vbCrLf & _  
      Err.Description, vbCritical, " Error!"  
   Exit Sub  
End Sub  
```  
  
### <a name="equivalent-adomdnet-code"></a>Code ADOMD.NET équivalent  
 L'exemple suivant, écrit en Visual Basic .NET et utilisant ADOMD.NET, montre comment effectuer les mêmes actions que l'exemple Visual Basic 6.0 précédent. La principale différence entre l'exemple suivant et l'exemple ADO MD présenté plus haut réside dans les objets utilisés pour effectuer les actions. L'exemple ADOMD.NET utilise les objets suivants :  
  
-   Crée une connexion avec un **AdomdConnection** objet.  
  
-   Exécute l’instruction MDX en utilisant un **AdomdCommand** objet.  
  
-   Récupère les métadonnées et données à partir de la **définir** objet, récupéré à partir de la **ensemble de cellules** objet.  
  
```  
Private Sub DisplayCellSetInOutputWindow()  
    Dim conn As AdomdConnection  
    Dim cmd As AdomdCommand  
    Dim cst As CellSet  
    Dim i As Integer  
    Dim j As Integer  
    Dim k As Integer  
    Dim strServer As String = "LOCALHOST"  
    Dim strSource As String = "SELECT [Measures].members ON COLUMNS, " & _  
        "NON EMPTY [Store].[Store City].members ON ROWS FROM SALES"  
    Dim strOutput As New System.IO.StringWriter  
  
    '*-----------------------------------------------------------------------  
    '* Open connection.  
    '*-----------------------------------------------------------------------  
    Try  
        ' Create a new AdomdConnection object, providing the connection  
        ' string.  
        conn = New AdomdConnection("Data Source=" & strServer & _  
        ";Provider=msolap;")  
        ' Open the connection.  
        conn.Open()  
    Catch ex As Exception  
        Throw New ApplicationException( _  
            "An error occurred while connecting.")  
    End Try  
  
    Try  
    '*-----------------------------------------------------------------------  
    '* Open cellset.  
    '*-----------------------------------------------------------------------  
        ' Create a new AdomdCommand object, providing the MDX query string.  
        cmd = New AdomdCommand(strSource, conn)  
        ' Run the command and return a CellSet object.  
        cst = cmd.ExecuteCellSet()  
  
    '*-----------------------------------------------------------------------  
    '* Concatenate output.  
    '*-----------------------------------------------------------------------  
  
    ' Include spacing to account for row headers.  
    strOutput.Write(vbTab, 6)  
  
    ' Iterate through the first axis of the CellSet object and  
    ' retrieve column headers.  
    For i = 0 To cst.Axes(0).Set.Tuples.Count - 1  
        strOutput.Write(cst.Axes(0).Set.Tuples(i).Members(0).Caption)  
        strOutput.Write(vbTab, 4)  
    Next  
    strOutput.WriteLine()  
  
    ' Iterate through the second axis of the CellSet object and  
    ' retrieve row headers and cell data.  
    For j = 0 To cst.Axes(1).Set.Tuples.Count - 1  
        ' Append the row header.  
        strOutput.Write(cst.Axes(1).Set.Tuples(j).Members(0).Caption)  
        strOutput.Write(vbTab, 4)  
  
        ' Append the cell data for that row.  
        For k = 0 To cst.Axes(0).Set.Tuples.Count - 1  
            strOutput.Write(cst.Cells(k, j).FormattedValue)  
            strOutput.Write(vbTab, 4)  
        Next  
        strOutput.WriteLine()  
    Next  
  
    ' Display the output.  
    Debug.WriteLine(strOutput.ToString)  
  
    '*-----------------------------------------------------------------------  
    '* Release resources.  
    '*-----------------------------------------------------------------------  
        conn.Close()  
    Catch ex As Exception  
        ' Ignore or handle errors.  
    Finally  
        cst = Nothing  
        cmd = Nothing  
        conn = Nothing  
    End Try  
End Sub  
```  
  
  
