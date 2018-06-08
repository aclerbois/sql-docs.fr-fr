---
title: Comprendre les schémas de base de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 91a54be06727a674a16f12295fa886f869b188e4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="understanding-the-database-schemas"></a>Présentation des schémas de base de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'Assistant Génération de schéma crée pour la base de données de la zone de sujet un schéma relationnel dénormalisé basé sur les dimensions et les groupes de mesures définis dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. L'Assistant génère pour chaque dimension une table relationnelle appelée table de dimension, destinée à stocker les données de dimension, et pour chaque groupe de mesures une table relationnelle appelée table de faits, servant à stocker les données de faits. Lorsqu'il génère ces tables relationnelles, l'Assistant ignore les dimensions liées, les groupes de mesures liés et les dimensions de temps de serveur.  
  
## <a name="validation"></a>Validation  
 Avant de commencer à générer le schéma relationnel sous-jacent, l'Assistant Génération de schéma valide les cubes et les dimensions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Si l'Assistant détecte des erreurs, il s'arrête et signale ces erreurs dans la fenêtre Liste des tâches de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Voici quelques exemples d'erreurs qui empêchent la génération :  
  
-   Les dimensions qui ont plus d'un attribut clé.  
  
-   Les attributs parents qui ont des types de données différents de ceux des attributs clés.  
  
-   Les groupes de mesures dépourvus de mesures.  
  
-   Les dimensions dégénérées ou les mesures mal configurées.  
  
-   Les clés de substitution mal configurées, par exemple plusieurs attributs utilisant le type d'attribut **ScdOriginalID** ou un attribut utilisant le type **ScdOriginalID** sans être lié à une colonne utilisant le type de données Integer.  
  
## <a name="dimension-tables"></a>Tables de dimension  
 Pour chaque dimension, l'Assistant Génération de schéma crée une table de dimension à inclure dans la base de données de la zone de sujet. La structure de la table de dimension dépend des choix effectués lors de la conception de la dimension sur laquelle elle est basée.  
  
 Columns  
 L'Assistant génère une colonne pour les liaisons associées à chaque attribut de la dimension sur laquelle la table de dimension est basée, par exemple les liaisons pour les propriétés **KeyColumns**, **NameColumn**, **ValueColumn**, **CustomRollupColumn**, **CustomRollupPropertiesColumn**et **UnaryOperatorColumn** de chaque attribut.  
  
 Relations  
 L'Assistant génère une relation entre la colonne de chaque attribut parent et la clé primaire de la table de dimension.  
  
 L'Assistant crée également, s'il y a lieu, une relation à la clé primaire de chaque table de dimension supplémentaire définie comme dimension référencée du cube.  
  
 Contraintes  
 L'Assistant génère par défaut pour chaque table de dimension une contrainte de clé primaire basée sur l'attribut clé de la dimension. Si la contrainte de clé primaire est générée, une colonne de nom séparée est créée par défaut. Une clé primaire logique est créée dans la vue de source de données même si vous décidez de ne pas créer la clé primaire dans la base de données.  
  
> [!NOTE]  
>  Une erreur se produit si plusieurs attributs clés sont spécifiés dans la dimension sur laquelle la table de dimension est basée.  
  
 Translations  
 L'Assistant génère une table séparée pour stocker les valeurs traduites de tout attribut qui nécessite une colonne de traduction. L'Assistant crée également une colonne séparée pour chacune des langues requises.  
  
## <a name="fact-tables"></a>Tables de faits  
 Pour chaque groupe de mesures d'un cube, l'Assistant Génération de schéma crée une table de faits à inclure dans la base de données de la zone de sujet. La structure de la table de faits dépend des choix effectués lors de la conception du groupe de mesures sur lequel elle est basée et des relations établies entre le groupe de mesures et les dimensions incluses.  
  
 Columns  
 L’Assistant génère une colonne pour chaque mesure, sauf pour les mesures qui utilisent la fonction d'agrégation **Count** . En effet, de telles mesures ne nécessitent pas de colonne correspondante dans la table de faits.  
  
 L'Assistant génère également une colonne pour chaque colonne d'attribut de granularité de chaque relation de dimension régulière du groupe de mesures et, s'il y a lieu, une ou plusieurs colonnes pour les liaisons associées à chaque attribut d'une dimension qui entretient une relation de dimension de fait avec le groupe de mesures sur lequel cette table est basée.  
  
 Relations  
 L'Assistant génère une relation pour chaque relation de dimension régulière de la table de faits à l'attribut de granularité de la table de la dimension. Si la granularité est basée sur l'attribut clé de la table de dimension, la relation est créée dans la base de données et dans la vue de source de données. Si la granularité est basée sur un autre attribut, la relation n'est créée que dans la vue de source de données.  
  
 Si vous choisissez de générer les index dans l'Assistant, un index non-cluster est créé pour chacune de ces colonnes de relation.  
  
 Contraintes  
 Les clés primaires ne sont pas générées sur les tables de faits.  
  
 Si vous choisissez d'appliquer l'intégrité référentielle, des contraintes d'intégrité référentielle sont générées, le cas échéant, entre les tables de dimension et les tables de faits.  
  
 Translations  
 L'Assistant génère une table séparée pour stocker les valeurs traduites de toute propriété du groupe de mesures qui nécessite une colonne de traduction. L'Assistant crée également une colonne séparée pour chacune des langues requises.  
  
## <a name="data-type-conversion-and-default-lengths"></a>Conversion de type de données et longueurs par défaut  
 L’Assistant Génération de schéma ignore les types de données dans tous les cas, sauf pour les colonnes utilisant le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **wchar** . Le type de données **wchar** se traduit directement en type de données **nvarchar** . Toutefois, si la longueur spécifiée d’une colonne utilisant le type **wchar** dépasse 4000 octets, l’Assistant Génération de schéma produit une erreur.  
  
 Le tableau suivant indique la longueur par défaut de la colonne si un élément de données, par exemple la liaison d'un attribut, n'a pas de longueur spécifiée.  
  
|Élément de données|Longueur par défaut (en octets)|  
|---------------|------------------------------|  
|KeyColumn|50|  
|NameColumn|50|  
|CustomRollupColumn|3000|  
|CustomRollupPropertiesColumn|500|  
|UnaryOperatorColumn|1|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnement de la génération incrémentielle](../../analysis-services/multidimensional-models/understanding-incremental-generation.md)   
 [Gérer les modifications apportées aux vues de sources de données et Sources de données](../../analysis-services/multidimensional-models/manage-changes-to-data-source-views-and-data-sources.md)  
  
  
