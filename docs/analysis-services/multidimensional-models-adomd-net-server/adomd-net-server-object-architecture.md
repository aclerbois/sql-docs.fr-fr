---
title: Architecture d’objets serveur ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f3806faf430f5909397a3b805433ddaca402b78f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="adomdnet-server-object-architecture"></a>Architecture des objets serveur ADOMD.NET
  Les objets serveur ADOMD.NET sont des objets d’assistance qui peuvent être utilisés pour créer les fonctions définies par l’utilisateur (UDF) ou des procédures stockées de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Pour utiliser le **Microsoft.AnalysisServices.AdomdServer** espace de noms (et ces objets), une référence à msmgdsrv.dll doit être ajoutée au projet UDF ou procédure stockée.  
  
 ![Affiche les relations d’objet dans le serveur ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-server/media/adomdnetserverobjectmodel.gif "montre les relations d’objet dans le serveur ADOMD.NET")  
Modèle objet ADOMD.NET  
  
 L'interaction avec la hiérarchie d'objets ADOMD.NET débute généralement avec un ou plusieurs objets de la couche de niveau supérieur, comme indiqué dans le tableau suivant.  
  
|Pour|Utiliser cet objet|  
|--------|---------------------|  
|Évaluer des instructions MDX (Multidimensional Expressions)|<xref:Microsoft.AnalysisServices.AdomdServer.Expression><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdServer.Expression> permet d'exécuter une expression MDX et d'évaluer cette expression sous un tuple spécifié.|  
|Offrir la possibilité d'exécuter des fonctions MDX sans construire l'instruction MDX entière|<xref:Microsoft.AnalysisServices.AdomdServer.MDX><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdServer.MDX> s'avère très pratique pour appeler des fonctions MDX prédéfinies sans utiliser l'objet <xref:Microsoft.AnalysisServices.AdomdServer.Expression>. Des fonctions supplémentaires pour l'objet <xref:Microsoft.AnalysisServices.AdomdServer.MDX> devraient être disponibles dans les futures versions.|  
|Représenter le contexte d'exécution actuel pour la fonction définie par l'utilisateur|<xref:Microsoft.AnalysisServices.AdomdServer.Context><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdServer.Context> affiche des informations telles que le cube ou le modèle d'exploration de données actuel, ainsi que diverses collections de métadonnées. L'une des principales utilisations de l'objet <xref:Microsoft.AnalysisServices.AdomdServer.Context> est la propriété <xref:Microsoft.AnalysisServices.AdomdServer.Hierarchy.CurrentMember%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdServer.Hierarchy>. Cette utilisation clé permet à l'auteur de la fonction définie par l'utilisateur ou de la procédure stockée de prendre des décisions en fonction du membre d'une certaine dimension sur lequel la requête porte.|  
|Créer des ensembles et des tuples|<xref:Microsoft.AnalysisServices.AdomdServer.SetBuilder>, <xref:Microsoft.AnalysisServices.AdomdServer.TupleBuilder><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdServer.SetBuilder> permet de créer des ensembles immuables, tandis que l'objet <xref:Microsoft.AnalysisServices.AdomdServer.TupleBuilder> permet de créer des tuples immuables.|  
|Prendre en charge une conversion implicite entre les six types de base du langage MDX|<xref:Microsoft.AnalysisServices.AdomdServer.MDXValue><br /> L'objet <xref:Microsoft.AnalysisServices.AdomdServer.MDXValue> assure une conversion implicite entre les types suivants :<br /><br /> <xref:Microsoft.AnalysisServices.AdomdServer.Hierarchy><br /><br /> <xref:Microsoft.AnalysisServices.AdomdServer.Level><br /><br /> <xref:Microsoft.AnalysisServices.AdomdServer.Member><br /><br /> <xref:Microsoft.AnalysisServices.AdomdServer.Tuple><br /><br /> <xref:Microsoft.AnalysisServices.AdomdServer.Set><br /><br /> Scalar, ou types de valeur|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du serveur ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)  
  
  
