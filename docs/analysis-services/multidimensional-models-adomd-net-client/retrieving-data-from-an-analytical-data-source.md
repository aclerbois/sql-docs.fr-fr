---
title: La récupération des données à partir d’une Source de données analytiques | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 50ded9ed90f9090d7a711c2d0cdb049e942896b8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-data-from-an-analytical-data-source"></a>Récupération de données à partir d'une source de données analytiques
  Dès lors que vous avez établi une connexion et que vous avez créé la requête, vous pouvez récupérer tout type de données. Dans ADOMD.NET, vous pouvez récupérer des données à l’aide de trois objets différents (<xref:Microsoft.AnalysisServices.AdomdClient.CellSet>, <xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>, et <xref:System.Xml.XmlReader>) en appelant une de le **Execute** méthodes de la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdCommand> objet.  
  
 Chacun de ces trois objets établit un équilibre entre interactivité et charge :  
  
-   *L’interactivité* fait référence à la facilité d’utilisation et la quantité d’informations disponibles via le modèle objet.  
  
-   *Surcharge* fait référence à la quantité de trafic qui génère un modèle objet via la connexion réseau au serveur, la quantité de mémoire nécessaire pour le modèle d’objet et la vitesse à laquelle le modèle objet récupère les données.  
  
 Pour vous aider à sélectionner l'objet de récupération de données qui répond le mieux aux besoins de votre application, le tableau suivant souligne les différences entre interactivité et charge pour chaque objet.  
  
|Objet|Interactivité|Charge|Conserve la dimensionnalité|Informations d'utilisation|  
|------------|-------------------|--------------|----------------------------|-----------------------|  
|<xref:Microsoft.AnalysisServices.AdomdClient.CellSet>|Maximale|Relativement élevée, ce qui se traduit par une vitesse de récupération de données des plus lentes|Oui|[Récupération de données à l’aide de la classe CellSet](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-cellset.md)|  
|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataAdapter>|Modérée|Modérée|non|[Remplissage d’un DataSet à partir d’un DataAdapter](http://go.microsoft.com/fwlink/?LinkId=70016)|  
|<xref:Microsoft.AnalysisServices.AdomdClient.AdomdDataReader>|Modérée|Modérée|non|[Récupération de données à l’aide d’AdomdDataReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-adomddatareader.md)|  
|<xref:System.Xml.XmlReader>|Minimale|Minimale, ce qui se traduit par une vitesse de récupération de données des plus rapides|Oui|[La récupération des données à l’aide de XmlReader](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-data-using-the-xmlreader.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du client ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/adomd-net-client-programming.md)  
  
  
