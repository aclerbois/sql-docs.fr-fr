---
title: La récupération des données à l’aide d’AdomdDataReader | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7f3035799df52ec4c4dbd44225247638b92048c0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-data-using-the-adomddatareader"></a>Récupération de données à l'aide d'AdomdDataReader
  Lorsqu'il s'agit de récupérer des données analytiques, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> offre un bon compromis entre charge et interactivité. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> récupère un flux de données aplati, avant uniquement et en lecture seule auprès d'une source de données analytiques. Ce flux de données, qui n'est pas mis en mémoire tampon, permet à la logique procédurale de traiter de façon séquentielle et efficace les résultats d'une source de données analytiques. Le choix de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> s'avère judicieux lorsqu'il s'agit de récupérer de grandes quantités de données en vue de les afficher, car les données ne sont pas placées en mémoire cache.  
  
 L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> peut également contribuer à améliorer les performances de l'application en récupérant les données dès qu'elles sont disponibles, au lieu d'attendre le retour des résultats complets de la requête. De plus, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> diminue la charge système dans la mesure où, par défaut, ce lecteur ne stocke qu'une ligne à la fois en mémoire.  
  
 L'optimisation des performances conférée par l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> a néanmoins un inconvénient : la quantité d'informations sur les données récupérées est moins importante qu'avec les autres méthodes de récupération de données. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> ne prend pas en charge un modèle objet volumineux pour représenter les données ou les métadonnées, pas plus qu'il ne tient compte des fonctionnalités analytiques plus complexes comme l'écriture différée de cellule. Toutefois, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> propose un ensemble de méthodes fortement typées pour la récupération de données d'ensemble de cellules, ainsi qu'une méthode permettant la récupération de métadonnées d'ensemble de cellules sous forme de tableau. En outre, <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> implémente la **IDbDataReader** pour la prise en charge la liaison de données et la récupération de données à l’aide de l’interface du **SelectCommand** (méthode), à partir de la **System.Data** espace de noms de la bibliothèque de classes Microsoft .NET Framework.  
  
## <a name="retrieving-data-from-the-adomddatareader"></a>Récupération de données à partir d'AdomdDataReader  
 Pour utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> afin de récupérer des données, procédez comme suit :  
  
1.  **Créer une nouvelle instance de l’objet.**  
  
     Pour créer une nouvelle instance de la classe <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>, vous devez appeler la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteReader%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>.  
  
2.  **Récupérer des données.**  
  
     Lorsque la commande s’exécute la requête, ADOMD.NET retourne les résultats dans le **Resultset** mettre en forme, un format tabulaire comme décrit dans la spécification XML for Analysis, pour aplatir les données pour le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> objet. Le format tabulaire est inhabituel en matière d'interrogation de données analytiques compte tenu de leur dimensionnalité variable.  
  
     ADOMD.NET stocke ces résultats tabulaires dans le tampon réseau du client jusqu'à ce que vous les demandiez en utilisant l'une des méthodes suivantes :  
  
    -   Appelez la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.Read%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>.  
  
         La méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.Read%2A> obtient une ligne à partir des résultats de la requête. Vous pouvez ensuite passer le nom ou la référence ordinale, de la colonne à la [élément](https://msdn.microsoft.com/en-us/library/ms131793(v=sql.130).aspx) propriété pour accéder à chaque colonne de la ligne retournée. Par exemple, la première colonne de la ligne actuelle est nommée ColumnName. Ensuite, `reader[0].ToString()` ou `reader["ColumnName"].ToString()` retourne le contenu de la première colonne dans la ligne actuelle.  
  
    -   Appel de l'une des méthodes d'accesseur typées.  
  
         L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> fournit une série de méthodes d'accesseur typées—méthodes qui permettent d'accéder aux valeurs de colonne dans leurs types de données natifs. Dans la mesure où vous connaissez le type de données sous-jacent d'une valeur de colonne, une méthode d'accesseur typée diminue le nombre de conversions de type nécessaires au moment de récupérer la valeur de colonne et, de ce fait, procure de meilleures performances.  
  
         <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetDateTime%2A>, <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetDouble%2A> et <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetInt32%2A> comptent parmi les méthodes d'accesseur typées disponibles. Pour obtenir la liste complète des méthodes d'accesseur typées, consultez <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>.  
  
3.  **Fermez le lecteur.**  
  
     Vous devez toujours appeler la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.Close%2A> lorsque vous avez fini d'utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>. Lorsqu'une instance d'un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> est ouverte, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> est utilisé exclusivement par cet objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>. Vous ne serez pas en mesure d’exécuter des commandes sur l’instance de la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>, y compris la création d’un autre <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> ou **System.Xml.XmlReader**, jusqu'à ce que vous fermiez la version d’origine <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>.  
  
### <a name="example-of-retrieving-data-from-the-adomddatareader"></a>Exemple de récupération de données à partir d'AdomdDataReader  
 L'exemple de code suivant parcourt un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> et retourne les deux premières valeurs de chaque ligne sous forme de chaînes.  
  
```vb  
If Reader.HasRows Then  
    Do While objReader.Read()  
        Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _  
            objReader.GetString(0), objReader.GetString(1))  
    Loop  
Else  
  Console.WriteLine("No rows returned.")  
End If  
  
objReader.Close()  
```  
  
```csharp  
if (objReader.HasRows)  
  while (objReader.Read())  
    Console.WriteLine("\t{0}\t{1}", _  
        objReader.GetString(0), objReader.GetString(1));  
else  
  Console.WriteLine("No rows returned.");  
  
objReader.Close();  
```  
  
## <a name="retrieving-metadata-from-the-adomddatareader"></a>Récupération de métadonnées à partir d'AdomdDataReader  
 Lorsqu'une instance d'un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> est ouverte, vous pouvez récupérer des informations de schéma, ou métadonnées, sur le jeu d'enregistrements actuel à l'aide de la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetSchemaTable%2A>. <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetSchemaTable%2A>Retourne un **DataTable** objet qui est rempli avec les informations de schéma pour le jeu d’enregistrements actuel. L’objet **DataTable** contiendra une ligne pour chaque colonne du jeu d’enregistrements. Chaque colonne de la ligne de table de schéma correspond à une propriété de la colonne retournée dans l’ensemble de cellules, où **ColumnName** représente le nom de la propriété et la valeur de la colonne correspond à la valeur de la propriété.  
  
### <a name="example-of-retrieving-metadata-from-the-adomddatareader"></a>Exemple de récupération de métadonnées à partir d'AdomdDataReader  
 L'exemple de code suivant écrit les informations de schéma pour un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>.  
  
```vb  
Dim schemaTable As DataTable = objReader.GetSchemaTable()  
  
Dim objRow As DataRow  
Dim objColumn As DataColumn  
  
For Each objRow In schemaTable.Rows  
  For Each objColumn In schemaTable.Columns  
    Console.WriteLine(objColumn.ColumnName & " = " & objRow(objColumn).ToString())  
  Next  
  Console.WriteLine()  
Next  
DataTable schemaTable = objReader.GetSchemaTable();  
```  
  
```csharp  
foreach (DataRow objRow in schemaTable.Rows)  
{  
  foreach (DataColumn objColumn in schemaTable.Columns)  
    Console.WriteLine(objColumn.ColumnName + " = " + objRow[objColumn]);  
  Console.WriteLine();  
}  
```  
  
## <a name="retrieving-multiple-result-sets"></a>Récupération de plusieurs ensembles de résultats  
 L'exploration de données prend en charge le concept de tables imbriquées, qu'ADOMD.NET présente sous forme d'ensembles de lignes imbriqués. Pour récupérer l'ensemble de lignes imbriqué associé à chaque ligne, vous devez appeler la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader.GetDataReader%2A>.  
  
## <a name="see-also"></a>Voir aussi  
 [La récupération des données à partir d’une Source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-from-an-analytical-data-source.md)   
 [La récupération des données à l’aide de l’ensemble de cellules](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-cellset.md)   
 [La récupération des données à l’aide de XmlReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-xmlreader.md)  
  
  
