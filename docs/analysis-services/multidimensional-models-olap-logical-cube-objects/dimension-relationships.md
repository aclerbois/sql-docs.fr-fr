---
title: Relations de dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 454527cf0e2e5d444e87cc18b0277b861eb03d48
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimension-relationships"></a>Relations de dimension
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'utilisation d'une dimension définit les relations entre une dimension de cube et les groupes de mesures d'un cube. Une dimension de cube est une instance d'une dimension de base de données qui est utilisée dans un cube spécifique. Un cube peut (et c'est souvent le cas) avoir des dimensions de cube qui ne sont pas directement liées à un groupe de mesures, mais qui peuvent être indirectement liées au groupe de mesures via une autre dimension ou un autre groupe de mesures. Lorsque vous ajoutez un groupe de dimension ou la mesure de base de données à un cube, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente de déterminer l’utilisation de la dimension en examinant les relations entre les tables de dimension et les tables de faits dans la vue de source de données du cube et en examinant les relations entre les attributs dans les dimensions. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] définit automatiquement les paramètres d'utilisation de la dimension pour les relations qu'il peut détecter.  
  
 Une relation entre une dimension et un groupe de mesures est constituée des tables de dimension et de faits qui participent à la relation et d'un attribut de granularité qui spécifie la granularité de la dimension dans le groupe de mesures particulier.  
  
## <a name="regular-dimension-relationships"></a>Relations de dimension régulières  
 Une relation de dimension régulière entre une dimension de cube et un groupe de mesures existe quand la colonne clé de la dimension est directement jointe avec la table de faits. Cette relation directe est basée sur une relation clé primaire - clé étrangère dans la base de données relationnelle sous-jacente, mais elle peut aussi être basée sur une relation logique définie dans la vue de source de données. Une relation de dimension régulière représente la relation entre des tables de dimension et une table de faits dans une conception de schéma en étoile traditionnelle. Pour plus d’informations sur les relations régulières, consultez [définir une relation régulière et les propriétés d’une relation régulière](../../analysis-services/multidimensional-models/define-a-regular-relationship-and-regular-relationship-properties.md).  
  
## <a name="reference-dimension-relationships"></a>Relations de dimension de référence  
 Une relation de dimension de référence entre une dimension de cube et un groupe de mesures existe quand la colonne clé de la dimension est indirectement jointe à la table de faits par l'intermédiaire d'une clé d'une autre table de dimension, comme le montre l'illustration suivante.  
  
 ![Diagramme logique, relation de dimension référencée](../../analysis-services/multidimensional-models-olap-logical-cube-objects/media/as-refdimension1.gif "diagramme logique, relation de dimension référencée")  
  
 Une relation de dimension de référence représente la relation entre des tables de dimension et une table de faits dans une conception de schéma en flocons. Quand des tables de dimension sont connectées dans un schéma en flocons, vous pouvez définir une seule dimension en utilisant des colonnes de plusieurs tables ou vous pouvez définir des dimensions distinctes basées sur des tables de dimension distinctes puis définir un lien entre celles-ci en utilisant une relation de dimension de référence. La figure suivante illustre une table de faits nommée **InternetSales**, et deux tables de dimension **client** et **Geography**, dans un schéma en flocons.  
  
 ![Schéma logique, relation de dimension référencée](../../analysis-services/multidimensional-models-olap-logical-cube-objects/media/as-refdim-schema1.gif "schéma logique, relation de dimension référencée")  
  
 Vous pouvez créer une dimension avec la **client** table comme table de dimension principale et la **Geography** table incluse en tant qu’une table associée. Une relation régulière est ensuite définie entre la dimension et le groupe de mesures InternetSales.  
  
 Vous pouvez également créer deux dimensions associées au groupe de mesures InternetSales : une dimension basée sur les **client** table et une dimension basée sur les **Geography** table. Vous pouvez ensuite lier la dimension Geography au groupe de mesures InternetSales au moyen d'une relation de dimension de référence utilisant la dimension Customer. Dans ce cas, quand les faits du groupe de mesures InternetSales sont dimensionnés par la dimension Geography, les faits sont dimensionnés par client et par localisation géographique. Si le cube contenait un deuxième groupe de mesures appelé Reseller Sales, il ne vous serait pas possible de dimensionner les faits dans le groupe de mesures correspondant par Geography car il n'y aurait pas de relation entre Reseller Sales et Geography.  
  
 Vous pouvez chaîner ensemble un nombre illimité de dimensions de référence, comme le montre l'illustration suivante.  
  
 ![Diagramme logique, relation de dimension référencée](../../analysis-services/multidimensional-models-olap-logical-cube-objects/media/as-refdimension2.gif "diagramme logique, relation de dimension référencée")  
  
 Pour plus d’informations sur les relations référencées, consultez [définir une relation référencée et les propriétés d’une relation référencée](../../analysis-services/multidimensional-models/define-a-referenced-relationship-and-referenced-relationship-properties.md).  
  
## <a name="fact-dimension-relationships"></a>Relations de dimension de fait  
 Les dimensions de fait, fréquemment appelées dimensions dégénérées, sont des dimensions standard construites à partir de colonnes d'attributs de tables de faits, et non de tables de dimension. Des données dimensionnelles utiles sont parfois stockées dans une table de faits pour réduire la duplication. Par exemple, le diagramme suivant montre la **FactResellerSales** table de faits, à partir de la [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] base de données exemple.  
  
 ![Colonnes en fait table peut prendre en charge les dimensions](../../analysis-services/multidimensional-models-olap-logical-cube-objects/media/as-factdim.gif "colonnes en fait table peut prendre en charge les dimensions")  
  
 La table contient des informations d'attribut non seulement pour chaque ligne d'une commande émise par un revendeur, mais aussi sur la commande elle-même. Les attributs entourés dans le diagramme précédent identifient les informations contenues dans le **FactResellerSales** table qui peut être utilisé en tant qu’attributs dans une dimension. Dans ce cas, deux éléments d'informations supplémentaires, le numéro de suivi du transporteur et le numéro du bon de commande émis par le revendeur, sont représentés par les colonnes d'attribut CarrrierTrackingNumber et CustomerPONumber. Ces informations offrent un intérêt : par exemple, les utilisateurs voudront sûrement voir les informations agrégées, telles que le coût total des produits, de toutes les commandes expédiées sous un numéro de suivi. Mais, sans une dimension, il est impossible d'organiser ou d'agréger les données de ces deux attributs.  
  
 Théoriquement, vous pouvez créer une table de dimension qui utilise les mêmes informations de clé que la table FactResellerSales, et transférer les deux autres colonnes d'attribut, CarrierTrackingNumber et CustomerPONumber, vers la table de dimension. Toutefois, vous dupliquez alors une partie significative des données et augmentez inutilement la complexité dans l'entrepôt de données pour juste représenter deux attributs sous la forme d'une dimension distincte.  
  
> [!NOTE]  
>  Les dimensions de fait sont fréquemment utilisées pour prendre en charge des actions d'extraction. Pour plus d’informations sur les d’actions, consultez [Actions &#40;Analysis Services - Données multidimensionnelles&#41;](../../analysis-services/multidimensional-models/actions-analysis-services-multidimensional-data.md).  
  
> [!NOTE]  
>  Les dimensions de fait doivent être mises à jour de manière incrémentielle après chaque mise à jour du groupe de mesures référencé par la relation de fait. Si la dimension de fait est une dimension ROLAP, le moteur de traitement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] supprime les caches et traite de manière incrémentielle le groupe de mesures.  
  
 Pour plus d’informations sur les relations de faits, consultez [définir une relation de faits et les propriétés de relation de faits](../../analysis-services/multidimensional-models/define-a-fact-relationship-and-fact-relationship-properties.md).  
  
## <a name="many-to-many-dimension-relationships"></a>Relations de dimension plusieurs-à-plusieurs  
 Dans la plupart des dimensions, chaque fait joint un et un seul membre de dimension, un seul membre de dimension peut être associé à plusieurs faits. Dans la terminologie des bases de données relationnelles, on parle de relation un-à-plusieurs. Cependant, il est fréquemment utile de joindre un seul fait à plusieurs membres de dimension. Par exemple, un client d'une banque peut avoir plusieurs comptes (compte-chèque, compte d'épargne, compte de carte de crédit et compte d'investissement), et un compte peut avoir une jointure et plusieurs détenteurs. La dimension Customer créée à partir de ces relations a alors plusieurs membres qui font référence à une transaction de compte.  
  
 ![Relation de dimension logiques schéma/plusieurs-à-plusieurs](../../analysis-services/multidimensional-models-olap-logical-cube-objects/media/as-many-dimension1.gif "relation de dimension logiques schéma/plusieurs-à-plusieurs")  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] permet de définir une relation plusieurs-à-plusieurs entre une dimension et une table de faits.  
  
> [!NOTE]  
>  Pour pouvoir prendre en charge une relation de dimension plusieurs-à-plusieurs, la vue de source de données doit avoir établi une relation de clé étrangère entre toutes les tables impliquées, comme indiqué dans le schéma précédent. Dans le cas contraire, vous ne pouvez pas sélectionner le groupe de mesures intermédiaire approprié lors de l’établissement de la relation dans le **utilisation de la Dimension** onglet du Concepteur de dimensions.  
  
 Pour plus d’informations sur les relations plusieurs-à-plusieurs, consultez [définir de type plusieurs-à-plusieurs et les relations de type plusieurs-à-plusieurs propriétés de relation](../../analysis-services/multidimensional-models/define-a-many-to-many-relationship-and-many-to-many-relationship-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Dimensions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)  
  
  
