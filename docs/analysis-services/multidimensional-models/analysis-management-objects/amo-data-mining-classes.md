---
title: Classes d’exploration de données AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 803e6738fc526f68b8937efaa2b65be1b7d30dc4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="amo-data-mining-classes"></a>Classes d'exploration de données AMO
  Les classes d'exploration de données vous permettent de créer, modifier, supprimer et traiter les objets d'exploration de données. L'utilisation d'objets d'exploration de données consiste notamment à créer des structures d'exploration de données, à créer des modèles d'exploration de données et à traiter les modèles.  
  
 Pour plus d’informations sur comment configurer l’environnement, puis sur <xref:Microsoft.AnalysisServices.Server>, <xref:Microsoft.AnalysisServices.Database>, <xref:Microsoft.AnalysisServices.DataSource>, et <xref:Microsoft.AnalysisServices.DataSourceView> , consultez [des Classes fondamentales AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-fundamental-classes.md).  
  
 La définition d'objets dans AMO (Analysis Management Objects) impose de définir plusieurs propriétés sur chaque objet pour configurer le contexte adéquat. Les objets complexes, tels que les objets OLAP et d'exploration de données, nécessitent un codage long et fastidieux.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Objets MiningStructure](#MiningStructure)  
  
-   [Objets MiningModel](#MiningModel)  
  
 L'illustration suivante montre la relation qui existe entre les classes décrites dans cette rubrique.  
  
 ![Classes DataMining AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-dataminingclasses.gif "Classes DataMining AMO")  
  
##  <a name="MiningStructure"></a> Objets MiningStructure  
 Une structure d'exploration de données fait office de conteneur pour les modèles d'exploration de données. La structure définit toutes les colonnes que les modèles d'exploration de données sont susceptibles d'utiliser. Chaque modèle d'exploration de données définit ses propres colonnes à partir du jeu de colonnes définies dans la structure.  
  
 Un objet <xref:Microsoft.AnalysisServices.MiningStructure> simple se compose des éléments suivants : des informations de base, une vue de source de données, un ou plusieurs objets <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>, zéro, un ou plusieurs objets <xref:Microsoft.AnalysisServices.TableMiningStructureColumn>, ainsi qu'un objet <xref:Microsoft.AnalysisServices.MiningModelCollection>.  
  
 Les informations de base sont constituées du nom et de l'ID (identificateur interne) de l'objet <xref:Microsoft.AnalysisServices.MiningStructure>.  
  
 L'objet <xref:Microsoft.AnalysisServices.DataSourceView> contient le modèle de données sous-jacent de la structure d'exploration de données.  
  
 Les objets <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn> sont des colonnes ou des attributs qui contiennent des valeurs uniques.  
  
 Les objets <xref:Microsoft.AnalysisServices.TableMiningStructureColumn> sont des colonnes ou des attributs qui contiennent plusieurs valeurs pour chaque cas.  
  
 L'objet <xref:Microsoft.AnalysisServices.MiningModelCollection> contient tous les modèles d'exploration de données basés sur les mêmes données.  
  
 Pour créer un objet <xref:Microsoft.AnalysisServices.MiningStructure>, il convient de l'ajouter à l'objet <xref:Microsoft.AnalysisServices.MiningStructureCollection> de la base de données et de mettre à jour l'objet <xref:Microsoft.AnalysisServices.MiningStructure> sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.MiningStructure>, il est nécessaire d'utiliser la méthode Drop de ce même <xref:Microsoft.AnalysisServices.MiningStructure> objet. La suppression d'un objet <xref:Microsoft.AnalysisServices.MiningStructure> de la collection n'a aucun effet sur le serveur.  
  
 Le <xref:Microsoft.AnalysisServices.MiningStructure> peuvent être traitées à l’aide de sa propre méthode process ou elle peut être traitée lorsqu’un objet parent lui-même traite avec sa propre méthode process.  
  
### <a name="columns"></a>Columns  
 Les colonnes contiennent les données du modèle et peuvent être de différents types selon l'utilisation : Key, Input, Predictable ou InputPredictable. Les colonnes prédictibles sont la cible de la génération du modèle d'exploration de données.  
  
 Les colonnes à valeur unique sont appelées <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn> dans AMO. Les colonnes à valeurs multiples sont appelées <xref:Microsoft.AnalysisServices.TableMiningStructureColumn>.  
  
#### <a name="scalarminingstructurecolumn"></a>ScalarMiningStructureColumn  
 Un objet <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn> simple est constitué d'informations de base, d'un type, d'un contenu et d'une liaison de données.  
  
 Les informations de base se composent du nom et de l'ID (identificateur interne) de l'objet <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>.  
  
 Le type correspond au type de données de la valeur : LONG, BOOLEAN, TEXT, DOUBLE, DATE.  
  
 Le contenu indique au moteur la façon dont la colonne peut être modélisée. Les valeurs possibles sont les suivantes : Discrete, Continuous, Discretized, Ordered, Cyclical, Probability, Variance, StdDev, ProbabilityVariance, ProbabilityStdDev, Support, Key.  
  
 La liaison de données lie la colonne d'exploration de données au modèle de données sous-jacent en utilisant un élément de vue de source de données.  
  
 Pour créer un objet <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>, il convient de l'ajouter à l'objet <xref:Microsoft.AnalysisServices.MiningStructureCollection> parent et de mettre à jour l'objet <xref:Microsoft.AnalysisServices.MiningStructure> parent sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer un <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>, il doit être supprimé de la collection du parent <xref:Microsoft.AnalysisServices.MiningStructure>et le parent <xref:Microsoft.AnalysisServices.MiningStructure> objet doit être mis à jour sur le serveur à l’aide de la méthode de mise à jour.  
  
#### <a name="tableminingstructurecolumn"></a>TableMiningStructureColumn  
 Un objet <xref:Microsoft.AnalysisServices.TableMiningStructureColumn> simple est constitué d'informations de base et de colonnes scalaires.  
  
 Les informations de base se composent du nom et de l'ID (identificateur interne) de l'objet <xref:Microsoft.AnalysisServices.TableMiningStructureColumn>.  
  
 Les colonnes scalaires sont des objets <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>.  
  
 Pour créer un objet <xref:Microsoft.AnalysisServices.TableMiningStructureColumn>, il convient de l'ajouter à la collection <xref:Microsoft.AnalysisServices.MiningStructure> parente et de mettre à jour l'objet <xref:Microsoft.AnalysisServices.TableMiningStructureColumn> parent sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn>, il convient de le supprimer de la collection de l'objet <xref:Microsoft.AnalysisServices.MiningStructure> parent, et l'objet <xref:Microsoft.AnalysisServices.MiningStructure> parent doit être mis à jour sur le serveur à l'aide de la méthode Remove.  
  
##  <a name="MiningModel"></a> Objets MiningModel  
 <xref:Microsoft.AnalysisServices.MiningModel> est l'objet qui vous permet de sélectionner les colonnes de la structure à utiliser, l'algorithme à utiliser et éventuellement des paramètres spécifiques afin de paramétrer le modèle. Par exemple, vous pouvez souhaiter définir plusieurs modèles d'exploration de données dans une même structure d'exploration de données qui utilisent les mêmes algorithmes. Vous pouvez également souhaiter ignorer certaines colonnes de la structure d'exploration de données dans un modèle, les utiliser comme entrées dans un autre modèle et les utiliser comme colonnes prédictibles et d'entrée dans un troisième modèle. Cela peut s'avérer utile si vous souhaitez traiter une colonne en tant que colonne continue dans un modèle d'exploration de données mais que vous souhaitez la traiter en tant que colonne discrétisée dans un autre.  
  
 Un objet <xref:Microsoft.AnalysisServices.MiningModel> simple est constitué d'informations de base, d'une définition d'algorithme et de colonnes.  
  
 Les informations de base sont constituées du nom et de l'ID (identificateur interne) du modèle d'exploration de données.  
  
 Une définition d'algorithme fait référence à l'un des algorithmes standard fournis dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ou à un algorithme personnalisé activé sur le serveur.  
  
 Les colonnes représentent une collection des colonnes utilisées par l'algorithme et la définition de leur utilisation.  
  
 A <xref:Microsoft.AnalysisServices.MiningModel> est créé en l’ajoutant à la <xref:Microsoft.AnalysisServices.MiningModelCollection> de la base de données et mise à jour le <xref:Microsoft.AnalysisServices.MiningModel> objet sur le serveur à l’aide de la méthode de mise à jour.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.MiningModel>, il est nécessaire d'utiliser la méthode Drop de l'objet <xref:Microsoft.AnalysisServices.MiningModel>. Suppression d’un <xref:Microsoft.AnalysisServices.MiningModel> à partir de la collection n’affecte pas le serveur.  
  
 Une fois créé, un objet <xref:Microsoft.AnalysisServices.MiningModel> peut être traité avec sa propre méthode Process ou avec celle de l'objet parent au moment où celui-ci est traité.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Classes fondamentales AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-fundamental-classes.md)   
 [Programmation d’objets d’exploration de données AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-data-mining-objects.md)   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
