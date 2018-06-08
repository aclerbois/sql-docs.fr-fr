---
title: Objets (Analysis Services - données multidimensionnelles) de dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b296da3faed422aea0da42d01e0a6ece7333e39c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimension-objects-analysis-services---multidimensional-data"></a>Objets de dimension (Analysis Services - Données multidimensionnelles)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un objet <xref:Microsoft.AnalysisServices.Dimension> simple est composé des informations de base, des attributs et des hiérarchies. Les informations de base incluent le nom de la dimension, le type de dimension, la source de données, le mode de stockage et d'autres informations. Les attributs définissent les données effectives dans la dimension. Les attributs n'appartiennent pas nécessairement à une hiérarchie, mais les hiérarchies sont construites à partir d'attributs. Une hiérarchie crée des listes de niveaux triées et définit les façons dont la dimension peut être explorée par un utilisateur.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques suivantes fournissent des informations supplémentaires sur la manière de concevoir et d'implémenter des objets de dimension.  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Dimensions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)|Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], les dimensions sont un composant fondamental des cubes. Les dimensions organisent les données en relation avec un domaine d'intérêt (clients, magasins ou employés, par exemple) pour les utilisateurs.|  
|[Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)|Les dimensions sont des ensembles d'attributs, liés à une ou plusieurs colonnes dans une table ou une vue de la vue de source de données.|  
|[Relations d'attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attribute-relationships.md)|Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], attributs au sein d’une dimension sont toujours liés directement ou indirectement à l’attribut clé. Quand vous définissez une dimension basée sur un schéma en étoile, c'est-à-dire quand tous les attributs de la dimension sont dérivés de la même table relationnelle, une relation d'attribut est automatiquement définie entre l'attribut clé et chaque attribut non-clé de la dimension. Quand vous définissez une dimension basée sur un schéma en flocon, où les attributs de la dimension sont dérivés de plusieurs tables liées, une relation d'attribut est automatiquement définie comme suit :<br /><br /> entre l'attribut clé et chaque attribut non-clé lié aux colonnes de la table de dimension principale ;<br /><br /> entre l'attribut clé et l'attribut lié à la clé étrangère dans la table secondaire qui lie les tables de dimension sous-jacentes ;<br /><br /> entre l'attribut lié à la clé étrangère de la table secondaire et chaque attribut non-clé lié aux colonnes de la table secondaire.|  
  
  
