---
title: La récupération des données à l’aide de XmlReader | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e3dceaad0eed7f251962b9b86b6979093c62fdfa
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-data-using-the-xmlreader"></a>Récupération de données à l'aide de XmlReader
  Le **XmlReader** classe, partie de la **System.Xml** espace de noms pour la bibliothèque de classes Microsoft .NET Framework, est similaire à la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> classe qui le **XmlReader** classe fournit également rapide, non mis en cache et en avant uniquement accès aux données. S’il n’est pas nécessaire pour une vue analytique en mémoire des données avec le <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> objet, le **XmlReader** objet est parfait pour récupérer des données XML, en particulier pour les grandes quantités de données. Étant donné que **XmlReader** flux de données, **XmlReader** n’a pas à récupérer et mettre en cache toutes les données avant de les exposer les données à l’appelant, comme ce serait le cas si un <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> objet ont été utilisés pour convertir la réponse XML for Analysis dans une représentation du modèle objet analytique.  
  
 Le **XmlReader** classe fournit un accès direct au XML réponse XMLA reçue par ADOMD.NET lorsque la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteXmlReader%2A> méthode de la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> objet est appelé. Les données étant récupérées dans un format XML brut, vous devez analyser les données et les métadonnées manuellement. Dès que les données ont été récupérées, le **XmlReader** objet doit être fermé.  
  
## <a name="retrieving-data-and-metadata"></a>Récupération de données et de métadonnées  
 Pour utiliser le **XmlReader** de classe pour récupérer des données, vous procédez comme suit :  
  
1.  **Créer une nouvelle instance de l’objet.**  
  
     Pour créer une nouvelle instance de la **XmlReader** (classe), vous appelez le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteXmlReader%2A> méthode de la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> objet.  
  
2.  **Récupérer des données.**  
  
     Une fois la commande exécute la requête et retourne un **XmlReader**, vous devez analyser les données et métadonnées. Les données et les métadonnées XML sont présentées dans le format natif utilisé par le fournisseur XMLA. Pour la plupart de XML pour les fournisseurs de l’analyse, le format natif est la **MDDataSet** format. Le **MDDataSet** format fournit des données et des métadonnées pour les ensembles de cellules dans un format bien structuré. Pour plus d’informations sur la **MDDataSet** format, consultez la spécification XML for Analysis.  
  
3.  **Fermez le lecteur.**  
  
     Vous devez toujours appeler la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.Close%2A> méthode lorsque vous avez fini d’utiliser le **XmlReader** objet. Pendant une **XmlReader** est ouvert, celui-ci **XmlReader** a l’usage exclusif de le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> objet qui a été utilisé pour exécuter la commande. Vous ne serez pas en mesure d’exécuter des commandes à l’aide de qui <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>, y compris la création d’un autre **XmlReader** ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>, jusqu'à ce que vous fermiez la version d’origine **XmlReader**.  
  
### <a name="example-of-retrieving-data-from-the-xmlreader"></a>Exemple de récupération de données à partir de XmlReader  
 L’exemple suivant exécute une commande et récupère les données comme un **XmlReader**, affichant le contenu du fichier à la console.  
  
 [!code-cs[Adomd.NetClient#OutputDataWithXML](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/retrieving-data-using-th_1_1.cs)]  
  
## <a name="see-also"></a>Voir aussi  
 [La récupération des données à partir d’une Source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-from-an-analytical-data-source.md)   
 [La récupération des données à l’aide de l’ensemble de cellules](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-cellset.md)   
 [Récupération de données à l’aide d’AdomdDataReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-adomddatareader.md)  
  
  
