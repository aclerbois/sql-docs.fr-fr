---
title: Dimensions dans les modèles multidimensionnels | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cdae19df652c081290f04d6227a394aa6d90676f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimensions-in-multidimensional-models"></a>Dimensions dans les modèles multidimensionnels
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une dimension de base de données est un ensemble d'objets associés, appelés attributs, qui peuvent être utilisés pour fournir des informations sur des données dans un ou plusieurs cubes. Par exemple, les attributs les plus courants dans une dimension de produit sont le nom, la catégorie, la gamme, la taille et le prix du produit. Ces objets sont liés à une ou plusieurs colonnes dans une ou plusieurs tables d'une vue de source de données. Par défaut, ces attributs sont visibles en tant que hiérarchies d'attributs et peuvent être utilisés pour comprendre les données de faits dans un cube. Les attributs peuvent être organisés en hiérarchies définies par l'utilisateur qui fournissent des chemins d'exploration pour aider les utilisateurs lorsqu'ils recherchent des données dans un cube.  
  
 Les cubes contiennent toutes les dimensions sur lesquelles reposent les analyses de données. Une instance d'une dimension de base de données dans un cube porte le nom de dimension de cube et porte sur un ou plusieurs groupes de mesures dans le cube. Une dimension de base de données peut être utilisée plusieurs fois dans un cube. Par exemple, une table de faits peut comporter plusieurs faits dépendants de l'heure, et une dimension de cube distincte peut être définie pour permettre l'analyse de chaque fait dépendant de l'heure. Cependant, il ne doit exister qu'une seule dimension de base de données dépendant de l'heure, ce qui signifie également qu'une seule table de cette base de données doit exister pour prendre en charge plusieurs dimensions de cube basées sur l'heure.  
  
> [!NOTE]  
>  Pour obtenir des informations sur les problèmes de performances liés à la conception de dimensions, consultez le [Guide des performances SQL Server 2008 R2 Analysis Services](http://go.microsoft.com/fwlink/?LinkId=306717).  
  
## <a name="defining-dimensions-attributes-and-hierarchies"></a>Définition de dimensions, d'attributs et de hiérarchies  
 La méthode la plus simple pour définir des dimensions, des attributs et des hiérarchies de base de données et de cube consiste à utiliser l'Assistant Cube pour les créer en même temps que la définition du cube. L'Assistant Cube crée les dimensions sur la base des tables de dimension identifiées par l'Assistant dans la vue de source de données ou que vous spécifiez pour une utilisation dans le cube. L'Assistant crée ensuite les dimensions de base de données et les ajoute au nouveau cube, en créant les dimensions de cube.  
  
 Lorsque vous créez un cube, vous pouvez également ajouter au cube toute dimension existant déjà dans la base de données. Ces dimensions peuvent avoir été définies antérieurement pour un autre cube ou par l'Assistant Dimension. Une fois qu'une dimension de base de données a été définie, vous pouvez la modifier et la configurer dans le Concepteur de dimensions. Vous pouvez également personnaliser la dimension de cube, dans une certaine mesure, dans le Concepteur de cube.  
  
> [!NOTE]  
>  Vous pouvez également concevoir et configurer des dimensions, des attributs et des hiérarchies par programme à l'aide de XMLA ou d'objets AMO (Analysis Management Objects). Pour plus d’informations, consultez [Langage de script Analysis Services &#40;ASSL pour XMLA&#41;](../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md) et [Développement avec AMO &#40;Analysis Management Objects&#41;](../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 Le tableau suivant décrit les rubriques de cette section.  
  
 [Définir les Dimensions de la base de données](../../analysis-services/multidimensional-models/define-database-dimensions.md)  
 Explique comment modifier et configurer une dimension de base de données à l'aide du Concepteur de dimensions.  
  
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
 Explique comment définir, modifier et configurer un attribut de dimension de base de données à l'aide du Concepteur de dimensions.  
  
 [Définir des relations d’attributs](../../analysis-services/multidimensional-models/attribute-relationships-define.md)  
 Explique comment définir, modifier et configurer une relation d'attribut à l'aide du Concepteur de dimensions.  
  
 [Créer des hiérarchies définies par l’utilisateur](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)  
 Explique comment définir, modifier et configurer une hiérarchie d'attributs de dimension définie par l'utilisateur à l'aide du Concepteur de dimensions.  
  
 [Utilisez l’Assistant Business Intelligence pour améliorer des Dimensions](http://msdn.microsoft.com/library/12d995d1-75ca-4890-bf4b-a2656910b8d0)  
 Explique comment améliorer une dimension de base de données à l'aide de l'Assistant Business Intelligence.  
  
## <a name="see-also"></a>Voir aussi  
 [Cubes dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/cubes-in-multidimensional-models.md)  
  
  
