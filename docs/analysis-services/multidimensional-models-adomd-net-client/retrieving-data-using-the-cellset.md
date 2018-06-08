---
title: La récupération des données à l’aide de l’ensemble de cellules | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 744cbd216e28d30bf8e80a705d85e6d64955db9a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-data-using-the-cellset"></a>Récupération de données à l'aide d'un ensemble de cellules
  Lors de la récupération de données analytiques, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> procure une interactivité et une souplesse optimales. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> est un cache de données et métadonnées hiérarchiques en mémoire qui préserve la dimensionnalité des données. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> peut également être parcouru en état connecté ou déconnecté. Du fait de cette aptitude en état déconnecté, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> peut être utilisé pour consulter les données et les métadonnées dans n'importe quel ordre et constitue le modèle objet le plus complet pour la récupération de données. Autre conséquence de cette aptitude en état déconnecté : l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> subit une charge maximale, et il s'agit du modèle objet de récupération de données ADOMD.NET le plus long à remplir.  
  
## <a name="retrieving-data-in-a-connected-state"></a>Récupération de données en état connecté  
 Pour utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> afin de récupérer des données, procédez comme suit :  
  
1.  **Créer une nouvelle instance de l’objet.**  
  
     Pour créer une nouvelle instance de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, vous devez appeler la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteCellSet%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>.  
  
2.  **Identifiez les métadonnées.**  
  
     En plus de récupérer les données, ADOMD.NET récupère également les métadonnées pour l'ensemble de cellules. Dès que la commande a exécuté la requête et a retourné un objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, vous pouvez récupérer les métadonnées par le biais de différents objets. Ces métadonnées sont nécessaires aux applications clientes en vue d'afficher et d'interagir avec les données d'ensemble de cellules. Par exemple, de nombreuses applications clientes proposent des fonctionnalités qui permettent d'explorer vers le bas une position spécifiée dans un ensemble de cellules, c'est-à-dire, d'en afficher hiérarchiquement les positions enfants.  
  
     Dans ADOMD.NET, les propriétés <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.Axes%2A> et <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.FilterAxis%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> représentent les métadonnées des axes de requête et de secteur, respectivement, dans l'ensemble de cellules retourné. Ces deux propriétés retournent des références à des objets <xref:Microsoft.AnalysisServices.AdomdClient.Axis>, qui de leur côté contiennent les positions représentées sur chaque axe.  
  
     Chaque objet <xref:Microsoft.AnalysisServices.AdomdClient.Axis> contient une collection d'objets <xref:Microsoft.AnalysisServices.AdomdClient.Position> qui représentent l'ensemble de tuples disponibles pour l'axe en question. Chaque objet <xref:Microsoft.AnalysisServices.AdomdClient.Position> représente un tuple unique qui contient un ou plusieurs membres, représenté(s) par une collection d'objets <xref:Microsoft.AnalysisServices.AdomdClient.Member>.  
  
3.  **Récupérer des données à partir de la collection de l’ensemble de cellules.**  
  
     En plus de récupérer les métadonnées, ADOMD.NET récupère également les données pour l'ensemble de cellules. Dès que la commande a exécuté la requête et a retourné un <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, vous pouvez récupérer les données en utilisant la collection <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.Cells%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>. Cette collection contient les valeurs calculées pour l'intersection de tous les axes de la requête. Par conséquent, il existe plusieurs indexeurs pour accéder à chaque intersection (ou cellule). Pour obtenir une liste d'indexeurs, consultez <xref:Microsoft.AnalysisServices.AdomdClient.CellCollection.Item%2A>.  
  
### <a name="example-of-retrieving-data-in-a-connected-state"></a>Exemple de récupération de données en état connecté  
 L'exemple suivant établit une connexion au serveur local, puis exécute une commande sur la connexion. L’exemple analyse les résultats à l’aide de la **ensemble de cellules** modèle d’objet : les légendes (métadonnées) pour les colonnes sont extraites du premier axe et les légendes (métadonnées) de chaque ligne sont récupérées à partir de l’axe secondaire, et les données d’intersection sont récupérées à l’aide de la <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.Cells%2A> collection.  
  
 [!code-cs[Adomd.NetClient#ReturnCommandUsingCellSet](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/retrieving-data-using-th_0_1.cs)]  
  
## <a name="retrieving-data-in-a-disconnected-state"></a>Récupération de données en état déconnecté  
 En chargeant les données XML retournées par une requête précédente, vous pouvez utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> pour fournir une méthode complète d'exploration des données analytiques sans qu'il soit nécessaire de disposer d'une connexion active.  
  
> [!NOTE]  
>  En état déconnecté, certaines propriétés des objets disponibles à partir de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> ne sont pas disponibles. Pour plus d’informations, consultez <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.LoadXml%2A>.  
  
### <a name="example-of-retrieving-data-in-a-disconnected-state"></a>Exemple de récupération de données en état déconnecté  
 L'exemple suivant est similaire à l'exemple de métadonnées et de données présenté précédemment dans cette rubrique. Toutefois, la commande dans l’exemple suivant s’exécute avec un appel à <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteXmlReader%2A>, et le résultat est retourné comme un **System.Xml.XmlReader**. L’exemple remplit ensuite le <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> objet en utilisant ce **System.Xml.XmlReader** avec la <xref:Microsoft.AnalysisServices.AdomdClient.CellSet.LoadXml%2A> (méthode). Bien que cet exemple charge le **System.Xml.XmlReader** immédiatement, vous pouvez mettre en cache le code XML qui est contenu par le lecteur de disque dur ou transporter vers une autre application par quelque moyen qu’avant de charger les données dans un ensemble de cellules.  
  
 [!code-cs[Adomd.NetClient#DemonstrateDisconnectedCellset](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/retrieving-data-using-th_0_2.cs)]  
  
## <a name="see-also"></a>Voir aussi  
 [La récupération des données à partir d’une Source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-from-an-analytical-data-source.md)   
 [La récupération des données à l’aide d’AdomdDataReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-adomddatareader.md)   
 [La récupération des données à l’aide de XmlReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-xmlreader.md)  
  
  
