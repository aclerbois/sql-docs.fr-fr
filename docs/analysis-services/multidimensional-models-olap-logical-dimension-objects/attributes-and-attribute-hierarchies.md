---
title: Attributs et hiérarchies d’attributs | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5e30f3be6270cdc62b4b17048f9032a7c5587557
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attributes-and-attribute-hierarchies"></a>Attributs et hiérarchies d'attributs
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les dimensions sont des ensembles d'attributs, liés à une ou plusieurs colonnes dans une table ou une vue de la vue de source de données.  
  
## <a name="key-attribute"></a>Attribut clé  
 Chaque dimension contient un attribut clé. Chaque attribut est lié à une ou plusieurs colonnes dans une table de dimension. Un attribut clé est l'attribut d'une dimension, qui identifie les colonnes dans la table principale de la dimension qui sont utilisées dans des relations de clé étrangère avec la table de faits. Généralement, l'attribut clé représente la colonne ou les colonnes clés primaires de la table de dimension. Vous pouvez définir une clé primaire logique dans une table d'une vue de source de données qui n'a pas de clé primaire physique dans la source de données sous-jacente. **Pour plus d’informations**, consultez [définir des clés primaires logiques dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-primary-keys-in-a-data-source-view-analysis-services.md). Lorsque vous définissez des attributs clés, l'Assistant Cube et l'Assistant Dimension tentent d'utiliser les colonnes clés primaires de la table de dimension dans la vue de source de données. Si la table de dimension n'a pas une clé primaire logique ou une clé primaire physique, les Assistants ne peuvent pas définir correctement les attributs clés de la dimension.  
  
## <a name="binding-an-attribute-to-columns-in-data-source-view-tables-or-views"></a>Liaison d'un attribut à des colonnes de vues ou de tables de vue de source de données  
 Un attribut est lié à des colonnes d'une ou plusieurs vues ou tables de vue de source de données. Il est toujours lié à une ou plusieurs colonnes clés, qui déterminent les membres qui sont contenus dans l'attribut. Par défaut, il s'agit de la seule colonne à laquelle est lié un attribut. Un attribut peut aussi être lié à une ou plusieurs autres colonnes à des fins spécifiques. Par exemple, d’un attribut **NameColumn** propriété détermine le nom qui apparaît à l’utilisateur pour chaque membre d’attribut, cette propriété de l’attribut peut être lié à une colonne de dimension particulière via une vue de source de données ou peut être liée à une colonne calculée dans la vue de source de données. Pour plus d’informations, consultez [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md).  
  
## <a name="attribute-hierarchies"></a>Hiérarchies d'attributs  
 Par défaut, les membres d'attribut sont organisés en hiérarchies à deux niveaux, constituées d'un niveau feuille et d'un niveau Tous. Le niveau Tous contient la valeur agrégée des membres de l'attribut à travers les mesures de chaque groupe de mesures dont la dimension à laquelle l'attribut se rapporte est membre. Toutefois, si le **IsAggregatable** est définie sur False, le niveau tous n’est pas créé. Pour plus d’informations, consultez [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md).  
  
 Les attributs peuvent être - et c'est généralement le cas - organisés en hiérarchies définies par l'utilisateur, qui fournissent des chemins d'accès d'exploration par lesquels les utilisateurs peuvent parcourir les données dans les groupes de mesures auquel l'attribut se rapporte. Dans les applications clientes, les attributs peuvent être utilisés pour fournir des informations de regroupement et de contrainte. Lorsque les attributs sont organisés en hiérarchies définies par l’utilisateur, vous définissez des relations entre les niveaux de hiérarchie lorsque les niveaux sont liées dans un plusieurs-à-un ou une relation un à un (appelée un *naturelle* relation). Par exemple, dans une hiérarchie de calendrier, le niveau Jour doit être associé au niveau Mois, le niveau Mois doit être associé au niveau Trimestre, etc. La création de relations entre les niveaux dans une hiérarchie définie par l'utilisateur permet à Analysis Services de définir plusieurs agrégations utiles pour améliorer les performances des requêtes et économiser la mémoire lors du traitement, ce qui peut être crucial avec des cubes complexes et volumineux. Pour plus d’informations, consultez [hiérarchies utilisateur](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md), [pourquoi hiérarchies](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md), et [définir des relations d’attributs](../../analysis-services/multidimensional-models/attribute-relationships-define.md).  
  
## <a name="attribute-relationships-star-schemas-and-snowflake-schemas"></a>Relations d'attributs, schémas en étoile et schémas en flocon  
 Par défaut, dans un schéma en étoile, tous les attributs sont directement liés à l'attribut clé, qui permet aux utilisateurs de parcourir les faits du cube sur la base de l'une des hiérarchies d'attributs de la dimension. Dans un schéma en flocon, tous les attributs sont soit directement liés à l'attribut clé si leur table sous-jacente est directement liée à la table de faits, soit indirectement liés au moyen de l'attribut qui est lié à la clé de la table sous-jacente qui lie la table en flocon à la table directement liée.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer des hiérarchies définies par l’utilisateur](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)   
 [Définir des relations d’attributs](../../analysis-services/multidimensional-models/attribute-relationships-define.md)   
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
