---
title: Procédures stockées d’exploration de données (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c8db1cbcd5cd0ebfd1f629a7ecd7ac14ce67806f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-stored-procedures-analysis-services---data-mining"></a>Procédures stockées d'exploration de données (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Depuis [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge des procédures stockées qui peuvent être écrites dans tout langage managé. Les langages managés pris en charge incluent .NET [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] , C# et C++ managé. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez appeler directement les procédures stockées à l’aide de l’instruction **CALL** ou dans le cadre d’une requête DMX (Data Mining Extensions).  
  
 Pour plus d’informations sur l’appel de procédures stockées [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , consultez [Appel des procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/calling-stored-procedures.md).  
  
 Pour obtenir des informations générales sur les fonctionnalités de programmation, consultez [programmation d’exploration de données](../../analysis-services/data-mining-programming.md).  
  
 Pour plus d’informations sur la programmation d’objets d’exploration de données, consultez l’article «[SQL Server Data Mining Programmability](http://go.microsoft.com/fwlink/?LinkId=93735)» (Programmation de l’exploration de données SQL Server) dans la bibliothèque MSDN.  
  
> [!NOTE]  
>  Lorsque vous interrogez des modèles d'exploration de données, surtout lorsque vous testez de nouvelles solutions d'exploration de données, vous voudrez peut-être appeler les procédures stockées système qui sont utilisées en interne par le moteur d'exploration de données. Vous pouvez afficher le nom de ces procédures stockées système en utilisant [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour créer une trace sur le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis en créant, parcourant et interrogeant les modèles d’exploration de données. Toutefois, [!INCLUDE[msCoName](../../includes/msconame-md.md)] ne garantit pas la compatibilité des procédures stockées système entre les versions, et vous ne devez jamais utiliser des appels de procédures stockées système dans un système de production. Pour des raisons de compatibilité, vous devez à la place créer vos propres requêtes à l'aide des langages DMX ou XML/A.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [SystemGetCrossValidationResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetcrossvalidationresults-analysis-services-data-mining.md)  
  
-   [SystemGetClusterCrossValidationResults & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/systemgetclustercrossvalidationresults-analysis-services-data-mining.md)  
  
-   [SystemGetAccuracyResults &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/systemgetaccuracyresults-analysis-services-data-mining.md)  
  
-   [SystemGetClusterAccuracyResults & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/systemgetclusteraccuracyresults-analysis-services-data-mining.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)   
 [Onglet Validation croisée &#40;vue graphique d’analyse de précision d’exploration de données&#41;](http://msdn.microsoft.com/library/bd215a68-1ad7-4046-9c44-ec8e2be13a64)   
 [Appel d’une procédure stockée](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)  
  
  
