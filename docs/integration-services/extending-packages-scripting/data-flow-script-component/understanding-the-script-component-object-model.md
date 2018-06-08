---
title: Présentation du modèle objet du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
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
- Script component [Integration Services], object model
ms.assetid: 2a0aae82-39cc-4423-b09a-72d2f61033bd
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 81d1f7170474ff1858b61a0d5facf190b635cac6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-the-script-component-object-model"></a>Présentation du modèle objet du composant Script
  Comme expliqué dans [Codage et débogage du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md), le projet de composant Script contient trois éléments de projet :  
  
1.  L’élément **ScriptMain**, qui contient la classe **ScriptMain** dans laquelle vous écrivez votre code. La classe **ScriptMain** hérite de la classe **UserComponent**.  
  
2.  L’élément **ComponentWrapper**, qui contient la classe **UserComponent**, une instance de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> contenant les méthodes et les propriétés qui permettent de traiter les données et d’interagir avec le package. L’élément **ComponentWrapper** contient également les classes de collection **Connections** et **Variables**.  
  
3.  L’élément **BufferWrapper**, qui contient des classes qui héritent de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> pour chaque entrée et sortie, et des propriétés typées pour chaque colonne.  
  
 Les objets, méthodes et propriétés présentés dans cette rubrique vous permettent d’écrire du code dans l’élément **ScriptMain**. Chaque composant n'utilise pas toutes les méthodes répertoriées ici ; cependant, lorsque ces méthodes sont utilisées, elles le sont dans l'ordre indiqué.  
  
 La classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> ne contient pas de code d'implémentation pour les méthodes présentées dans cette rubrique. Il est donc inutile, mais sans conséquence, d'ajouter un appel à l'implémentation de la classe de base à votre propre implémentation de la méthode.  
  
 Pour plus d’informations sur l’utilisation des méthodes et des propriétés de ces classes dans un type de composant Script particulier, consultez la section [Autres exemples de composants Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md). Les rubriques d'exemples contiennent également des exemples de code complets.  
  
## <a name="acquireconnections-method"></a>Méthode AcquireConnections  
 Généralement, les sources et les destinations doivent se connecter à une source de données externe. Substituez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.AcquireConnections%2A> de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> pour extraire la connexion ou les informations de connexion du gestionnaire de connexions approprié.  
  
 L’exemple suivant retourne un **System.Data.SqlClient.SqlConnection** à partir d’un gestionnaire de connexions ADO.NET.  
  
```vb  
Dim connMgr As IDTSConnectionManager100  
Dim sqlConn As SqlConnection  
  
Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
    connMgr = Me.Connections.MyADONETConnection  
    sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
End Sub  
```  
  
 L’exemple suivant retourne un chemin complet et un nom de fichier à partir d’un gestionnaire de connexions de fichiers plats, puis ouvre le fichier à l’aide de **System.IO.StreamReader**.  
  
```vb  
Private textReader As StreamReader  
Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
    Dim connMgr As IDTSConnectionManager100 = _  
        Me.Connections.MyFlatFileSrcConnectionManager  
    Dim exportedAddressFile As String = _  
        CType(connMgr.AcquireConnection(Nothing), String)  
    textReader = New StreamReader(exportedAddressFile)  
  
End Sub  
```  
  
## <a name="preexecute-method"></a>Méthode PreExecute  
 Substituez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PreExecute%2A> de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> lorsque vous devez effectuer un traitement une seule fois avant d'entamer le traitement des lignes de données. Par exemple, dans une destination, vous pouvez configurer la commande paramétrable que la destination utilise pour insérer chaque ligne de données dans la source de données.  
  
```vb  
    Dim sqlConn As SqlConnection  
    Dim sqlCmd As SqlCommand  
    Dim sqlParam As SqlParameter  
...  
    Public Overrides Sub PreExecute()  
  
        sqlCmd = New SqlCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
            "VALUES(@addressid, @city)", sqlConn)  
        sqlParam = New SqlParameter("@addressid", SqlDbType.Int)  
        sqlCmd.Parameters.Add(sqlParam)  
        sqlParam = New SqlParameter("@city", SqlDbType.NVarChar, 30)  
        sqlCmd.Parameters.Add(sqlParam)  
  
    End Sub  
```  
  
```csharp  
SqlConnection sqlConn;   
SqlCommand sqlCmd;   
SqlParameter sqlParam;   
  
public override void PreExecute()   
{   
  
    sqlCmd = new SqlCommand("INSERT INTO Person.Address2(AddressID, City) " + "VALUES(@addressid, @city)", sqlConn);   
    sqlParam = new SqlParameter("@addressid", SqlDbType.Int);   
    sqlCmd.Parameters.Add(sqlParam);   
    sqlParam = new SqlParameter("@city", SqlDbType.NVarChar, 30);   
    sqlCmd.Parameters.Add(sqlParam);   
  
}  
```  
  
## <a name="processing-inputs-and-outputs"></a>Traitement des entrées et des sorties  
  
### <a name="processing-inputs"></a>Traitement des entrées  
 Les composants Script configurés en tant que transformations ou destinations possèdent une entrée.  
  
#### <a name="what-the-bufferwrapper-project-item-provides"></a>Composants fournis par l'élément de projet BufferWrapper  
 Pour chaque entrée configurée, l’élément de projet **BufferWrapper** contient une classe qui dérive de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> et qui porte le même nom que l’entrée. Chaque classe de mémoire tampon d'entrée contient les propriétés, fonctions et méthodes suivantes :  
  
-   Des propriétés d'accesseur typées et nommées pour chaque colonne d'entrée sélectionnée. Ces propriétés sont en lecture seule ou en lecture/écriture selon le **Type d’utilisation** spécifié pour la colonne dans la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**.  
  
-   Une propriété **\<column>_IsNull** pour chaque colonne d’entrée sélectionnée. Cette propriété est également en lecture seule ou en lecture/écriture selon le **Type d’utilisation** spécifié pour la colonne.  
  
-   Une méthode **DirectRowTo\<outputbuffer>** pour chaque sortie configurée. Vous utilisez ces méthodes lorsque vous filtrez des lignes sur l’une des différentes sorties dans le même **ExclusionGroup**.  
  
-   Une fonction **NextRow** pour récupérer la ligne d’entrée suivante et une fonction **EndOfRowset** pour déterminer si la dernière mémoire tampon de données a été traitée. Généralement, vous n’avez pas besoin de ces fonctions lorsque vous exécutez les méthodes de traitement d’entrée implémentées dans la classe de base **UserComponent**. La section suivante fournit des informations supplémentaires sur la classe de base **UserComponent**.  
  
#### <a name="what-the-componentwrapper-project-item-provides"></a>Composants fournis par l'élément de projet ComponentWrapper  
 L’élément de projet ComponentWrapper contient une classe nommée **UserComponent** dérivée de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La classe **ScriptMain** dans laquelle vous écrivez votre code personnalisé dérive à son tour de **UserComponent**. La classe **UserComponent** contient les méthodes suivantes :  
  
-   Une implémentation substituée de la méthode **ProcessInput**. Il s’agit de la méthode que le moteur de flux de données appelle au moment de l’exécution, juste après la méthode **PreExecute**. Elle peut être appelée plusieurs fois. **ProcessInput** transfère le traitement à la méthode **\<inputbuffer> _ProcessInput**. La méthode **ProcessInput** recherche ensuite la fin de la mémoire tampon d’entrée et, si elle la trouve, appelle la méthode remplçable **FinishOutputs** et la méthode privée **MarkOutputsAsFinished**. La méthode **MarkOutputsAsFinished** appelle ensuite **SetEndOfRowset** sur la dernière mémoire tampon de sortie.  
  
-   Une implémentation substituable de la méthode **\<inputbuffer>_ProcessInput**. Cette implémentation par défaut parcourt simplement chaque ligne d’entrée et appelle **\<inputbuffer>_ProcessInputRow**.  
  
-   Une implémentation substituable de la méthode **\<inputbuffer>_ProcessInputRow**. L'implémentation par défaut est vide. Il s'agit normalement de la méthode que vous devez substituer pour écrire votre code de traitement de données personnalisé.  
  
#### <a name="what-your-custom-code-should-do"></a>Opérations à effectuer par votre code personnalisé  
 Vous pouvez utiliser les méthodes suivantes pour traiter l’entrée dans la classe **ScriptMain** :  
  
-   Substituez **\<inputbuffer>_ProcessInputRow** pour traiter les données dans chaque ligne d’entrée lors de leur transfert.  
  
-   Substituez **\<inputbuffer>_ProcessInput** uniquement si vous devez effectuer une autre opération pendant que vous parcourez les lignes d’entrée, (par exemple, lorsque vous devez vérifier si **EndOfRowset** exécute une autre action une fois que toutes les lignes ont été traitées). Appelez **\<inputbuffer>_ProcessInputRow** pour effectuer le traitement de la ligne.  
  
-   Substituez **FinishOutputs** si vous devez effectuer une opération sur les sorties avant leur fermeture.  
  
 La méthode **ProcessInput** garantit que ces méthodes sont appelées aux moments appropriés.  
  
### <a name="processing-outputs"></a>Traitement des sorties  
 Les composants Script configurés en tant que sources ou transformations possèdent une ou plusieurs sorties.  
  
#### <a name="what-the-bufferwrapper-project-item-provides"></a>Composants fournis par l'élément de projet BufferWrapper  
 Pour chaque sortie configurée, l'élément de projet BufferWrapper contient une classe qui dérive de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> et qui porte le même nom que la sortie. Chaque classe de mémoire tampon d'entrée contient les propriétés et méthodes suivantes :  
  
-   Des propriétés d'accesseur nommées, typées et en écriture seule pour chaque colonne de sortie.  
  
-   Une propriété **\<column>_IsNull** en écriture seule pour chaque colonne de sortie sélectionnée qui vous permet d’affecter la valeur **null** à la valeur de colonne.  
  
-   Une méthode **AddRow** permettant d’ajouter une ligne vide à la mémoire tampon de sortie.  
  
-   Une méthode **SetEndOfRowset** pour indiquer au moteur de flux de données qu’aucune autre mémoire tampon de données n’est attendue. Une fonction **EndOfRowset** permet également de déterminer si la mémoire tampon active est la dernière mémoire tampon de données. Généralement, vous n’avez pas besoin de ces fonctions lorsque vous exécutez les méthodes de traitement d’entrée implémentées dans la classe de base **UserComponent**.  
  
#### <a name="what-the-componentwrapper-project-item-provides"></a>Composants fournis par l'élément de projet ComponentWrapper  
 L’élément de projet ComponentWrapper contient une classe nommée **UserComponent** dérivée de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La classe **ScriptMain** dans laquelle vous écrivez votre code personnalisé dérive à son tour de **UserComponent**. La classe **UserComponent** contient les méthodes suivantes :  
  
-   Une implémentation substituée de la méthode **PrimeOutput**. Le moteur de flux de données appelle cette méthode avant **ProcessInput** au moment de l’exécution. La méthode est appelée une seule fois. **PrimeOutput** transfère le traitement à la méthode **CreateNewOutputRows**. Puis, si le composant est une source (autrement dit, s’il ne possède pas d’entrée), **PrimeOutput** appelle la méthode substituable **FinishOutputs** et la méthode privée **MarkOutputsAsFinished**. La méthode **MarkOutputsAsFinished** appelle **SetEndOfRowset** sur la dernière mémoire tampon de sortie.  
  
-   Une implémentation substituable de la méthode **CreateNewOutputRows**. L'implémentation par défaut est vide. Il s'agit normalement de la méthode que vous devez substituer pour écrire votre code de traitement de données personnalisé.  
  
#### <a name="what-your-custom-code-should-do"></a>Opérations à effectuer par votre code personnalisé  
 Vous pouvez utiliser les méthodes suivantes pour traiter les sorties dans la classe **ScriptMain** :  
  
-   Substituez **CreateNewOutputRows** uniquement lorsque vous pouvez ajouter et remplir des lignes de sortie avant de traiter des lignes d’entrée. Par exemple, vous pouvez utiliser **CreateNewOutputRows** dans une source, mais dans une transformation à sorties asynchrones, vous devez appeler **AddRow** pendant ou après le traitement des données d’entrée.  
  
-   Substituez **FinishOutputs** si vous devez effectuer une opération sur les sorties avant leur fermeture.  
  
 La méthode **PrimeOutput** garantit que ces méthodes sont appelées aux moments appropriés.  
  
## <a name="postexecute-method"></a>Méthode PostExecute  
 Substituez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PostExecute%2A> de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> lorsque vous devez effectuer un traitement une seule fois après avoir traité les lignes de données. Par exemple, dans une source, vous pouvez fermer le **System.Data.SqlClient.SqlDataReader** qui vous a permis de charger des données dans le flux de données.  
  
> [!IMPORTANT]  
>  La collection de **ReadWriteVariables** est disponible uniquement dans la méthode **PostExecute**. Par conséquent, vous ne pouvez pas incrémenter directement la valeur d'une variable de package à mesure que vous traitez chaque ligne de données. Incrémentez plutôt la valeur d’une variable locale, puis définissez la valeur de la variable de package sur la valeur de la variable locale dans la méthode **PostExecute** une fois que toutes les données ont été traitées.  
  
## <a name="releaseconnections-method"></a>Méthode ReleaseConnections  
 Généralement, les sources et les destinations doivent se connecter à une source de données externe. Substituez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ReleaseConnections%2A> de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> pour fermer et libérer la connexion que vous avez ouverte précédemment dans la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.AcquireConnections%2A>.  
  
```vb  
    Dim connMgr As IDTSConnectionManager100  
...  
    Public Overrides Sub ReleaseConnections()  
  
        connMgr.ReleaseConnection(sqlConn)  
  
    End Sub  
```  
  
```csharp  
IDTSConnectionManager100 connMgr;  
  
public override void ReleaseConnections()  
{  
  
    connMgr.ReleaseConnection(sqlConn);  
  
}  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)   
 [Codage et débogage du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)  
  
  
