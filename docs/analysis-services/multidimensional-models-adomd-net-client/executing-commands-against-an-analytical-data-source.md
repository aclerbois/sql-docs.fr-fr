---
title: L’exécution de commandes sur une Source de données analytiques | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 01f7c32643c4e0a48f2451de37729f919b057208
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="executing-commands-against-an-analytical-data-source"></a>Exécution de commandes sur une source de données analytiques
  Après avoir établi une connexion à une source de données analytiques, vous pouvez utiliser un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> pour exécuter des commandes sur cette source de données et en obtenir les résultats. Ces commandes permettent de récupérer des données via MDX (Multidimensional Expressions), DMX (Data Mining Extensions), voire une syntaxe limitée de SQL. En outre, vous pouvez utiliser des commandes ASSL (Analysis Services Scripting Language) pour modifier la base de données sous-jacente.  
  
## <a name="creating-a-command"></a>Création d'une commande  
 Avant d'exécuter une commande, vous devez la créer. Pour ce faire, vous pouvez employer deux méthodes différentes :  
  
-   La première méthode consiste à utiliser le constructeur <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>, qui peut prendre une commande à exécuter au niveau de la source de données, et un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> sur lequel la commande doit être exécutée.  
  
-   La deuxième méthode fait appel à la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.CreateCommand%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.  
  
 Le texte de la commande à exécuter peut être interrogé et modifié à l'aide de la propriété <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.CommandText%2A>. Les commandes que vous créez ne retournent pas nécessairement de données après avoir été exécutées.  
  
## <a name="running-a-command"></a>Exécution d’une commande  
 Une fois que vous avez créé un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand>, votre commande peut employer plusieurs méthodes <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.Execute%2A> pour effectuer diverses actions. Le tableau ci-dessous répertorie certaines de ces actions.  
  
|Pour|Utiliser la méthode|  
|--------|---------------------|  
|Retourner les résultats sous forme de flux de données|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteReader%2A> pour retourner un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>|  
|Retourner un objet <xref:Microsoft.AnalysisServices.AdomdClient.CellSet>|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteCellSet%2A>|  
|Exécuter des commandes qui ne retournent pas de lignes|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteNonQuery%2A>|  
|Retourner un **XMLReader** objet qui contient les données à un document XML pour le format compatible Analysis (XMLA)|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand.ExecuteXmlReader%2A>|  
  
### <a name="example-of-running-a-command"></a>Exemple d'exécution d'une commande  
 Cet exemple utilise le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> pour exécuter une commande XMLA qui traitera le **Adventure Works DW** cube sur le serveur local, sans retourner de données.  
  
 [!code-cs[Adomd.NetClient#ExecuteXMLAProcessCommand](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/executing-commands-again_1.cs)]  
  
  
