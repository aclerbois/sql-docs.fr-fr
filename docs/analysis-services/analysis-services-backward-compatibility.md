---
title: Compatibilité descendante de SQL Server 2016 Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5ab4f304d865992a3269b4ee83c9e25f61069e8c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-backward-compatibility-sql-server-2016"></a>Compatibilité descendante de Analysis Services (SQL Server 2016)
[!INCLUDE[ssas-appliesto-sql2016](../includes/ssas-appliesto-sql2016.md)]

Cet article décrit les modifications de la disponibilité des fonctionnalités et de comportement entre la version actuelle et la version précédente.

## <a name="deprecated-features"></a>Fonctionnalités déconseillées
A *fonctionnalité déconseillée* sera supprimé du produit dans une version ultérieure, mais est toujours pris en charge et incluse dans la version actuelle pour assurer la compatibilité descendante. Il est recommandé de que vous arrêter d’utiliser les fonctionnalités déconseillées dans les projets nouveaux et existants pour assurer la compatibilité avec les versions ultérieures.
  
Les fonctionnalités suivantes sont déconseillées dans cette version :
  
|||  
|-|-|  
|**En mode/une catégorie**|**Fonctionnalité**|  
|(Multidimensionnel)|Partitions distantes|  
|(Multidimensionnel)|Groupes de mesures liés distants|  
|(Multidimensionnel)|Écriture différée dimensionnelle|  
|(Multidimensionnel)|Dimensions liées|   
|(Multidimensionnel)|Notifications de table SQL Server pour la mise en cache proactive.  <br />La solution de remplacement consiste à utiliser l’interrogation pour la mise en cache proactive. <br />Consultez [Mise en cache proactive &#40;dimensions&#41;](../analysis-services/multidimensional-models-olap-logical-dimension-objects/proactive-caching-dimensions.md) et [Mise en cache proactive &#40;partitions&#41;](../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-proactive-caching.md).|  
|(Multidimensionnel)|Cubes de session. Il n’existe aucune solution de remplacement.|  
|(Multidimensionnel)|Cubes locaux. Il n’existe aucune solution de remplacement.|  
|Tabulaire|Les niveaux de compatibilité 1100 et 1103 des modèles tabulaires ne seront pas pris en charge dans une future version. La solution consiste à définir des modèles au niveau de compatibilité 1200 ou supérieur, la conversion des définitions de modèle en métadonnées tabulaires. Consultez [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md).|  
|Outils|SQL Server Profiler pour la capture de traces<br /><br /> La solution consiste à utiliser le Générateur de profils d’événements étendus, intégré dans SQL Server Management Studio.  <br /> Consultez [Surveiller Analysis Services avec des événements étendus SQL Server](../analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events.md).|  
|Outils|Server Profiler pour Trace Replay <br />Remplacement. Il n’existe aucune solution de remplacement.|  
|Objets de gestion de trace et API de trace|Objets Microsoft.AnalysisServices.Trace (contenant les API des objets Analysis Services de trace et de relecture). La solution de remplacement est multiple :<br /><br /> -Configuration de trace : Microsoft.SqlServer.Management.XEvent<br />-Lecture de trace : Microsoft.SqlServer.XEvent.Linq<br />-   Relecture de trace : Aucune|  
  
> [!NOTE]  
>  Les fonctionnalités précédemment annoncées comme déconseillées dans [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] restent en place. Le code prenant en charge ces fonctionnalités n’ayant pas encore été supprimé du produit, bon nombre de celles-ci sont toujours présentes dans cette version. Lors des fonctions précédemment déconseillées peuvent être accessibles, ils sont considérés comme déconseillés et ne peut être physiquement retirées du produit à tout moment.  

## <a name="discontinued-features"></a>Fonctionnalités supprimées
A *abandonné fonctionnalité* a été déconseillée dans une version antérieure. Il peut continuer à être inclus dans la version actuelle, mais n’est plus pris en charge. Fonctionnalités supprimées peuvent être supprimées entièrement dans une future version ou mettre à jour.

Les fonctionnalités suivantes ont été déconseillées dans une version antérieure et ne sont plus prises en charge dans cette version.

|||  
|-|-|  
|**Fonctionnalité**|**Remplacement ou contournement**|  
|[CalculationPassValue &#40;MDX&#41;](../mdx/calculationpassvalue-mdx.md)|Aucun. L’utilisation de cette fonctionnalité a été déconseillée dans SQL Server 2005.|  
|[CalculationCurrentPass &#40;MDX&#41;](../mdx/calculationcurrentpass-mdx.md)|Aucun. L’utilisation de cette fonctionnalité a été déconseillée dans SQL Server 2005.|  
|Indicateur d’optimiseur de requête NON_EMPTY_BEHAVIOR|Aucun. L’utilisation de cette fonctionnalité a été déconseillée dans SQL Server 2008.|  
|Assemblys COM|Aucun. L’utilisation de cette fonctionnalité a été déconseillée dans SQL Server 2008.|  
|Propriété intrinsèque de cellule CELL_EVALUATION_LIST|Aucun. L’utilisation de cette fonctionnalité a été déconseillée dans SQL Server 2005.|  
  
> [!NOTE]  
>  Les fonctionnalités précédemment annoncées comme déconseillées dans [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] restent en place. Le code prenant en charge ces fonctionnalités n’ayant pas encore été supprimé du produit, bon nombre de celles-ci sont toujours présentes dans cette version. Lors des fonctions précédemment déconseillées peuvent être accessibles, ils sont considérés comme déconseillés et ne peut être physiquement retirées du produit à tout moment.  

## <a name="breaking-changes"></a>Modifications avec rupture
Une *modification avec rupture* bloque le fonctionnement d’un modèle de données, d’un code d’application ou d’un script après la mise à niveau du modèle ou du serveur.
  
### <a name="net-40-version-upgrade"></a>Mise à niveau du .NET 4.0  
 Les bibliothèques clientes Analysis Services Management Objects (AMO), ADOMD.NET et le modèle d’objet tabulaire (TOM) maintenant ciblent le runtime .NET 4.0. Il peut s’agir d’une modification majeure pour les applications qui ciblent le .NET 3.5. Les applications utilisant des versions plus récentes de ces assemblys doivent maintenant cibler .NET 4.0 ou ultérieur.  
  
### <a name="amo-version-upgrade"></a>Mise à niveau de la version AMO  
 Cette version est une mise à niveau de version pour [Analysis Services Management Objects &#40;AMO&#41; ](https://msdn.microsoft.com/library/mt436122.aspx) et est une modification avec rupture dans certaines circonstances.  Le code et les scripts qui appellent AMO continueront de s’exécuter comme avant, si vous mettez à niveau une version précédente. Toutefois, si vous avez besoin pour *recompiler* votre application et que vous ciblez une instance de SQL Server 2016 Analysis Services, vous devez ajouter l’espace de noms suivant pour rendre votre code ou votre script opérationnel :  
  
```  
  
using Microsoft.AnalysisServices;  
using Microsoft.AnalysisServices.Core;  
  
```  
  
 L’espace de noms [Microsoft.AnalysisServices.Core](https://msdn.microsoft.com/library/microsoft.analysisservices.core.aspx) est maintenant nécessaire chaque fois que vous faites référence à l’assembly Microsoft.AnalysisServices dans votre code. Les objets qui auparavant ne figuraient que dans l’espace de noms **Microsoft.AnalysisServices** sont déplacés dans l’espace de noms principal dans cette version, si l’objet est utilisé de la même façon dans des scénarios multidimensionnels et tabulaires.  Par exemple, les API liées au serveur sont déplacées dans l’espace de noms principal.  
  
 Bien qu’il y ait plusieurs espaces de noms, deux coexistent dans le même assembly (Microsoft.AnalysisServices.dll).  
  
### <a name="xevent-discover-changes"></a>Modifications de DISCOVER XEvent  
 Pour mieux prendre en charge XEvent découvrir la diffusion en continu dans SSMS pour SQL Server 2016 Analysis Services, `DISCOVER_XEVENT_TRACE_DEFINITION` est remplacé par les traces XEvent suivants :  
  
-   DISCOVER_XEVENT_PACKAGES  
  
-   DISCOVER_XEVENT_OBJECT  
  
-   DISCOVER_XEVENT_OBJECT_COLUMNS  
  
-   DISCOVER_XEVENT_SESSION_TARGETS  

## <a name="behavior-changes"></a>Changements de comportement
Une *modifications de comportement* affecte le mode de fonctionnement ou d’interaction des fonctionnalités de la version actuelle de SQL Server par rapport aux versions précédentes.
  
Une révision des valeurs par défaut, une configuration manuelle requise pour une mise à niveau ou une restauration, ou une nouvelle implémentation d’une fonction existante sont toutes des exemples de modification de comportement dans le produit.
  
Les comportements de fonctionnalité modifiés dans cette version, mais qui restent opérationnels après la mise à niveau d’un modèle ou d’un code, sont répertoriés ici.
  
### <a name="analysis-services-in-sharepoint-mode"></a>Analysis Services en mode SharePoint
 L’exécution de l’Assistant de configuration de PowerPivot en tant que tâche de post-installation n’est plus nécessaire. Cela est vrai pour toutes les versions prises en charge de SharePoint qui chargent des modèles à partir de l’actuelle SQL Server 2016 Analysis Services.
  
### <a name="directquery-mode-for-tabular-models"></a>Mode DirectQuery dans les modèles tabulaires
 *DirectQuery* est un mode d’accès aux données des modèles tabulaires, où la requête s’exécute sur une base de données relationnelle principale, extrayant le jeu de résultats en temps réel. Il est souvent utilisé pour les jeux de données trop volumineux pour la mémoire ou lorsque les données sont volatiles et que vous souhaitez recevoir les données les plus récentes suite aux requêtes exécutées sur un modèle tabulaire.
  
 Dans plusieurs versions précédentes, DirectQuery existait sous la forme d’un mode d’accès aux données. Dans SQL Server 2016 Analysis Services, l’implémentation a été légèrement modifiée, en supposant que le modèle tabulaire est au niveau de compatibilité 1200 ou supérieur. DirectQuery a moins de restrictions qu’auparavant. Il propose également d’autres propriétés de base de données.
  
 Si vous utilisez DirectQuery dans un modèle tabulaire existant, vous pouvez conserver ce dernier à son niveau de compatibilité actuel (1100 ou 1103) et continuer de l’utiliser tel qu’il est mis en œuvre à ces niveaux. Vous pouvez également passer à 1200 ou supérieur à tirer parti des améliorations apportées à DirectQuery.
  
 Il n’existe aucune mise à niveau sur place d’un modèle DirectQuery, car les paramètres à partir de niveaux de compatibilité antérieurs n’ont pas d’équivalents exacts dans les niveaux de compatibilité 1200 et supérieurs plus récente. Si vous avez un modèle tabulaire existant qui s’exécute en mode DirectQuery, vous devez ouvrir le modèle dans SQL Server Data Tools, désactiver DirectQuery, définissez le **le niveau de compatibilité** propriété 1200 ou supérieur, puis reconfigurer les propriétés de DirectQuery. Consultez [DirectQuery Mode](../analysis-services/tabular-models/directquery-mode-ssas-tabular.md) pour plus d’informations.


## <a name="see-also"></a>Voir aussi
[Compatibilité descendante de Analysis Services (2017 de serveur SQL)](analysis-services-backward-compatibility-sql2017.md)
