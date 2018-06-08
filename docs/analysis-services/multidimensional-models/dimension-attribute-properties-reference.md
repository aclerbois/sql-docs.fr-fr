---
title: Référence de propriétés d’attribut de dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 26975562f3617742cbcc3bfb3a47e41af09ba8b3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimension-attribute-properties-reference"></a>Référence des propriétés d’attribut de dimension
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], il y a de nombreuses propriétés qui déterminent le fonctionnement des dimensions et des attributs de dimension. Le tableau suivant répertorie et décrit chacune de ces propriétés d'attribut.  
  
|Propriété| Description|  
|--------------|-----------------|  
|**AttributeHierarchyDisplayFolder**|Identifie le dossier où afficher aux utilisateurs finaux la hiérarchie d'attribut associée.|  
|**AttributeHierarchyEnabled**|Détermine si une hiérarchie d’attribut est générée par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour l’attribut. Si la hiérarchie d'attribut n'est pas activée, l'attribut ne peut pas être utilisé dans une hiérarchie définie par l'utilisateur et la hiérarchie d'attribut ne peut pas être référencée dans des instructions MDX (Multidimensional Expressions).|  
|**AttributeHierarchyOptimizedState**|Détermine le niveau d'optimisation appliqué à la hiérarchie d'attribut. Par défaut, une hiérarchie d’attribut est **FullyOptimized**, ce qui signifie que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère des index pour la hiérarchie d’attribut de façon à améliorer les performances des requêtes. L’autre option, **NotOptimized**, signifie qu’aucun index n’est créé pour la hiérarchie d’attribut. **NotOptimized** est utile si la hiérarchie d’attribut est utilisée à des fins autres que l’interrogation, car aucun index supplémentaire n’est généré pour l’attribut. D'autres utilisations pour une hiérarchie d'attribut peuvent aider à classer un autre attribut.|  
|**AttributeHierarchyOrdered**|Détermine si la hiérarchie d'attribut associée est ordonnée. La valeur par défaut est **True**. Toutefois, si vous savez qu’une hiérarchie d’attribut ne sera pas utilisée pour des requêtes, vous pouvez gagner du temps de traitement en affectant la valeur **False**à cette propriété.|  
|**AttributeHierarchyVisible**|Détermine si la hiérarchie d'attribut est visible par des applications clientes. La valeur par défaut est **True**. Toutefois, si vous savez qu’une hiérarchie d’attribut ne sera pas utilisée pour des requêtes, vous pouvez gagner du temps de traitement en affectant la valeur **False**à cette propriété.|  
|**CustomRollupColumn**|Spécifie la colonne qui définit une formule de cumul personnalisée.|  
|**CustomRollupPropertiesColumn**|Spécifie la colonne qui contient les propriétés d'une formule de cumul personnalisée.|  
|**DefaultMember**|Spécifie une expression MDX (Multidimensional Expressions) qui définit la mesure par défaut de l'attribut.|  
|**Description**|Contient la description de l'attribut.|  
|**DiscretizationBucketCount**|Contient le nombre de compartiments où effectuer la discrétisation.|  
|**DiscretizationMethod**|Définit la méthode à utiliser pour la discrétisation.|  
|**EstimatedCount**|Spécifie le nombre estimé de membres dans l'attribut. Jusqu'à l'exécution de l'Assistant Conception d'agrégation, la valeur par défaut est zéro. Vous pouvez autoriser l'Assistant à compter le nombre d'enregistrements ou bien entrer une valeur estimée. Entrez une valeur manuellement si vous connaissez le nombre de membres et que vous voulez gagner le temps qui est nécessaire pour effectuer une requête de comptage sur la base de données. Si vous utilisez un sous-ensemble de test de vos données de production, vous pouvez utiliser les chiffres de vos données de production de façon que la conception de l'agrégation soit optimisée pour les données de production plutôt que pour les données de test.|  
|**GroupingBehavior**|Valeur définie par l'utilisateur qui fournit un indicateur aux applications clientes sur le regroupement d'attributs.|  
|**ID**|Contient l'identificateur unique (ID) de la dimension.|  
|**InstanceSelection**|Fournit un indicateur aux applications clientes sur le mode d'affichage d'une liste d'éléments en fonction du nombre d'éléments attendus dans la liste. Les options disponibles sont les suivantes :<br /><br /> **None** Aucun indicateur n’est fourni à l’application cliente. Ceci est la valeur par défaut.<br /><br /> **DropDown** Le nombre d’éléments est suffisamment faible pour qu’ils soient affichés dans une liste déroulante.<br /><br /> **List** Le nombre d’éléments est trop important pour une **liste**déroulante, mais ne nécessite pas de filtrage.<br /><br /> **FilteredList** Le nombre d’éléments est suffisamment important pour demander aux utilisateurs de filtrer les éléments à afficher.<br /><br /> **MandatoryFilter** Le nombre d’éléments est tellement important que l’affichage doit toujours être filtré.|  
|**IsAggregatable**|Spécifie si les valeurs des membres d'attribut peuvent être agrégées. La valeur par défaut est **True**, ce qui signifie que la hiérarchie d’attribut contient un niveau (Tous). Si la valeur de cette propriété est **False**, la hiérarchie d’attribut ne contient pas un niveau (Tous).|  
|**KeyColumns**|Contient la ou les colonnes qui représentent la clé pour l'attribut, qui est la colonne de la table relationnelle sous-jacente de la vue de source de données à laquelle l'attribut est lié. La valeur de cette colonne pour chaque membre est affichée pour les utilisateurs, sauf si une valeur est spécifiée pour la propriété **NameColumn** .|  
|**MemberNamesUnique**|Détermine si les noms des membres de la hiérarchie d'attribut doivent être uniques.|  
|**MembersWithData**|Utilisé par les attributs parents pour déterminer si les membres de données pour les membres non-feuilles de l'attribut parent sont affichés ou non. Cette valeur de propriété est utilisée uniquement quand la valeur Parent est affectée à la propriété **Usage** . Cela signifie qu'une hiérarchie parent-enfant a été définie. Les options disponibles sont les suivantes :<br /><br /> **NonLeafDataHidden** Les données non-feuilles sont masquées.<br /><br /> **NonLeafDataVisible** Les données non-feuilles sont visibles.|  
|**MembersWithDataCaption**|Fournit une chaîne modèle qui est utilisée par des attributs parents pour créer des légendes pour les membres de données générés par le système dans l'attribut parent. Cette valeur de propriété est utilisée uniquement quand la valeur Parent est affectée à la propriété **Usage** . Cela signifie qu'une hiérarchie parent-enfant a été définie.|  
|**Nom**|Contient le nom convivial de l’attribut.|  
|**NameColumn**|Identifie la colonne qui fournit le nom de l'attribut qui est affiché aux utilisateurs, plutôt que la valeur de la colonne clé pour l'attribut. Cette colonne est utilisée lorsque la valeur de la colonne clé pour un membre d'attribut n'est pas très claire ou non utile à l'utilisateur, ou bien quand la colonne clé est basée sur une clé composite. La propriété **NameColumn** n’est pas utilisée dans les hiérarchies parent-enfant ; à la place, la propriété **NameColumn** pour les membres enfants est utilisée comme noms de membre d’une hiérarchie parent-enfant.|  
|**NamingTemplate**|Définit comment les niveaux sont appelés dans une hiérarchie parent-enfant construite à partir de l'attribut parent. Cette valeur de propriété est utilisée uniquement quand la valeur Parent est affectée à la propriété **Usage** . Cela signifie qu'une hiérarchie parent-enfant a été définie.|  
|**OrderBy**|Décrit comment classer les membres qui sont contenus dans la hiérarchie d'attribut. La valeur par défaut est Nom, qui spécifie que le classement des membres d’attribut est basé sur la valeur de la propriété **NameColumn** , le cas échéant. Sinon, les membres sont classés par la valeur de la colonne clé. Les options disponibles sont les suivantes :<br /><br /> **NameColumn** Classement par la valeur de la propriété **NameColumn** .<br /><br /> **Key** Classement par valeur de la colonne clé du membre d’attribut.<br /><br /> **AttributeKey** Classement par valeur de la clé de membre d’un attribut spécifié, qui doit avoir une relation d’attribut avec l’attribut.<br /><br /> **AttributeName** Classement par valeur du nom de membre d’un attribut spécifié, qui doit avoir une relation d’attribut avec l’attribut.|  
|**OrderByAttribute**|Identifie l'attribut selon lequel les membres de la hiérarchie d'attribut doivent être classés.|  
|**RootMemberIf**|Détermine le mode d'identification des membres racines ou du plus haut niveau d'une hiérarchie parent-enfant. Cette valeur de propriété est utilisée uniquement quand la valeur Parent est affectée à la propriété **Usage** . Cela signifie qu'une hiérarchie parent-enfant a été définie. La valeur par défaut étant **ParentIsBlankSelfOrMissing**, seuls les membres qui satisfont à une ou plusieurs des conditions décrites pour **ParentIsBlank**, **ParentIsSelf**ou **ParentIsMissing** sont traités en tant que membres racines. Les valeurs suivantes sont aussi disponibles :<br /><br /> **ParentIsBlank** Seuls les membres avec une valeur NULL, une valeur zéro ou une chaîne vide dans les colonnes clés sont traités en tant que membres racines.<br /><br /> **ParentIsSelf** Seuls les membres qui sont parents d’eux-mêmes sont traités en tant que membres racines.<br /><br /> **ParentIsMissing** Seuls les membres dont les parents sont introuvables sont traités en tant que membres racines.|  
|**Type**|Contient le type de l'attribut. Pour plus d’informations, consultez [Configurer des types d’attributs](../../analysis-services/multidimensional-models/attribute-properties-configure-attribute-types.md).|  
|**UnaryOperatorColumn**|Spécifie la colonne qui fournit des opérateurs unaires. Il s'agit d'une liaison de type DataItem qui définit les détails d'une colonne fournissant un opérateur unaire.|  
|**Usage**|Décrit le mode d'utilisation d'un attribut.<br /><br /> Les options disponibles sont les suivantes :<br /><br /> **Regular** L’attribut est un attribut régulier. Ceci est la valeur par défaut.<br /><br /> **Key** L’attribut est un attribut de clé.<br /><br /> **Parent** L’attribut est un attribut parent.|  
|**ValueColumn**|Identifie la colonne qui fournit la valeur de l'attribut. Si l’élément **NameColumn** de l’attribut est spécifié, les mêmes valeurs de **DataItem** sont utilisées en tant que valeurs par défaut pour l’élément **ValueColumn** . Si l’élément **NameColumn** de l’attribut n’est pas spécifié et que la collection **KeyColumns** de l’attribut contient un seul élément **KeyColumn** représentant une colonne clé avec un type de données chaîne, les mêmes valeurs de **DataItem** sont utilisées en tant que valeurs par défaut pour l’élément **ValueColumn** .|  
  
> [!NOTE]  
>  Pour plus d’informations sur la définition des valeurs de la propriété **KeyColumn** au moment de l’utilisation de valeurs Null et pour obtenir des informations sur d’autres problèmes d’intégrité des données, consultez [Gestion des problèmes d’intégrité de données dans Analysis Services 2005](http://go.microsoft.com/fwlink/?LinkId=81891).  
  
> [!NOTE]  
>  Le membre par défaut sur un attribut sert à évaluer les expressions lorsqu'un membre de la hiérarchie n'est pas explicitement inclus dans une requête. Le membre par défaut d’un attribut est spécifié par la propriété **DefaultMember** de l’attribut. Lorsqu'une hiérarchie de dimension est incluse dans une requête, tous les membres par défaut des attributs correspondant aux niveaux de la hiérarchie sont ignorés. Si aucune hiérarchie de dimension n'est incluse dans une requête, les membres par défaut sont alors utilisés pour tous les attributs de la dimension. Pour plus d’informations sur les membres par défaut, consultez [Définir un membre par défaut](../../analysis-services/multidimensional-models/attribute-properties-define-a-default-member.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)  
  
  
