---
title: Assistant génération de schéma (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 41e1c17aac763657a16327bb038c180f614fcf67
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="schema-generation-wizard-analysis-services"></a>Assistant Génération de schéma (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] prend en charge deux méthodes d'utilisation des schémas relationnels lors de la définition d'objets OLAP dans un projet ou une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Généralement, vous définissez des objets OLAP en fonction d'un modèle d'objet logique construit dans une vue de source de données d'un projet ou d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Cette vue de source de données est définie en fonction d'éléments de schéma provenant d'une ou plusieurs sources de données relationnelles, lorsqu'ils sont personnalisés dans la vue de source de données.  
  
 Vous pouvez également commencer par définir des objets OLAP, puis générer une vue de source de données, une source de données et le schéma de base de données relationnelle sous-jacent qui prend en charge ces objets OLAP. Cette base de données relationnelle est appelée « base de données de la zone de sujet ».  
  
 Cette approche, parfois appelée construction verticale, sert fréquemment à créer des prototypes et des modèles d'analyse. Lorsque vous utilisez cette approche, vous faites appel à l'Assistant Génération de schéma pour créer la vue de source de données et les objets de source de données sous-jacents selon les objets OLAP définis dans un projet ou une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Il s'agit d'une approche itérative. Vous exécuterez très probablement l'Assistant à plusieurs reprises à mesure que vous modifierez la conception des dimensions et des cubes. Chaque fois que vous exécutez l'Assistant, il intègre les modifications dans les objets sous-jacents et, dans la mesure du possible, préserve les données contenues dans les bases de données sous-jacentes.  
  
 Le schéma généré est un schéma du moteur de base de données relationnelle SQL Server. L'Assistant ne génère pas de schémas pour d'autres produits de base de données relationnelle.  
  
 Les données qui remplissent la base de données de zone de sujet sont ajoutées séparément, à l'aide des outils et techniques que vous utilisez pour remplir toute base de données relationnelle SQL Server. Dans la plupart des cas, les données sont conservées lorsque vous exécutez à nouveau l'Assistant, mais il existe des exceptions. Par exemple, certaines données doivent être supprimées si elles sont contenues dans une dimension ou un attribut que vous supprimez. Si l'Assistant Génération de schéma doit supprimer des données en raison d'une modification de schéma, vous recevez un avertissement avant que les données ne soient supprimées et vous pouvez annuler la régénération.  
  
 En règle générale, toute modification apportée à un objet initialement généré par l'Assistant Génération de schéma est écrasée lorsque l'Assistant Génération de schéma régénère ultérieurement cet objet. La principale exception à la règle s'applique lorsque vous ajoutez des colonnes à une table générée par l'Assistant Génération de schéma. Dans ce cas, l'Assistant Génération de schéma préserve les colonnes que vous avez ajoutées à la table, ainsi que les données de ces colonnes.  
  
## <a name="in-this-section"></a>Contenu de cette section  
 Le tableau suivant répertorie des rubriques supplémentaires qui expliquent comment utiliser l'Assistant Génération de schéma.  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Utiliser l’Assistant Génération de schéma &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/use-the-schema-generation-wizard-analysis-services.md)|Explique comment générer le schéma pour les bases de données de la zone de sujet et de la zone de transit.|  
|[Présentation des schémas de base de données](../../analysis-services/multidimensional-models/understanding-the-database-schemas.md)|Décrit le schéma généré pour les bases de données de la zone de sujet et de la zone de transit.|  
|[Présentation de la génération incrémentielle](../../analysis-services/multidimensional-models/understanding-incremental-generation.md)|Décrit les fonctionnalités de génération incrémentielle de l'Assistant Génération de schéma.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)   
 [Sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-sources-in-multidimensional-models.md)   
 [Sources de données prises en charge &#40;SSAS – Multidimensionnel&#41;](../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md)  
  
  
