---
title: Présentation des Dimensions (Analysis Services - données multidimensionnelles) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 97353e1662d9dbddee72cfee288299c222ab7992
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimensions---introduction"></a>Dimensions : Introduction
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Tous les Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] les dimensions sont des groupes d’attributs basés sur des colonnes de tables ou vues dans une vue de source de données. Les dimensions existent indépendamment d'un cube, elles peuvent être utilisées dans plusieurs cubes et plusieurs fois dans un même cube, ainsi que liées entre les instances d'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Une dimension qui existe indépendamment d'un cube est appelée dimension de base de données et une instance de dimension de base de données dans un cube est appelée dimension de cube.  
  
## <a name="dimension-based-on-a-star-schema-design"></a>Dimension basée sur un schéma en étoile  
 La structure d'une dimension repose principalement sur la structure de la table ou des tables de dimension sous-jacentes. La structure la plus simple est appelée schéma en étoile, où chaque dimension est basée sur une seule table de dimension qui est directement liée à la table de faits par une relation clé primaire - clé étrangère.  
  
 Le diagramme suivant montre une sous-section de le [!INCLUDE[ssSampleDBDWobject](../../includes/sssampledbdwobject-md.md)] base de données exemple, dans lequel le **FactResellerSales** table de faits est associée à deux tables de dimension, **DimReseller** et **DimPromotion**. Le **ResellerKey** colonne dans la **FactResellerSales** table de faits définit une relation de clé étrangère avec la **ResellerKey** colonne clé primaire de la **DimReseller** table de dimension. De même, la **PromotionKey** colonne dans la **FactResellerSales** table de faits définit une relation de clé étrangère avec la **PromotionKey** colonne clé primaire de la **DimPromotion** table de dimension.  
  
 ![Schéma logique pour la relation de dimension](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/media/dimfactrelationship.gif "schéma logique pour la relation de dimension")  
  
## <a name="dimension-based-on-a-snowflake-schema-design"></a>Dimension basée sur un schéma en flocon  
 Une structure plus complexe est fréquemment requise parce que des informations provenant de plusieurs tables sont nécessaires pour définir la dimension. Dans cette structure, appelée un schéma en flocon, chaque dimension est basée sur les attributs de plusieurs tables liées les unes aux autres ainsi qu'à la table de faits par des relations clé primaire - clé étrangère. Par exemple, le diagramme suivant illustre les tables nécessaires pour décrire complètement la dimension Product dans la **AdventureWorksDW** exemple de projet :  
  
 ![Tables de dimension Product d’AdventureWorksAS](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/media/dimproduct.gif "Tables de dimension Product d’AdventureWorksAS")  
  
 Pour décrire complètement un produit, la catégorie et la sous-catégorie du produit doivent être incluses dans la dimension Product. Toutefois, ces informations ne résident pas directement dans la table principale pour la **DimProduct** dimension. Une relation de clé étrangère à partir de **DimProduct** à **DimProductSubcategory**, qui a à son tour une relation de clé étrangère avec la **DimProductCategory** table, permet d’inclure les informations de catégories de produits et de sous-catégories dans la dimension Product.  
  
## <a name="snowflake-schema-versus-reference-relationship"></a>Schéma en flocon et relation de référence  
 Parfois, vous pouvez être amené à choisir entre l'utilisation d'un schéma en flocons pour définir les attributs d'une dimension à partir de plusieurs tables ou la définition de deux dimensions distinctes avec l'établissement d'une relation de dimensions de référence entre celles-ci. Le diagramme suivant montre ce scénario.  
  
 ![Schéma logique pour un exemple de dimension référencée](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/media/dimindirect.gif "schéma logique pour un exemple de dimension référencée")  
  
 Dans le diagramme précédent, le **FactResellerSales** table de faits n’a pas d’une relation de clé étrangère avec la **DimGeography** table de dimension. Toutefois, le **FactResellerSales** table de faits a une relation de clé étrangère avec la **DimReseller** table de dimension, qui à son tour a une relation de clé étrangère avec la **DimGeography** table de dimension. Pour définir une dimension Reseller qui contient des informations géographiques sur chaque revendeur, vous devez extraire les attributs à partir de la **DimGeography** et **DimReseller** tables de dimension. Cependant, dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez obtenir le même résultat en créant deux dimensions distinctes et en les liant avec un groupe de mesures en définissant une relation de dimensions de référence entre les deux dimensions. Pour plus d’informations sur les relations de dimension de référence, consultez [relations de Dimension](../../analysis-services/multidimensional-models-olap-logical-cube-objects/dimension-relationships.md).  
  
 Un avantage des relations de dimensions de référence dans ce scénario est que vous pouvez créer une seule dimension Geography, puis créer plusieurs dimensions de cube basés sur cette dimension, sans qu'un espace de stockage supplémentaire soit nécessaire. Par exemple, vous pouvez lier une des dimensions de cube Geography à une dimension Reseller, et une autre dimension de cube Geography à une dimension Customer. **Rubriques connexes :**[relations de Dimension](../../analysis-services/multidimensional-models-olap-logical-cube-objects/dimension-relationships.md), [définir une relation référencée et des propriétés de relation référencée](../../analysis-services/multidimensional-models/define-a-referenced-relationship-and-referenced-relationship-properties.md)  
  
## <a name="processing-a-dimension"></a>Traitement d'une dimension  
 Après avoir créé une dimension, vous devez traiter celle-ci avant de pouvoir afficher les membres des attributs et des hiérarchies dans la dimension. Après avoir modifié la structure d'une dimension ou après la mise à jour des informations de ses tables sous-jacentes, vous devez traiter à nouveau la dimension pour pouvoir afficher les modifications. Lorsque vous traitez une dimension après des changements structuraux, vous devez également traiter les cubes qui comprennent cette dimension, sinon le cube ne sera pas visible.  
  
## <a name="security"></a>Sécurité  
 Tous les objets subordonnés d'une dimension, y compris les hiérarchies, les niveaux et les membres, sont protégés en utilisant des rôles dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. La sécurité d'une dimension peut être appliquée à tous les cubes de la base de données qui utilisent la dimension, ou à un cube donné seulement. Pour plus d’informations sur la sécurité de dimension, consultez [accorder des autorisations sur une dimension &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-a-dimension-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Stockage de dimension](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-storage.md)   
 [Traductions de dimension](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimension-translations.md)   
 [Dimensions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md)  
  
  
