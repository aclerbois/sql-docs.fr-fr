---
title: Programmation de modèle tabulaire pour la compatibilité des niveaux de 1050 via 1103 | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a1e14033255d45eaacda1d553c71224e11dfe964
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tabular-model-programming-for-compatibility-levels-1050-through-1103"></a>Programmation de modèle tabulaire pour la compatibilité 1050 1103 via des niveaux
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les modèles tabulaires utilisent des constructions relationnelles pour modéliser les données Analysis Services utilisées par les applications analytiques et de rapports. Ces modèles s'exécutent sur une instance Analysis Service qui est configurée pour le mode tabulaire, en utilisant un moteur d'analyse en mémoire pour le stockage et les analyses de table rapides qui agrègent et calculent des données à mesure qu'elles sont demandées.  
  
 Si les besoins de votre solution BI personnalisée sont mieux remplis par une base de données de modèles tabulaires, vous pouvez utiliser l'une des bibliothèques Analysis Services et interfaces de programmation clientes pour intégrer votre application à des modèles tabulaires sur une instance Analysis Services. Pour interroger et calculer des données de modèle tabulaire, vous pouvez utiliser MDX ou DAX incorporé dans votre code.  
  
 Pour les modèles tabulaires créés dans les versions antérieures d’Analysis Services, notamment les modèles à des niveaux de compatibilité 1050 via 1103, les objets que vous utilisez par programme dans AMO, ADOMD.NET, XMLA ou OLE DB sont fondamentalement les mêmes pour les solutions tabulaires et multidimensionnelles. Plus précisément, les métadonnées d’objets définies pour les modèles multidimensionnels sont également utilisée pour les niveaux de compatibilité de modèle tabulaire 1050-1103.  
  
 À partir de SQL Server 2016, les modèles tabulaires peuvent créés ou mis à niveau vers le niveau de compatibilité 1200 ou supérieur, qui utilise des métadonnées tabulaires pour définir le modèle. Métadonnées et programmabilité sont fondamentalement différents à ce niveau. Consultez [tabulaire modèle de programmation pour 1200 de niveau de compatibilité et versions ultérieures](../../analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200.md) et [mettre à niveau Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md) pour plus d’informations.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Annotations CSDL pour Business Intelligence & #40 ; CSDLBI & #41 ;](../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/csdl-annotations-for-business-intelligence-csdlbi.md)  
  
 [Présentation du modèle d’objet tabulaire au niveau de compatibilité 1050 1103 via des niveaux](../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/understanding-tabular-object-model-at-levels-1050-through-1103.md)  
  
 [Informations techniques de référence pour les Annotations BI au langage CSDL](../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/technical-reference-for-bi-annotations-to-csdl.md)  
  

[IMDEmbeddedData, interface](../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/imdembeddeddata-interface.md)


  
  
