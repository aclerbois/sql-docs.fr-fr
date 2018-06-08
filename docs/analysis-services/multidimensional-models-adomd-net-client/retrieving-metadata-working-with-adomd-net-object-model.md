---
title: Utilisation du modèle objet ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 29e59b5811f6c13c7aa222c7d6eba31f9bcd706e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-metadata---working-with-adomdnet-object-model"></a>La récupération des métadonnées - fonctionne avec le modèle objet ADOMD.NET
  ADOMD.NET fournit un modèle objet qui permet d'afficher les cubes et les objets subordonnés contenus dans une source de données analytiques. Toutefois, toutes les métadonnées d'une source de données analytiques spécifique ne sont pas accessibles via le modèle objet. Le modèle objet donne uniquement accès aux informations qui permettent à une application cliente de s'afficher de telle sorte que l'utilisateur puisse construire des commandes de manière interactive. Dans la mesure où la complexité des métadonnées à présenter est réduite, le modèle objet ADOMD.NET n'en est que plus facile à utiliser.  
  
 Dans le modèle objet ADOMD.NET, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> donne accès aux informations sur les cubes OLAP (Online Analytical Processing) et les modèles d'exploration de données définis dans une source de données analytiques, ainsi qu'aux objets connexes tels que les dimensions, les jeux nommés et les algorithmes d'exploration.  
  
## <a name="retrieving-olap-metadata"></a>Récupération de métadonnées OLAP  
 Chaque objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> renferme une collection d'objets <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> qui représentent les cubes accessibles à l'utilisateur ou à l'application. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> expose les informations sur le cube, ainsi que les divers objets qui lui sont associés comme les dimensions, les indicateurs de performance clés, les mesures, les jeux nommés, etc.  
  
 Dans la mesure du possible, vous devez utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> pour représenter les métadonnées dans les applications clientes conçues pour prendre en charge plusieurs serveurs OLAP ou à des fins plus générales d'affichage et d'accès aux métadonnées.  
  
> [!NOTE]  
>  Pour des métadonnées spécifiques au fournisseur ou pour un affichage et un accès aux métadonnées détaillés, utilisez des ensembles de lignes de schéma pour récupérer les métadonnées. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Dans l'exemple suivant, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> est utilisé pour récupérer les cubes visibles et leurs dimensions à partir du serveur local :  
  
 [!code-cs[Adomd.NetClient#RetrieveCubesAndDimensions](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/retrieving-metadata-work_1_1.cs)]  
  
## <a name="retrieving-data-mining-metadata"></a>Récupération de métadonnées d'exploration de données  
 Chaque objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> intègre plusieurs collections qui fournissent des informations sur les possibilités de la source de données en matière d'exploration de données :  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningModelCollection> contient une liste de tous les modèles d'exploration de données de la source de données.  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningServiceCollection> fournit des informations sur les algorithmes d'exploration de données disponibles.  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.MiningStructureCollection> expose des informations sur les structures d'exploration de données du serveur.  
  
 Pour déterminer comment exécuter une requête sur un modèle d'exploration de données du serveur, parcourez de manière itérative la collection <xref:Microsoft.AnalysisServices.AdomdServer.MiningModel.Columns%2A>. Chaque objet <xref:Microsoft.AnalysisServices.AdomdClient.MiningModelColumn> présente les caractéristiques suivantes :  
  
-   si l'objet est une colonne d'entrée (<xref:Microsoft.AnalysisServices.AdomdClient.MiningModelColumn.IsInput%2A>) ;  
  
-   si l'objet est une colonne de prédiction (<xref:Microsoft.AnalysisServices.AdomdClient.MiningModelColumn.IsPredictable%2A>) ;  
  
-   les valeurs associées à une colonne discrète (<xref:Microsoft.AnalysisServices.AdomdClient.MiningModelColumn.Values%2A>) ;  
  
-   le type de données de la colonne (<xref:Microsoft.AnalysisServices.AdomdClient.MiningModelColumn.Type%2A>).  
  
## <a name="see-also"></a>Voir aussi  
 [Récupération de métadonnées à partir d’une source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-from-an-analytical-data-source.md)  
  
  
