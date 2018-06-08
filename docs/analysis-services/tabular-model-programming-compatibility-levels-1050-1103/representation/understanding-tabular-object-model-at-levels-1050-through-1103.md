---
title: Présentation du modèle d’objet tabulaire au niveau 1050 via 1103 | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5ff9348f28d8791d7a9cc6c571a3a4f05bbab58f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="understanding-tabular-object-model-at-levels-1050-through-1103"></a>Présentation du modèle d’objet tabulaire au niveau 1050 via 1103
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]

  Un modèle tabulaire est une représentation logique de tables, de relations, de hiérarchies, de perspectives, de mesures, et de performance clés. Cette section présente l'implémentation interne à l'aide d'objets AMO. Consultez [développement avec Analysis Management Objects &#40;AMO&#41; ](../../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md) si vous n’avez pas encore utilisé AMO.  
  
 L'approche ici est hiérarchisée, tous les objets appropriés dans le modèle tabulaire sont logiquement mappés aux objets AMO, et les interactions ou le flux de travail requis sont expliqués. Un exemple de code source pour créer un modèle tabulaire à l’aide d’AMO, AMO pour exemple tabulaire est disponible à partir de Codeplex. Remarque importante à propos du code dans l'exemple : le code est fourni uniquement comme un support aux concepts logiques expliqués ici et ne doit pas être utilisé dans un environnement de production, ni à des fins autres que pédagogiques. L'exemple est fourni sans prise en charge ni garantie.  
  
## <a name="database-representation"></a>Représentation de la base de données  
 Une base de données fournit un objet conteneur pour le modèle tabulaire. Tous les objets dans un modèle tabulaire sont contenus dans la base de données. En termes d'objets AMO, une représentation de base de données a une relation de mappage un-à-un avec `xref:Microsoft.AnalysisServices.Database` et aucun autre objet AMO principal n'est requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans l'objet de base de données AMO peuvent être utilisés lors de la modélisation.  
  
 Consultez [la représentation sous forme de la base de données&#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/database-representation-tabular.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de base de données.  
  
## <a name="connection-representation"></a>Représentation de la connexion  
 Une connexion établit la relation entre les données à inclure dans une solution de modèle tabulaire et le modèle lui-même. En termes d'objets AMO, une connexion a une relation de mappage un-à-un avec `xref:Microsoft.AnalysisServices.DataSource` et aucun autre objet AMO principal n'est requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans l'objet datasource AMO peuvent être utilisés lors de la modélisation.  
  
 Consultez [représentation de la connexion &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/connection-representation-tabular.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de source de données.  
  
## <a name="table-representation"></a>Représentation d'une table  
 Les tables sont des objets de base de données qui contiennent toutes les données dans la base de données. En termes d'objets AMO, une table a une relation de mappage un-à-plusieurs. Une table est représentée par l’utilisation des objets AMO suivants : `xref:Microsoft.AnalysisServices.DataSourceView`, `xref:Microsoft.AnalysisServices.Dimension`, `xref:Microsoft.AnalysisServices.Cube`, `xref:Microsoft.AnalysisServices.CubeDimension`, `xref:Microsoft.AnalysisServices.MeasureGroup` et `xref:Microsoft.AnalysisServices.Partition` sont les principaux objets requis. Toutefois, il est important de noter que cela ne signifie pas que tous les objets contenus dans les objets AMO précédemment mentionnés peuvent être utilisés lors de la modélisation.  
  
 Consultez [la représentation sous forme de Tables &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-representation-tabular.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de table.  
  
### <a name="calculated-column-representation"></a>Représentation d'un colonne calculée  
 Les colonnes calculées sont des expressions évaluées qui génèrent une colonne dans une table, où une nouvelle valeur est calculée et stockée pour chaque ligne de la table. En termes d’objets AMO, une colonne calculée a une relation de mappage un-à-plusieurs. Une colonne calculée est représentée par l'utilisation des objets AMO suivants : `xref:Microsoft.AnalysisServices.Dimension` et `xref:Microsoft.AnalysisServices.MeasureGroup` sont les principaux objets requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans les objets AMO mentionnés précédemment peuvent être utilisés lors de la modélisation.  
  
 Consultez [représentation d’une colonne calculée &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-calculated-column-representation.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de colonne calculée.  
  
### <a name="calculated-measure-representation"></a>Représentation de la mesure calculée  
 Les mesures calculées sont des expressions stockées qui sont évaluées à la demande une fois le modèle déployé. En termes d'objets AMO, une mesure calculée a une relation de mappage un-à-plusieurs. Une colonne calculée est représentée par l'utilisation des objets AMO suivants : `xref:Microsoft.AnalysisServices.MdxScript.Commands%2A` et `xref:Microsoft.AnalysisServices.MdxScript.CalculationProperties%2A` sont les principaux objets requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans les objets AMO mentionnés précédemment peuvent être utilisés lors de la modélisation.  
  
> [!NOTE]  
>  Les objets `xref:Microsoft.AnalysisServices.Measure` n'ont aucune relation avec les mesures calculées dans les modèles tabulaires et ne sont pas pris en charge dans ces modèles.  
  
 Consultez [représentation de la mesure calculée &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-calculated-measure-representation.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de mesure calculée.  
  
### <a name="hierarchy-representation"></a>Représentation d'une hiérarchie  
 Les hiérarchies permettent à l'utilisateur final d'explorer plus facilement les objets. En termes d'objets AMO, une représentation de hiérarchie a une relation de mappage un-à-un avec `xref:Microsoft.AnalysisServices.Hierarchy` et aucun autre objet AMO principal n'est requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans l'objet de base de données AMO peuvent être utilisés lors de la modélisation tabulaire.  
  
 Consultez [la représentation sous forme de hiérarchie &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-hierarchy-representation.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de hiérarchie.  
  
### <a name="key-performance-indicator-kpi--representation"></a>Représentation d'un indicateur de performance clé (KPI)  
 Un KPI évalue la performance d'une valeur, définie par une mesure de base, par rapport à une valeur cible. En termes d'objets AMO, la représentation d'un KPI a une relation de mappage un-à-plusieurs. Un KPI est représenté par l’utilisation des objets AMO suivants : `xref:Microsoft.AnalysisServices.MdxScript.Commands%2A` et `xref:Microsoft.AnalysisServices.MdxScript.CalculationProperties%2A` sont les principaux objets requis.  Il est important de noter que cela ne signifie pas que tous les objets contenus dans les objets AMO mentionnés précédemment peuvent être utilisés lors de la modélisation.  
  
> [!NOTE]  
>  En outre, il existe une différence importante, les objets `xref:Microsoft.AnalysisServices.Kpi` n'ont aucune relation avec les indicateurs de performance clés dans les modèles tabulaires. Et ils ne sont pas pris en charge dans les modèles tabulaires.  
  
 Consultez [représentation d’indicateur de Performance clé &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-key-performance-indicator-representation.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de l’indicateur de performance clé.  
  
### <a name="partition-representation"></a>Représentation d'une partition  
 À des fins opérationnelles, une table peut être divisée en différents sous-ensembles de lignes qui, lorsqu’elles sont combinées ensemble, la table. Chacun de ces sous-ensembles est une partition de la table. En termes d’objets AMO, une représentation de partition a une relation de mappage avec `xref:Microsoft.AnalysisServices.Partition` et aucun autre objet AMO principal n’est requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans l'objet de base de données AMO peuvent être utilisés lors de la modélisation.  
  
 Consultez [représentation d’une Partition &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/tables-partition-representation.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de partition.  
  
## <a name="relationship-representation"></a>Représentation d'une relation  
 Une relation est une connexion entre deux tables de données. La relation établit la façon dont les données des deux tables doivent être mises en corrélation.  
  
 Dans les modèles tabulaires, plusieurs relations peuvent être définies entre deux tables. Lorsque plusieurs relations entre deux tables sont définies, une seule peut être définie comme relation active par défaut. Toutes les autres relations sont inactives.  
  
 En termes d'objets AMO, toutes les relations inactives ont une représentation d'une relation de mappage un-à-un avec `xref:Microsoft.AnalysisServices.Relationship` et aucun autre objet AMO principal n'est requis. Pour la relation active, d'autres conditions existent et un mappage à `xref:Microsoft.AnalysisServices.ReferenceMeasureGroupDimension` est également requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans la relation AMO ou l'objet referenceMeasureGroupDimension peuvent être utilisés lors de la modélisation.  
  
 Consultez [la représentation sous forme de relation &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/relationship-representation-tabular.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de relation.  
  
## <a name="perspective-representation"></a>Représentation d'une perspective  
 Une perspective est un mécanisme pour simplifier ou mieux cibler le mode. En termes d'objets AMO, une représentation de relation a une relation de mappage un-à-un avec `xref:Microsoft.AnalysisServices.Perspective` et aucun autre objet AMO principal n'est requis. Il est important de noter que cela ne signifie pas que tous les objets contenus dans l’objet de perspective AMO peuvent être utilisés lors de la modélisation tabulaire.  
  
 Consultez [représentation d’une Perspective &#40;tabulaire&#41; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/perspective-representation-tabular.md) pour une explication détaillée sur la façon de créer et manipuler la représentation sous forme de perspective.  
  
> [!WARNING]  
>  Les perspectives ne sont pas un mécanisme de sécurité ; les objets en dehors de la perspective sont toujours accessibles à l'utilisateur via d'autres interfaces.  
  
  
