---
title: Cubes locaux (Analysis Services - données multidimensionnelles) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 11c4a7b2917d7baa4860137fa0764026264024a8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="local-cubes-analysis-services---multidimensional-data"></a>Cubes locaux (Analysis Services - Données multidimensionnelles)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Pour créer, mettre à jour ou supprimer des cubes locaux, vous devez écrire et exécuter un script ASSL ou un programme AMO.  
  
 Les cubes locaux et les modèles d'exploration de données locaux permettent d'analyser une station de travail cliente lorsqu'elle est déconnectée du réseau. Par exemple, une application cliente peut appeler le fournisseur OLE DB pour OLAP 9.0 (MSOLAP.3), qui à son tour charge le moteur de cube local pour créer et interroger des cubes locaux, comme l'indique l'illustration suivante.  
  
 ![Architecture de client pour les cubes locaux et les modèles](../../../analysis-services/multidimensional-models/olap-physical/media/as-localcubearch9.gif "architecture du Client pour les cubes locaux et des modèles")  
  
 ADMOD.NET et AMO (Analysis Management Objects) chargent également le moteur de cube local lors de l'interaction avec des cubes locaux. Un seul processus peut accéder à un fichier de cube local, car le moteur de cube local verrouille exclusivement un fichier de cube local lorsqu'il établit une connexion au cube local. Avec un processus, jusqu'à cinq connexions simultanées sont autorisées.  
  
 Un fichier .cub peut contenir plusieurs cubes ou modèles d'exploration de données. Les interrogations de cubes et modèles d'exploration de données locaux sont traitées par le moteur de cube local et ne nécessitent pas de connexion à une instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  L'utilisation de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] pour gérer les cubes locaux n'est pas prise en charge.  
  
## <a name="local-cubes"></a>Cubes locaux  
 Un cube local peut être créé et rempli à partir d’un cube d’existant dans un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance ou d’une source de données relationnelles.  
  
|Source de données pour un cube local|Méthode de création|  
|------------------------------------|---------------------|  
|Cube serveur|Vous pouvez utiliser l’instruction CREATE GLOBAL CUBE ou un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] script ASSL (Scripting Language) pour créer et remplir un cube à partir d’un cube basé sur le serveur. Pour plus d’informations, consultez [CREATE GLOBAL CUBE Statement &#40;MDX&#41; ](../../../mdx/mdx-data-definition-create-global-cube.md) ou [Analysis Services Scripting Language &#40;ASSL de XMLA&#41;](../../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md).|  
|Source de données relationnelles|Vous utilisez un script ASSL pour créer et remplir un cube à partir d'une base de données relationnelle OLE DB. Pour créer un cube local via ASSL, il vous suffit de vous connecter à un fichier de cube local (*.cub) et d'exécuter le script ASSL de la même manière que vous exécutez un script ASSL sur une instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] pour créer un cube serveur. Pour plus d’informations, consultez [Référence Analysis Services Scripting Language &#40;ASSL&#41;](../../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md).|  
  
 Utilisez l'instruction REFRESH CUBE pour reconstruire un cube local et mettre à jour ses données. Pour plus d’informations, consultez [REFRESH CUBE Statement &#40;MDX&#41;](../../../mdx/mdx-data-definition-refresh-cube.md).  
  
### <a name="local-cubes-created-from-server-based-cubes"></a>Cubes locaux créés à partir de cubes serveur  
 Lorsque vous concevez des cubes locaux créés à partir de cubes serveur, vous devez prendre en compte les points suivants :  
  
-   Les mesures de comptage de valeurs ne sont pas prises en charge.  
  
-   Lorsque vous ajoutez une mesure, vous devez également inclure au moins une dimension qui est liée à la mesure ajoutée. Pour plus d’informations sur les relations de dimension aux groupes de mesures, consultez [relations de Dimension](../../../analysis-services/multidimensional-models-olap-logical-cube-objects/dimension-relationships.md).  
  
-   Lorsque vous ajoutez une hiérarchie parent-enfant, les niveaux et filtres d'une hiérarchie parent-enfant sont ignorés, et l'intégralité de la hiérarchie parent-enfant est incluse.  
  
-   Aucune propriété de membre n'est créée.  
  
-   Lorsque vous incluez une mesure semi-additive, aucune coupe n'est autorisée sur la dimension Account ou Time.  
  
-   Les dimensions de référence sont toujours matérialisées.  
  
-   Lorsque vous insérez une dimension plusieurs à plusieurs, les règles suivantes s'appliquent :  
  
    -   Vous ne pouvez pas découper la dimension plusieurs à plusieurs.  
  
    -   Vous devez ajouter une mesure à partir du groupe de mesures intermédiaire.  
  
    -   Vous ne pouvez pas découper les dimensions communes aux deux groupes de mesures impliqués dans la relation plusieurs à plusieurs.  
  
-   Seuls les membres calculés, jeux nommés et affectations qui reposent sur des mesures et des dimensions ajoutées au cube local apparaîtront dans le cube local. Les membres calculés, jeux nommés et affectations non valides seront automatiquement exclus.  
  
### <a name="security"></a>Sécurité  
 Dans l’ordre pour un utilisateur de créer un cube local à partir d’un cube serveur, l’utilisateur doit pouvoir **extraction et Cube Local** autorisations sur le cube serveur. Pour plus d’informations, consultez [accorder des autorisations de cube ou modèle &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md).  
  
 Les cubes locaux ne sont pas sécurisés à l'aide de rôles comme les cubes serveur. Toute personne bénéficiant d'un accès au niveau fichier à un fichier de cube local peut interroger les cubes contenus dans ce dernier. Vous pouvez utiliser la **mot de passe de chiffrement** propriété de connexion d’un fichier de cube local pour définir un mot de passe sur le fichier de cube local. La définition d'un mot de passe sur un fichier de cube local nécessite que toutes les connexions futures au fichier de cube local utilisent ce mot de passe afin d'interroger le fichier.  
  
## <a name="see-also"></a>Voir aussi  
 [Instruction CREATE GLOBAL CUBE &#40;MDX&#41;](../../../mdx/mdx-data-definition-create-global-cube.md)   
 [Développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)   
 [REFRESH CUBE Statement &#40;MDX&#41;](../../../mdx/mdx-data-definition-refresh-cube.md)  
  
  
