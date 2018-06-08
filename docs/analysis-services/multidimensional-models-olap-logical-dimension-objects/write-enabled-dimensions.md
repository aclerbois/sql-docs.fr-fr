---
title: Dimensions activées en écriture | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 23f2fff5a78be0dad52f674a8d23c1922a86391c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="write-enabled-dimensions"></a>Dimensions activées en écriture
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  
  
 Dans une dimension, les données sont généralement en lecture seule. Cependant, dans certains scénarios, vous pouvez activer l'écriture sur une dimension. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], l'activation de l'écriture sur une dimension permet aux utilisateurs de l'entreprise de modifier le contenu de la dimension et de constater l'impact immédiat des modifications sur les hiérarchies de la dimension. Toute dimension basée sur une seule table peut être activée en écriture. Dans une dimension activée en écriture, les utilisateurs et les administrateurs peuvent modifier, déplacer, ajouter et supprimer des membres d'attribut. Ces mises à jour sont collectivement regroupées sous l'appellation d' *écriture différée de dimension*.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge l'écriture différée de dimension dans tous les attributs de dimension, et il est possible de modifier n'importe quel membre d'une dimension. Pour un cube ou une partition activé en écriture, les mises à jour sont stockées dans une table d'écriture différée distincte des tables sources du cube. Toutefois, pour une dimension d'écriture différée, les mises à jour sont enregistrées directement dans la table de la dimension. De plus, si la dimension activée en écriture figure dans un cube à plusieurs partitions dont certaines ou l'ensemble des sources de données possèdent une copie de la table de dimension, seule la table de dimension d'origine est mise à jour lors de l'écriture différée.  
  
 Les dimensions activées en écriture et les cubes activés en écriture ont des fonctionnalités différentes mais complémentaires. Une dimension activée en écriture permet aux utilisateurs de l'entreprise de mettre à jour les membres, tandis qu'un cube activé en écriture leur permet de mettre à jour les valeurs des cellules. Bien que ces deux fonctions soient complémentaires, il n'est pas nécessaire de les utiliser conjointement. Il n'est pas nécessaire d'inclure une dimension dans un cube pour pouvoir utiliser l'écriture différée de dimension. Une dimension activée en écriture peut également figurer dans un cube qui n'est pas activé en écriture. Différentes procédures permettent d'activer en écriture les dimensions et les cubes et de gérer leur sécurité.  
  
 Les restrictions suivantes s'appliquent à l'écriture différée de dimension :  
  
-   Lorsque vous créez un membre, vous devez inclure chaque attribut dans une dimension. Vous ne pouvez pas insérer un membre sans définir la valeur de l'attribut clé de la dimension. Par conséquent, la création de membres est soumise aux contraintes (telles que valeurs de clés non NULL) définies dans la table de dimension.  
  
-   Seuls les schémas en étoile prennent en charge l'écriture différée de dimension. En d'autres termes, une dimension doit reposer sur une seule table directement associée à une table de faits. Après l'activation en écriture d'une dimension, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] valide cette condition lorsque vous effectuez un déploiement vers une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante ou lorsque vous créez un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Vous pouvez modifier ou supprimer un membre existant d'une dimension d'écriture différée. Lorsque vous supprimez un membre, la suppression s'applique à tous les membres enfants. Par exemple, si vous supprimez un pays/région dans une dimension Customer qui contient les attributs CountryRegion, Province, City et Customer, vous supprimez toutes les régions, toutes les villes et tous les clients qui appartiennent au pays/région supprimé. Si le pays/région a une seule province et que vous supprimez la province, vous supprimez également le pays.  
  
 Les membres d'une dimension d'écriture différée ne peuvent être déplacés que dans le même niveau. Par exemple, une ville peut être transférée vers le niveau City dans un autre pays/région ou une autre province, mais pas vers le niveau Province ou CountryRegion. Dans une hiérarchie parent-enfant, tous les membres sont des membres feuilles, et vous pouvez donc transférer un membre vers n'importe quel niveau à l'exception du niveau **(All)** .  
  
 Si un membre d'une hiérarchie parent-enfant est supprimé, les enfants du membre sont déplacés vers le parent du membre. Vous devez disposer d'autorisations de mise à jour sur la table relationnelle du membre supprimé, mais aucune autorisation n'est nécessaire sur les membres déplacés. Lorsqu'une application déplace un membre dans une hiérarchie parent-enfant, l'application peut indiquer dans l'opération UPDATE si les descendants du membre doivent être transférés avec le membre ou transférés vers le parent du membre. Pour supprimer de manière récursive un membre dans une hiérarchie parent-enfant, l'utilisateur doit disposer d'autorisations de mise à jour sur la table relationnelle du membre et de tous les descendants du membre.  
  
> [!NOTE]  
>  Les mises à jour de l'attribut parent d'une hiérarchie parent-enfant ne doivent pas inclure les mises à jour d'autres propriétés ou attributs.  
  
 Toutes les modifications d'une dimension modifient la structure de la dimension. Chaque modification d'une dimension correspond à une transaction nécessitant un traitement incrémentiel pour mettre à jour la structure de la dimension. Les dimensions activées en écriture ont les mêmes conditions de traitement que les autres dimensions.  
  
> [!NOTE]  
>  L'écriture différée de dimension n'est pas prise en charge par les dimensions liées.  
  
## <a name="security"></a>Sécurité  
 Seuls les utilisateurs de l'entreprise qui figurent dans les rôles de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] disposant d'une autorisation de lecture/écriture sur une dimension activée en écriture peuvent mettre à jour la dimension. Pour chaque rôle, vous pouvez déterminer les membres qui peuvent être mis à jour et ceux qui ne le peuvent pas. Pour que les utilisateurs de l'entreprise puissent mettre à jour les dimensions activées en écriture, leur application cliente doit prendre en charge cette fonctionnalité. Pour que cette action puisse être réalisée, la dimension activée en écriture doit figurer dans un cube qui a été traité depuis la dernière modification de la dimension. Pour plus d’informations, consultez [Autorisation de l’accès à des objets et des opérations &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/authorizing-access-to-objects-and-operations-analysis-services.md).  
  
 Les utilisateurs et les groupes inclus dans le rôle des Administrateurs peuvent mettre à jour les membres d'attribut d'une dimension activée en écriture, même si la dimension n'est pas incluse dans un cube.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés de Dimension de base de données](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/database-dimension-properties.md)   
 [Partitions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-write-enabled-partitions.md)   
 [Dimensions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)  
  
  
