---
title: Programmation du Client ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fbef1da93d4edd44cedbf89ef52133a22b5bb6d9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="adomdnet-client-programming"></a>Programmation du client ADOMD.NET
  Les composants clients ADOMD.NET résident dans le **Microsoft.AnalysisServices.AdomdClient** espace de noms (dans microsoft.analysisservices.adomdclient.dll). Ces composants clients offrent les fonctionnalités de client et les applications de couche intermédiaire pour interroger facilement les données et les métadonnées à partir d’une banque de données analytiques, telles que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="using-the-adomdnet-client-objects"></a>Utilisation des objets clients ADOMD.NET  
 Lorsqu'il s'agit d'interroger une source de données analytiques, il convient d'effectuer un ensemble de tâches courantes. Le tableau suivant présente les tâches courantes dans lesquelles les objets clients ADOMD.NET sont utilisés dans le cadre d'une requête de ce type.  
  
|Tâche| Description|  
|----------|-----------------|  
|[Établissement de connexions dans ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/connections-in-adomd-net.md)|Dans ADOMD.NET, il convient d'utiliser un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> pour établir des connexions avec les sources de données analytiques, telles que les bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Vous pouvez utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> pour exécuter des commandes ou récupérer des données et des métadonnées à partir de la source de données analytiques.|  
|[Récupération de métadonnées à partir d’une source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-from-an-analytical-data-source.md)|Après avoir établi une connexion, vous pouvez utiliser une large palette d'objets pour récupérer des informations sur la source de données sous-jacente. Ces fonctionnalités permettent aux applications de s'adapter à la source de données à laquelle elles se sont connectées.|  
|[Exécution de commandes sur une source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/executing-commands-against-an-analytical-data-source.md)|L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> fournit les interfaces nécessaires à l'exécution de commandes sur la source de données analytiques sous-jacente.|  
|[Récupération de données à partir d’une source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-from-an-analytical-data-source.md)|Après l’exécution d’une commande, les données peuvent être récupérées et analysée à l’aide du <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>, ou **System.XmlReader** objets.|  
|[Exécution de Transactions dans ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/connections-in-adomd-net-performing-transactions.md)|Toutes les actions énumérées dans les lignes précédentes de ce tableau peuvent se produire dans une transaction validée en lecture, où les verrous partagés sont maintenus pendant la lecture des données afin d'éviter des lectures erronées. Les données peuvent toujours être modifiées avant la fin de la transaction, ce qui provoque des lectures non renouvelables ou des données fantômes. L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> fournit les fonctionnalités de transaction dans ADOMD.NET.|  
  
 L'interaction avec la hiérarchie d'objets ADOMD.NET débute généralement avec un ou plusieurs objets de la couche de niveau supérieur, comme indiqué dans le tableau suivant.  
  
|Pour|Utiliser cet objet|  
|--------|---------------------|  
|Se connecter à une source de données analytiques|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> représente à la fois une connexion à une source de données et les métadonnées de la source de données. Par exemple, vous pouvez vous connecter à un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] cube local (.cub) du fichier et examinez le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.Cubes%2A> propriété pour obtenir les métadonnées relatives aux cubes présents sur la source de données analytiques. Cet objet représente également l’implémentation de la **IDbConnection** interface, une interface qui est requise par tous les fournisseurs de données .NET Framework.|  
|Découvrir les possibilités d'exploration de données propres à la source de données|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> expose plusieurs collections d'exploration de données :<br /><br /><br /><br /> <xref:Microsoft.AnalysisServices.AdomdClient.MiningModelCollection> contient une liste de tous les modèles d'exploration de données de la source de données.<br /><br /><br /><br /> <xref:Microsoft.AnalysisServices.AdomdClient.MiningServiceCollection> fournit des informations sur les algorithmes d'exploration de données disponibles.<br /><br /><br /><br /> <xref:Microsoft.AnalysisServices.AdomdClient.MiningStructureCollection> expose des informations sur les structures d'exploration de données du serveur.|  
|Interroger la source de données|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> représente l'instruction ou la requête qui sera envoyée au serveur. Après avoir établi une connexion à une source de données, vous devez utiliser un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> pour exécuter les instructions dans le langage pris en charge, tel que MDX (Multidimensional Expressions) ou DMX (Data Mining Extensions). Vous pouvez également utiliser un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> pour retourner les résultats sous forme d'objets <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>.|  
|Récupérer les données avec rapidité et efficacité|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader> peut être créé en faisant appel à la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteReader%2A> d'un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>. Cet objet implémente le **IDbDataReader** de l’interface à partir de la **System.Data** espace de noms de la bibliothèque de classes .NET Framework.|  
|Récupérer des données analytiques avec une quantité maximale de métadonnées|<xref:Microsoft.AnalysisServices.AdomdClient.CellSet><br /> <xref:Microsoft.AnalysisServices.AdomdClient.CellSet> peut être créé en faisant appel à la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> ou <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteCellSet%2A> de <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>. Dès lors que <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> a retourné <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, vous pouvez examiner les données analytiques contenues dans <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>.|  
|Récupérer les métadonnées relatives aux cubes, notamment les dimensions disponibles, les mesures, les jeux nommés, etc.|<xref:Microsoft.AnalysisServices.AdomdClient.CubeDef><br /> <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> représente les métadonnées relatives à un cube. Vous faites référence à <xref:Microsoft.AnalysisServices.AdomdClient.CubeDef> à partir de <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.|  
|Récupérer des données à l’aide du **System.Data.IDbDataAdapter** interface|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataAdapter><br /> <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataAdapter> fournit une prise en charge en lecture seule pour les applications clientes .NET Framework existantes.|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du serveur ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)   
 [Développement avec ADOMD.NET](../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md)  
  
  
