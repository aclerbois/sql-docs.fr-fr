---
title: Extension d’OLAP par des personnalisations | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1c49d2d350504daef36d0fbe861b26ccf220e7a7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="extending-olap-through-personalizations"></a>Extension d'OLAP par des personnalisations
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Analysis Services fournit de nombreuses fonctions intrinsèques à utiliser avec les langages MDX (Multidimensional Expressions) et les Extensions DMX (Data Mining). Ces fonctions sont conçues pour accomplir toutes les opérations possibles, depuis les calculs statistiques standard jusqu'au parcours des membres d'une hiérarchie. Cependant, comme avec tout autre produit complexe et puissant, il est toujours nécessaire d'étendre les fonctionnalités de ce type d'outil.  
  
 Par conséquent, Analysis Services vous permet d'ajouter des assemblys et des extensions personnalisées à une instance du service, afin de répondre aux besoins de votre entreprise chaque fois que les fonctionnalités standard sont insuffisantes.  
  
## <a name="assemblies"></a>Assemblys  
 Les assemblys vous permettent d'étendre les fonctions d'entreprise de MDX et de DMX. Vous générez les fonctionnalités souhaitées dans une bibliothèque, telle qu'une bibliothèque de liens dynamiques (DLL), puis vous ajoutez cette bibliothèque en tant qu'assembly à une instance d'Analysis Services ou à une base de données Analysis Services. Les méthodes publiques de la bibliothèque sont alors exposées en tant que fonctions définies par l'utilisateur aux expressions, procédures, calculs, actions et applications clientes MDX et DMX.  
  
## <a name="personalized-extensions"></a>Extensions personnalisées  
 Les extensions de personnalisation SQL Server Analysis Services sont la base de l'idée d'implémentation d'une architecture de plug-in. Elles sont une modification simple et élégante de l'architecture d'assembly managée existante et sont exposées partout dans le modèle objet Analysis Services <xref:Microsoft.AnalysisServices.AdomdServer>, la syntaxe MDX (Multidimensional Expressions) et les ensembles de lignes de schéma.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des assemblys de modèle multidimensionnel](../../../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)   
 [Extensions de personnalisation de Analysis Services](../../../analysis-services/multidimensional-models/extending-olap/analysis-services-personalization-extensions.md)  
  
  
