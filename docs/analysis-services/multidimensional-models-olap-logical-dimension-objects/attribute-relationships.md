---
title: Relations d’attributs | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9035cc2c0d3308848b570ae6c157c9d70cd77cc9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attribute-relationships"></a>Relations d’attributs
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], attributs au sein d’une dimension sont toujours liés directement ou indirectement à l’attribut clé. Quand vous définissez une dimension basée sur un schéma en étoile, c'est-à-dire quand tous les attributs de la dimension sont dérivés de la même table relationnelle, une relation d'attribut est automatiquement définie entre l'attribut clé et chaque attribut non-clé de la dimension. Quand vous définissez une dimension basée sur un schéma en flocon, où les attributs de la dimension sont dérivés de plusieurs tables liées, une relation d'attribut est automatiquement définie comme suit :  
  
-   entre l'attribut clé et chaque attribut non-clé lié aux colonnes de la table de dimension principale ;  
  
-   entre l'attribut clé et l'attribut lié à la clé étrangère dans la table secondaire qui lie les tables de dimension sous-jacentes ;  
  
-   entre l'attribut lié à la clé étrangère de la table secondaire et chaque attribut non-clé lié aux colonnes de la table secondaire.  
  
 Cependant, pour plusieurs raisons, vous pouvez souhaiter modifier ces relations d'attributs par défaut. Par exemple, vous voulez définir une hiérarchie naturelle, un ordre de tri personnalisé ou une granularité de dimension basée sur un attribut non-clé. Pour plus d’informations, consultez [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md).  
  
> [!NOTE]  
>  Les relations d'attributs sont appelées propriétés de membre dans les expressions MDX (Multidimensional Expressions).  
  
## <a name="natural-hierarchy-relationships"></a>Relations de hiérarchie naturelle  
 Une hiérarchie une hiérarchie naturelle quand chaque attribut inclus dans la hiérarchie définie par l'utilisateur possède une relation un-à-plusieurs avec l'attribut se trouvant immédiatement au-dessous de lui. Imaginons par exemple une dimension Customer basée sur une table source relationnelle avec huit colonnes :  
  
-   CustomerKey  
  
-   CustomerName  
  
-   Age  
  
-   Gender  
  
-   Email  
  
-   Ville  
  
-   Pays  
  
-   Région  
  
 La dimension Analysis Services correspondante a sept attributs :  
  
-   Customer (basée sur CustomerKey, avec CustomerName fournissant les noms des membres)  
  
-   Age, Gender, Email, City, Region, Country  
  
 Les relations représentant des hiérarchies naturelles sont appliquées en créant une relation d'attribut entre l'attribut d'un niveau et l'attribut du niveau qui se trouve au-dessous. Pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ceci spécifie une relation naturelle et une agrégation potentielle. Dans la dimension Customer, une hiérarchie naturelle existe pour les attributs Country, Region, City et Customer. La hiérarchie naturelle pour `{Country, Region, City, Customer}` est décrite en ajoutant les relations d'attributs suivantes :  
  
-   l'attribut Country en tant que relation d'attribut de l'attribut Region ;  
  
-   l'attribut Region en tant que relation d'attribut de l'attribut City ;  
  
-   l'attribut City en tant que relation d'attribut de l'attribut Customer.  
  
 Pour parcourir les données dans le cube, vous pouvez également créer une hiérarchie définie par l’utilisateur qui ne représente pas une hiérarchie naturelle dans les données (qui est appelée un *ad hoc* ou *reporting* hiérarchie). Par exemple, vous pouvez créer une hiérarchie définie par l'utilisateur basée sur `{Age, Gender}`. Les utilisateurs ne voient aucune différence quant à la façon dont les deux hiérarchies se comportent, bien que la hiérarchie naturelle bénéficie de structures d'agrégation et d'indexation, qui sont cachées à l'utilisateur et qui représentent les relations naturelles de la source de données.  
  
 Le **SourceAttribute** propriété d’un niveau détermine l’attribut utilisé pour décrire le niveau. Le **KeyColumns** propriété sur l’attribut spécifie la colonne dans la vue de source de données qui fournit les membres. Le **NameColumn** propriété sur l’attribut peut spécifier un autre nom de colonne pour les membres.  
  
 Pour définir un niveau dans une hiérarchie définie par l’utilisateur à l’aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], le **Concepteur de dimensions** vous permet de sélectionner un attribut de dimension, d’une colonne dans une table de dimension ou d’une colonne d’une table associée incluse dans la vue de source de données pour le cube. Pour plus d’informations sur la création de hiérarchies définies par l’utilisateur, consultez [pourquoi hiérarchies](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md).  
  
 Dans Analysis Services, une hypothèse est généralement faite à propos du contenu des membres. Les membres feuilles n'ont pas de descendants et contiennent des données dérivées des sources de données sous-jacentes. Les membres non-feuilles ont des descendants et contiennent des données dérivées des agrégations effectuées sur les membres enfants. Dans les niveaux agrégés, les membres sont basés sur les agrégations des niveaux inférieurs. Par conséquent, lorsque le **IsAggregatable** est définie sur **False** sur un attribut source d’un niveau, aucun attribut agrégeable ne doit être ajoutée en tant que niveaux au-dessus de lui.  
  
## <a name="defining-an-attribute-relationship"></a>Définition d'une relation d'attribut  
 La principale contrainte lorsque vous créez une relation d'attribut consiste à veiller à ce que l'attribut auquel la relation d'attribut fait référence ne dispose que d'une seule valeur pour chaque membre de l'attribut auquel la relation d'attribut appartient. Par exemple, si vous définissez une relation entre un attribut City (Ville) et un attribut State (État), chaque Ville ne peut être liée qu'à un seul État.  
  
## <a name="attribute-relationship-queries"></a>Requêtes de relations d'attributs  
 Vous pouvez utiliser des requêtes MDX pour récupérer des données de relations d’attributs, sous la forme de propriétés de membre, avec le **propriétés** (mot clé) de l’expression MDX **sélectionnez** instruction. Pour plus d’informations sur la façon d’utiliser MDX pour récupérer les propriétés de membre, consultez [à l’aide des propriétés de membre &#40;MDX&#41;](../../analysis-services/multidimensional-models/mdx/mdx-member-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)   
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)   
 [Hiérarchies utilisateur](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md)   
 [Propriétés de la hiérarchie utilisateur](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies-properties.md)  
  
  
