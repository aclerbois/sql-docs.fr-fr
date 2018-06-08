---
title: Partitions activées en écriture | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f5bca17d40456d55eb84c6699011547b7f399c05
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="partitions---write-enabled-partitions"></a>Partitions - Partitions activées en écriture
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans un cube, les données sont généralement en lecture seule. Cependant, dans certains scénarios, vous pouvez activer l'écriture sur une partition. Les partitions activées en écriture permettent aux utilisateurs professionnels d'explorer différents scénarios en changeant les valeurs des cellules et en analysant les effets de ces modifications sur les données de cube. Si vous activez une partition en écriture, les applications clientes peuvent enregistrer les modifications des données de la partition. Ces modifications, qu'il est convenu d'appeler données d'écriture différée, sont stockées dans une table séparée et ne remplacent pas de données existantes d'un groupe de mesures. Toutefois, elles sont incorporées dans les résultats des requêtes comme si elles faisaient partie des données de cube.  
  
 Vous pouvez activer en écriture la totalité d'un cube ou uniquement certaines de ces partitions. Les dimensions activées en écriture sont différentes mais complémentaires. Une partition activée en écriture permet aux utilisateurs de mettre à jour les cellules de partition, tandis qu'une dimension activée en écriture leur permet de mettre à jour les membres de dimension. Vous pouvez également utiliser ces deux fonctionnalités conjointement. Par exemple, un cube activé en écriture ou une partition activée en écriture ne doit pas inclure les dimensions activées en écriture. **Rubrique connexe :**[Write Dimensions](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md).  
  
> [!NOTE]  
>  Si vous souhaitez activer en écriture un cube dont la source de données est une base de données Microsoft Access, n'utilisez pas le fournisseur Microsoft OLE DB pour pilotes ODBC dans les définitions de la source de données du cube, ses partitions ou ses dimensions. Utilisez à la place Microsoft Jet 4.0 OLE DB Provider ou n'importe quelle version du Jet Service Pack qui comprend Jet 4.0 OLE. Pour plus d’informations, consultez l’article de la Base de connaissances Microsoft [comment obtenir le dernier service pack pour le moteur de base de données Microsoft Jet 4.0](http://support.microsoft.com/?kbid=239114).  
  
 Un cube peut être activée en écriture uniquement si toutes ses mesures utilisent la **somme** fonction d’agrégation. Les groupes de mesures liés et les cubes locaux ne peuvent pas être activés en écriture.  
  
## <a name="writeback-storage"></a>Mode de stockage des données d'écriture différée  
 Toute modification apportée par l'utilisateur professionnel est stockée dans la table d'écriture différée sous la forme de différence par rapport à la valeur actuellement affichée. Par exemple, si un utilisateur final modifie une valeur de cellule de 90 à 100, la valeur **+ 10** est stocké dans la table d’écriture différée, ainsi que l’heure de la modification et les informations relatives à l’utilisateur de l’entreprise qui l’a effectuée. L'effet net du cumul des modifications est affiché aux applications clientes. La valeur initiale contenue dans le cube est préservée et un suivi des modifications est enregistré dans la table d'écriture différée.  
  
 Les modifications apportées aux cellules feuilles et non-feuilles sont traitées différemment. Une cellule feuille représente une intersection d'une mesure et d'un membre feuille de toutes les dimensions référencées par le groupe de mesures. La valeur d'une cellule feuille est tirée directement de la table de faits et ne peut pas être divisée davantage en descendant dans la hiérarchie. Si un cube ou une partition est activé en écriture, il est possible de modifier une cellule feuille. Des modifications peuvent être apportées à une cellule non-feuille uniquement si l'application cliente offre un moyen de les diffuser parmi les cellules feuilles qui composent la cellule non-feuille. Ce processus, appelé allocation, est géré dans MDX (Multidimensional Expressions) à l'aide de l'instruction UPDATE CUBE. Les développeurs de solutions Business intelligence peuvent utiliser l'instruction UPDATE CUBE pour inclure la fonctionnalité d'allocation. Pour plus d’informations, consultez [instruction UPDATE CUBE &#40;MDX&#41;](../../mdx/mdx-data-manipulation-update-cube.md).  
  
> [!IMPORTANT]  
>  Quand les cellules mises à jour ne se chevauchent pas, la propriété de chaîne de connexion **Update Isolation Level** peut être utilisée pour améliorer les performances pour UPDATE CUBE. Pour plus d’informations, consultez <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A>.  
  
 Qu'une application cliente diffuse ou non les modifications apportées aux cellules non-feuilles, à chaque évaluation des requêtes, les modifications figurant dans la table d'écriture différée sont appliquées aux valeurs des cellules feuilles aussi bien qu'à celles des cellules non-feuilles, de sorte que les utilisateurs professionnels peuvent voir l'impact des modifications sur l'ensemble du cube.  
  
 Les modifications apportées par l'utilisateur professionnel sont conservées dans une table en écriture différée séparée que vous pouvez utiliser de la façon suivante :  
  
-   Effectuez une conversion en partition afin d'incorporer les modifications de façon permanente dans le cube. Cette action rend le groupe de mesures accessible en lecture seule. Vous pouvez spécifier une expression de filtre pour sélectionner les modifications à convertir.  
  
-   Ignorer la table d'écriture différée pour replacer la partition dans son état initial. Cette action rend la partition accessible en lecture seule.  
  
## <a name="security"></a>Sécurité  
 Un utilisateur professionnel est autorisé à enregistrer des modifications dans la table d'écriture différée d'un cube uniquement s'il appartient à un rôle ayant un accès en lecture/écriture aux cellules du cube. Pour chaque rôle, vous pouvez déterminer les cellules de cube qui peuvent être mises à jour et celles qui ne le peuvent pas. Pour plus d’informations, consultez [accorder des autorisations de cube ou modèle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Dimensions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md)   
 [Agrégations et conceptions d’agrégation](../../analysis-services/multidimensional-models-olap-logical-cube-objects/aggregations-and-aggregation-designs.md)   
 [Partitions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md)   
 [Dimensions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md)  
  
  
