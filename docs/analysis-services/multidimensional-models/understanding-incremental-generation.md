---
title: Présentation de génération incrémentielle | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 71b3f839326bec0a8b5606e2c7de3f25584b4ff1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="understanding-incremental-generation"></a>Présentation de la génération incrémentielle
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après la génération de schéma initiale, vous pouvez modifier les définitions de cubes et de dimensions à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], puis exécuter à nouveau l'Assistant Génération de schéma. L'Assistant met à jour le schéma dans la base de données de la zone de sujet et dans la vue de source de données associée pour refléter les modifications, en conservant si possible les données qui existent actuellement dans les tables à régénérer. Si vous avez modifié les tables après la génération initiale, l'Assistant Génération de schéma préserve ces modifications si possible en appliquant les règles suivantes :  
  
-   Si une table a été générée antérieurement par l'Assistant, elle est remplacée. Vous pouvez empêcher le remplacement d’une table générée par l’Assistant en affectant à la propriété **AllowChangesDuringGeneration** de cette table dans la vue de source de données la valeur **false**. Lorsque vous prenez le contrôle d'une table, celle-ci est traitée comme toutes les autres tables définies par l'utilisateur et n'est pas concernée par la régénération. Après avoir exclu une table de la génération, vous pouvez ultérieurement affecter à sa propriété **AllowChangesDuringGeneration** dans la vue de source de données la valeur **true** et rouvrir cette table pour des modifications par l’Assistant. Pour plus d’informations, consultez [Modifier les propriétés d’une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/change-properties-in-a-data-source-view-analysis-services.md).  
  
-   Si une table a été ajoutée à la vue de source de données ou à la base de données sous-jacente autrement que par l'intermédiaire de l'Assistant, la table n'est pas remplacée.  
  
 Lorsque l'Assistant Génération de schéma régénère des tables qui ont été générées antérieurement dans la base de données de la zone de sujet, vous pouvez configurer l'Assistant pour qu'il préserve les données de ces tables.  
  
## <a name="supporting-data-preservation"></a>Prise en charge de la préservation des données  
 En règle générale, l'Assistant Génération de schéma préserve les données stockées dans les tables qu'il a générées. De plus, si vous ajoutez des colonnes à des tables que l'Assistant a générées, celui-ci préserve aussi ces données. Vous pouvez tirer parti de ce comportement pour ajouter ou modifier vos dimensions et vos cubes, puis régénérer les objets sous-jacents sans avoir à recharger les données stockées dans les tables sous-jacentes.  
  
> [!NOTE]  
>  Si vous chargez des données à partir de fichiers texte délimités, vous pouvez aussi décider si l'Assistant Génération de schéma remplacera ces fichiers et les données qu'ils contiennent durant la régénération. Les fichiers texte sont remplacés complètement ou ne sont pas remplacés du tout. L'Assistant Génération de schéma ne remplace pas ces fichiers partiellement. Par défaut, ces fichiers ne sont pas remplacés.  
  
### <a name="partial-preservation"></a>Préservation partielle  
 Dans certaines circonstances, l'Assistant Génération de schéma ne peut pas préserver les données existantes. Le tableau suivant fournit des exemples de situations où l'Assistant ne peut pas préserver toutes les données des tables sous-jacentes durant la régénération.  
  
|Type de modification des données|Traitement|  
|-------------------------|---------------|  
|Changement de type de données incompatible|Dans la mesure du possible, l’Assistant Génération de schéma utilise les conversions de type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] standard pour convertir les données existantes d’un type à l’autre. Toutefois, si vous changez le type de données d'un attribut en lui affectant un type incompatible avec les données existantes, l'Assistant supprime les données de la colonne concernée.|  
|Erreurs d'intégrité référentielle|Si une modification que vous apportez à une dimension ou un cube contenant des données provoque une erreur d'intégrité référentielle durant la régénération, l'Assistant Génération de schéma supprime toutes les données de la table de clé étrangère. Les données supprimées ne se limitent pas à la colonne qui a causé la violation de la contrainte de clé étrangère, ni aux lignes qui contiennent les erreurs d'intégrité référentielle. Par exemple, si vous changez d'attribut clé de dimension pour adopter un attribut ayant des données non uniques ou NULL, toutes les données de la table de clé étrangère sont supprimées. En outre, la suppression de toutes les données d'une table peut avoir des répercussions en cascade et provoquer d'autres violations de l'intégrité référentielle.|  
|Suppression d'attribut ou de dimension|Si vous supprimez un attribut d'une dimension, l'Assistant Génération de schéma supprime la colonne qui est mappée avec l'attribut supprimé. Si vous supprimez une dimension, l'Assistant supprime la table qui est mappée avec la dimension supprimée. En pareils cas, l'Assistant supprime les données contenues dans la colonne ou la table supprimée.|  
  
 Comme l'Assistant Génération de schéma émet un avertissement avant de supprimer des données, vous pouvez l'annuler sans perdre de données. Toutefois, l'Assistant Génération de schéma est incapable de faire la différence entre une perte de données anticipée et une perte de données imprévue. Lorsque vous exécutez l'Assistant, une boîte de dialogue vous présente les tables et les colonnes contenant des données qui seront supprimées. Vous pouvez alors soit continuer et laisser l'Assistant supprimer les données, soit annuler l'Assistant et corriger les modifications que vous avez apportées à ces tables et ces colonnes.  
  
## <a name="supporting-cube-and-dimension-changes"></a>Prise en charge des modifications de cubes et de dimensions  
 Lorsque vous modifiez les propriétés de dimensions et de cubes, l'Assistant Génération de schéma régénère les objets appropriés dans la base de données de la zone de sujet et dans la vue de source de données associée comme l'explique le tableau suivant.  
  
 Suppression d'un objet comme une dimension, un cube ou un attribut.  
 L'Assistant Génération de schéma supprime les objets sous-jacents avec lesquels l'objet supprimé est mappé. Si vous ajoutez des colonnes à une table que l'Assistant a générée, les nouvelles colonnes n'empêchent pas la suppression de la table. La suppression d'un objet entraîne la suppression des données stockées dans les objets sous-jacents, mais peut aussi causer la suppression d'autres données si une erreur d'intégrité référentielle se produit.  
  
 Attribution d'un nouveau nom à un objet comme une dimension, un cube ou un attribut.  
 L'Assistant Génération de schéma renomme les objets sous-jacents avec lesquels l'objet renommé est mappé. L'Assistant renomme également tous les objets concernés, par exemple les clés primaires. Les données existantes stockées dans les objets sous-jacents sont préservées.  
  
 Modification d'un objet, par exemple en changeant son type de données.  
 L'Assistant Génération de schéma modifie les objets sous-jacents avec lesquels l'objet modifié est mappé. Les données existantes stockées dans les objets de base de données sous-jacents sont préservées, sauf si le nouveau type de données est incompatible avec les données existantes.  
  
 Ajout d'un nouvel objet, comme une dimension, un cube ou un attribut.  
 L'Assistant Génération de schéma ajoute les objets sous-jacents avec lesquels le nouvel objet est mappé.  
  
 Si l'Assistant Génération de schéma ne peut pas faire la modification nécessaire à cause de la présence d'un objet utilisateur dans la base de données de la zone de sujet (parce que le moteur de base de données retourne une erreur), l'Assistant Génération de schéma échoue et affiche l'erreur retournée par le moteur de base de données. Par exemple, si vous créez une contrainte de clé primaire ou un index non-cluster sur une table après sa génération par l'Assistant Génération de schéma, celui-ci ne supprime pas cette table parce qu'il n'a pas créé la contrainte ou l'index.  
  
## <a name="supporting-schema-changes"></a>Prise en charge des modifications de schéma  
 Lorsque vous modifiez les propriétés des tables ou des colonnes dans la base de données de la zone de sujet ou dans la vue de source de données associée, l'Assistant Génération de schéma traite les modifications comme le décrit le tableau suivant.  
  
 Suppression d'une table ou d'une colonne générée par l'Assistant Génération de schéma.  
 Si vous supprimez une table ou une colonne générée par l'Assistant Génération de schéma, celui-ci régénère la table ou la colonne supprimée. L'Assistant n'avertit pas que la table ou la colonne supprimée sera régénérée.  
  
 Modification des propriétés d'une table ou d'une colonne générée par l'Assistant Génération de schéma.  
 Si vous modifiez les propriétés d'une table ou d'une colonne générée par l'Assistant Génération de schéma, celui-ci régénère la table ou la colonne modifiée sans la modification. Par exemple, si vous changez le type de données d'une colonne ou la possibilité d'y stocker la valeur NULL, ou encore le groupe de fichiers d'une table générée par l'Assistant Génération de schéma, la modification ne survivra pas à la régénération. L'Assistant n'avertit pas que l'objet modifié sera régénéré sans la modification.  
  
 Ajout d'une colonne à une table générée par l'Assistant Génération de schéma ou ajout d'une table à la base de données de la zone de sujet ou de transit.  
 Si vous ajoutez une colonne à une table générée par l'Assistant Génération de schéma, celui-ci préserve la colonne supplémentaire, ainsi que les données qui y sont stockées, durant la régénération. Cependant, si vous ajoutez une table à la base de données de la zone de sujet ou à la base de données de la zone de transit, l'Assistant Génération de schéma n'incorpore pas la nouvelle table. La colonne ou la table ajoutée n'est pas reflétée dans le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , les packages DTS, la vue de source de données ou tout autre emplacement dans le schéma qui est généré.  
  
## <a name="supporting-data-source-and-data-source-view-changes"></a>Prise en charge des modifications de source de données ou de vue de source de données  
 Lorsque l'Assistant Génération de schéma est exécuté à nouveau, il réutilise la même source de données et la même vue de source de données que lors de la génération initiale. Si vous ajoutez une source de données ou une vue de source de données, l'Assistant ne l'utilise pas. Si vous supprimez la source de données ou la vue de source de données d'origine après la génération initiale, vous devez exécuter l'Assistant depuis le début. Toutes les options précédemment définies dans l'Assistant sont également supprimées. Tous les objets existants d'une base de données sous-jacente qui étaient liés à une source de données ou une vue de source de données supprimée sont traités comme des objets créés par l'utilisateur lorsque vous exécutez l'Assistant Génération de schéma la fois suivante.  
  
 Si la vue de source de données ne reflète pas l'état réel de la base de données sous-jacente au moment de la génération, l'Assistant Génération de schéma risque de rencontrer des erreurs en générant les schémas pour la base de données de la zone de sujet et la base de données de la zone de transit. Par exemple, si la vue de source de données spécifie qu’une colonne a le type de données **int**, alors qu’il s’agit en fait du type **string**, l’Assistant Génération de schéma définit le type de données **int** pour la clé étrangère afin de la faire correspondre à la vue de source de données, puis il ne parvient pas à créer la relation car le véritable type de données de cette colonne est **string**.  
  
 D'un autre côté, si vous modifiez la chaîne de connexion à la source de données en spécifiant une base de données différente de celle de la génération précédente, aucune erreur ne se produit. La nouvelle base de données est utilisée, et aucune modification n'est apportée à la base de données précédente.  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer les modifications apportées aux vues de sources de données et Sources de données](../../analysis-services/multidimensional-models/manage-changes-to-data-source-views-and-data-sources.md)   
 [Assistant génération de schéma & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/schema-generation-wizard-analysis-services.md)  
  
  
