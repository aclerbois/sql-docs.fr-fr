---
title: Référence des fonctions MDX (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7fd5b9ee4a70ac58ab44a056f0abfb1086d24b76
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580031"
---
# <a name="mdx-function-reference-mdx"></a>Guide de référence des fonctions MDX (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Permet d’utiliser des fonctions dans la syntaxe des Expressions multidimensionnelles (MDX). Ces fonctions peuvent être employées dans n'importe quelle instruction MDX valide ; elles sont souvent utilisées dans des requêtes, des définitions de cumul personnalisées et d'autres calculs. Cette section fournit des informations sur les fonctions MDX comprises dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
 Vous pouvez utiliser les tableaux ci-après pour rechercher des fonctions selon le type de valeurs qu'elles retournent, ou bien sélectionner une fonction par son nom dans la liste alphabétique du sommaire.  
  
## <a name="array-functions"></a>Fonctions de tableau  
  
|Fonction|Description|  
|--------------|-----------------|  
|[SetToArray &#40;MDX&#41;](../mdx/settoarray-mdx.md)|Convertit un ou plusieurs jeux en tableau utilisé par une fonction définie par l'utilisateur.|  
  
## <a name="hierarchy-functions"></a>Fonctions de hiérarchie  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Hiérarchie &#40;MDX&#41;](../mdx/hierarchy-mdx.md)|Retourne la hiérarchie qui contient le membre ou le niveau spécifié.|  
|[Dimension &#40;MDX&#41;](../mdx/dimension-mdx.md)|Retourne la dimension qui contient un membre, un niveau ou une hiérarchie spécifiés.|  
|[Dimensions &#40;MDX&#41;](../mdx/dimensions-mdx.md)|Retourne la hiérarchie spécifiée par une expression numérique ou de chaîne.|  
  
## <a name="level-functions"></a>Fonctions de niveau  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Niveau &#40;MDX&#41;](../mdx/level-mdx.md)|Retourne le niveau d'un membre.|  
|[Niveaux &#40;MDX&#41;](../mdx/levels-mdx.md)|Retourne le niveau dont la position dans une dimension ou une hiérarchie est spécifiée par une expression numérique ou dont le nom est spécifié par une expression de chaîne.|  
  
## <a name="logical-functions"></a>Fonctions logiques  
  
|Fonction|Description|  
|--------------|-----------------|  
|[IsAncestor &#40;MDX&#41;](../mdx/isancestor-mdx.md)|Retourne une valeur indiquant si un membre spécifié est un ancêtre d'un autre membre spécifié.|  
|[IsEmpty &#40;MDX&#41;](../mdx/isempty-mdx.md)|Retourne une valeur indiquant si l'expression évaluée est la valeur de la cellule vide.|  
|[IsGeneration &#40;MDX&#41;](../mdx/isgeneration-mdx.md)|Retourne une valeur indiquant si un membre spécifié est dans une génération spécifiée.|  
|[IsLeaf &#40;MDX&#41;](../mdx/isleaf-mdx.md)|Retourne une valeur indiquant si un membre spécifié est un membre feuille.|  
|[IsSibling &#40;MDX&#41;](../mdx/issibling-mdx.md)|Retourne une valeur indiquant si un membre spécifié est un frère d'un autre membre spécifié.|  
  
## <a name="member-functions"></a>Fonctions de membre  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Ancêtre &#40;MDX&#41;](../mdx/ancestor-mdx.md)|Retourne l'ancêtre d'un membre au niveau spécifié ou à la distance spécifiée.|  
|[ClosingPeriod &#40;MDX&#41;](../mdx/closingperiod-mdx.md)|Retourne le dernier frère parmi les descendants d'un membre à un niveau spécifique.|  
|[Cousin &#40;MDX&#41;](../mdx/cousin-mdx.md)|Retourne le membre enfant avec la même position relative sous un membre parent que le membre enfant spécifié.|  
|[CurrentMember &#40;MDX&#41;](../mdx/currentmember-mdx.md)|Retourne le membre actuel dans une dimension ou une hiérarchie spécifiée au cours d'une itération.|  
|[DataMember &#40;MDX&#41;](../mdx/datamember-mdx.md)|Retourne le membre de données générées par le système associé à un membre non-feuille d'une dimension.|  
|[DefaultMember &#40;MDX&#41;](../mdx/defaultmember-mdx.md)|Retourne le membre par défaut d'une dimension ou d'une hiérarchie.|  
|[FirstChild &#40;MDX&#41;](../mdx/firstchild-mdx.md)|Retourne le premier enfant d'un membre.|  
|[FirstSibling &#40;MDX&#41;](../mdx/firstsibling-mdx.md)|Retourne le premier enfant du parent d'un membre.|  
|[Élément &#40;membre&#41; &#40;MDX&#41;](../mdx/item-member-mdx.md)|Retourne un membre à partir d'un tuple spécifié.|  
|[Décalage &#40;MDX&#41;](../mdx/lag-mdx.md)|Retourne le membre qui est un nombre spécifié de positions avant un membre spécifié le long de la dimension du membre.|  
|[LastChild &#40;MDX&#41;](../mdx/lastchild-mdx.md)|Retourne le dernier enfant d'un membre spécifié.|  
|[LastSibling &#40;MDX&#41;](../mdx/lastsibling-mdx.md)|Retourne le dernier enfant du parent d'un membre spécifié.|  
|[Entraîner &#40;MDX&#41;](../mdx/lead-mdx.md)|Retourne un membre qui est un nombre spécifié de positions après un membre spécifié le long de la dimension du membre.|  
|[LinkMember &#40;MDX&#41;](../mdx/linkmember-mdx.md)|Retourne le membre équivalent à un membre spécifié dans une hiérarchie spécifique.|  
|[Membres &#40;chaîne&#41; &#40;MDX&#41;](../mdx/members-string-mdx.md)|Retourne un membre spécifié par une expression de chaîne.|  
|[NextMember &#40;MDX&#41;](../mdx/nextmember-mdx.md)|Retourne le membre suivant dans le niveau qui contient le membre spécifié.|  
|[OpeningPeriod &#40;MDX&#41;](../mdx/openingperiod-mdx.md)|Retourne le premier frère parmi les descendants d'un niveau spécifié, éventuellement à un membre spécifié.|  
|[ParallelPeriod &#40;MDX&#41;](../mdx/parallelperiod-mdx.md)|Retourne un membre d'une période antérieure dans la même position relative que le membre spécifié.|  
|[Parent &#40;MDX&#41;](../mdx/parent-mdx.md)|Retourne le parent d'un membre.|  
|[PrevMember &#40;MDX&#41;](../mdx/prevmember-mdx.md)|Retourne le membre précédent dans le niveau qui contient le membre spécifié.|  
|[StrToMember &#40;MDX&#41;](../mdx/strtomember-mdx.md)|Retourne le membre spécifié par une chaîne au format MDX.|  
|[UnknownMember &#40;MDX&#41;](../mdx/unknownmember-mdx.md)|Retourne le membre inconnu associé à un niveau ou un membre.|  
|[ValidMeasure &#40;MDX&#41;](../mdx/validmeasure-mdx.md)|Retourne une mesure valide dans un cube virtuel en forçant les dimensions inapplicables à leur niveau maximum.|  
  
## <a name="numeric-functions"></a>Fonctions numériques  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Agrégation &#40;MDX&#41;](../mdx/aggregate-mdx.md)|Retourne une valeur scalaire calculée par agrégation soit de mesures soit d'une expression numérique spécifiée éventuellement sur les tuples d'un jeu spécifié.|  
|[AVG &#40;MDX&#41;](../mdx/avg-mdx.md)|Retourne la valeur moyenne des mesures ou la valeur moyenne d'une expression numérique facultative, évaluée sur un jeu spécifié.|  
|[CalculationCurrentPass &#40;MDX&#41;](../mdx/calculationcurrentpass-mdx.md)|Retourne le test de calcul actuel d'un cube pour le contexte de requête spécifié.|  
|[CalculationPassValue &#40;MDX&#41;](../mdx/calculationpassvalue-mdx.md)|Retourne la valeur d’une expression MDX évaluée sur le test de calcul spécifié d’un cube.|  
|[CoalesceEmpty &#40;MDX&#41;](../mdx/coalesceempty-mdx.md)|Fusionne la valeur d'une cellule vide avec un nombre ou une chaîne et retourne la valeur fusionnée.|  
|[Corrélation &#40;MDX&#41;](../mdx/correlation-mdx.md)|Retourne le coefficient de corrélation de deux séries évaluées sur un jeu.|  
|[Nombre &#40;Dimension&#41; &#40;MDX&#41;](../mdx/count-dimension-mdx.md)|Retourne le nombre de dimensions contenues dans un cube.|  
|[Nombre &#40;des niveaux de hiérarchie&#41; &#40;MDX&#41;](../mdx/count-hierarchy-levels-mdx.md)|Retourne le nombre de niveaux d'une dimension ou d'une hiérarchie.|  
|[Nombre &#40;définir&#41; &#40;MDX&#41;](../mdx/count-set-mdx.md)|Retourne le nombre de cellules d'un jeu.|  
|[Nombre &#40;Tuple&#41; &#40;MDX&#41;](../mdx/count-tuple-mdx.md)|Retourne le nombre de dimensions d'un tuple.|  
|[Covariance &#40;MDX&#41;](../mdx/covariance-mdx.md)|Retourne la covariance de remplissage de deux séries évaluées sur un jeu, à l'aide de la formule de remplissage biaisée.|  
|[CovarianceN &#40;MDX&#41;](../mdx/covariancen-mdx.md)|Retourne l'exemple de covariance de deux séries évaluées sur un jeu, à l'aide de la formule de remplissage non biaisée.|  
|[DistinctCount &#40;MDX&#41;](../mdx/distinctcount-mdx.md)|Retourne le nombre des différents tuples non vides d'un jeu.|  
|[IIf &#40;MDX&#41;](../mdx/iif-mdx.md)|Retourne l'une des deux valeurs déterminées par un test logique.|  
|[LinRegIntercept &#40;MDX&#41;](../mdx/linregintercept-mdx.md)|Calcule la régression linéaire d’un jeu et retourne la valeur de l’ordonnée à l’origine dans la ligne de régression y = ax + b.|  
|[LinRegPoint &#40;MDX&#41;](../mdx/linregpoint-mdx.md)|Calcule la régression linéaire d’un jeu et retourne la valeur de *y* dans la ligne de régression y = ax + b.|  
|[LinRegR2 &#40;MDX&#41;](../mdx/linregr2-mdx.md)|Calcule la régression linéaire d'un jeu et retourne R2 (coefficient de détermination).|  
|[LinRegSlope &#40;MDX&#41;](../mdx/linregslope-mdx.md)|Calcule la régression linéaire d’un jeu et retourne la valeur de la pente dans la ligne de régression y = ax + b.|  
|[LinRegVariance &#40;MDX&#41;](../mdx/linregvariance-mdx.md)|Calcule la régression linéaire d’un jeu et retourne la variance associée à la ligne de régression y = ax + b.|  
|[LookupCube &#40;MDX&#41;](../mdx/lookupcube-mdx.md)|Retourne la valeur d'une expression MDX évaluée sur un autre cube spécifié dans la même base de données.|  
|[Max &#40;MDX&#41;](../mdx/max-mdx.md)|Retourne la valeur maximale d'une expression numérique évaluée sur un jeu.|  
|[Valeur médiane &#40;MDX&#41;](../mdx/median-mdx.md)|Retourne la valeur médiane d'une expression numérique évaluée sur un jeu.|  
|[Min &#40;MDX&#41;](../mdx/min-mdx.md)|Retourne la valeur minimale d'une expression numérique évaluée sur un jeu.|  
|[Ordinal &#40;MDX&#41;](../mdx/ordinal-mdx.md)|Retourne la valeur ordinale de base zéro associée à un niveau.|  
|[Prédire &#40;MDX&#41;](../mdx/predict-mdx.md)|Retourne une valeur d'expression numérique évaluée sur un modèle d'exploration de données.|  
|[Rang &#40;MDX&#41;](../mdx/rank-mdx.md)|Retourne le rang de base un d'un tuple dans un jeu.|  
|[RollupChildren &#40;MDX&#41;](../mdx/rollupchildren-mdx.md)|Retourne une valeur générée par le cumul des valeurs des enfants d'un membre spécifié à l'aide de l'opérateur unaire spécifié.|  
|[STDDEV &#40;MDX&#41;](../mdx/stddev-mdx.md)|Alias de [Stdev &#40;MDX&#41;](../mdx/stdev-mdx.md).|  
|[StddevP &#40;MDX&#41;](../mdx/stddevp-mdx.md)|Alias de [StdevP &#40;MDX&#41;](../mdx/stdevp-mdx.md).|  
|[StDev &#40;MDX&#41;](../mdx/stdev-mdx.md)|Retourne l'écart-type d'une expression numérique évaluée sur un jeu, à l'aide de la formule de remplissage non biaisée.|  
|[StdevP &#40;MDX&#41;](../mdx/stdevp-mdx.md)|Retourne l'écart-type du remplissage d'une expression numérique évaluée sur un jeu, à l'aide de la formule de remplissage biaisée.|  
|[StrToValue &#40;MDX&#41;](../mdx/strtovalue-mdx.md)|Retourne la valeur spécifiée par une chaîne au format MDX.|  
|[Somme &#40;MDX&#41;](../mdx/sum-mdx.md)|Retourne la somme d'une expression numérique évaluée sur un jeu.|  
|[Valeur &#40;MDX&#41;](../mdx/value-mdx.md)|Retourne la valeur d'une mesure.|  
|[Var &#40;MDX&#41;](../mdx/var-mdx.md)|Retourne l'exemple de variance d'une expression numérique évaluée sur un jeu, à l'aide de la formule de remplissage non biaisée.|  
|[Variance &#40;MDX&#41;](../mdx/variance-mdx.md)|Alias de [Var &#40;MDX&#41;](../mdx/var-mdx.md).|  
|[VarianceP &#40;MDX&#41;](../mdx/variancep-mdx.md)|Alias de [VarP &#40;MDX&#41;](../mdx/varp-mdx.md).|  
|[VarP &#40;MDX&#41;](../mdx/varp-mdx.md)|Retourne la variance du remplissage d'une expression numérique évaluée sur un jeu, à l'aide de la formule de remplissage biaisée.|  
  
## <a name="set-functions"></a>Fonctions de jeu  
  
|Fonction|Description|  
|--------------|-----------------|  
|[AddCalculatedMembers &#40;MDX&#41;](../mdx/addcalculatedmembers-mdx.md)|Retourne un jeu généré par l'ajout de membres calculés à un jeu spécifié.|  
|[AllMembers &#40;MDX&#41;](../mdx/allmembers-mdx.md)|Retourne un jeu qui contient tous les membres de la dimension, de la hiérarchie ou du niveau spécifié, notamment les membres calculés.|  
|[Ancêtres &#40;MDX&#41;](../mdx/ancestors-mdx.md)|Retourne un jeu de tous les ancêtres d'un membre à un niveau spécifié ou à une distance spécifiée.|  
|[Ascendants &#40;MDX&#41;](../mdx/ascendants-mdx.md)|Retourne le jeu des ascendants du membre spécifié, notamment le membre lui-même.|  
|[Axe &#40;MDX&#41;](../mdx/axis-mdx.md)|Retourne un jeu défini dans un axe.|  
|[BottomCount &#40;MDX&#41;](../mdx/bottomcount-mdx.md)|Trie un jeu en ordre croissant et retourne le nombre spécifié de tuples avec les valeurs les plus basses.|  
|[BottomPercent &#40;MDX&#41;](../mdx/bottompercent-mdx.md)|Trie un jeu en ordre croissant, et retourne un jeu de tuples avec les valeurs les plus basses dont le total cumulé est égal ou inférieur à un pourcentage spécifié.|  
|[BottomSum &#40;MDX&#41;](../mdx/bottomsum-mdx.md)|Trie un jeu en ordre croissant et retourne un jeu de tuples avec les valeurs les plus basses dont le total est égal ou inférieur à une valeur spécifiée.|  
|[Enfants &#40;MDX&#41;](../mdx/children-mdx.md)|Retourne les enfants d'un membre spécifié.|  
|[Crossjoin &#40;MDX&#41;](../mdx/crossjoin-mdx.md)|Retourne le produit croisé d'un ou plusieurs jeux.|  
|[CurrentOrdinal &#40;MDX&#41;](../mdx/currentordinal-mdx.md)|Retourne le numéro d'itération actuel dans un jeu lors d'une itération.|  
|[Descendants &#40;MDX&#41;](../mdx/descendants-mdx.md)|Retourne le jeu de descendants d'un membre à un niveau spécifié ou à une distance spécifiée, en incluant ou en excluant éventuellement des descendants dans d'autres niveaux.|  
|[Distinct &#40;MDX&#41;](../mdx/distinct-mdx.md)|Retourne un jeu et supprime les tuples dupliqués à partir d'un jeu spécifié.|  
|[DrilldownLevel &#40;MDX&#41;](../mdx/drilldownlevel-mdx.md)|Extrait vers le bas les membres d'un jeu d'un niveau sous le niveau le plus bas représenté dans le jeu, ou d'un niveau sous le niveau facultatif d'un membre représenté dans le jeu.|  
|[DrilldownLevelBottom &#40;MDX&#41;](../mdx/drilldownlevelbottom-mdx.md)|Extrait vers le bas les membres les plus bas d'un jeu, d'un niveau à partir d'un niveau spécifié.|  
|[DrilldownLevelTop &#40;MDX&#41;](../mdx/drilldownleveltop-mdx.md)|Extrait vers le bas les membres les plus hauts d'un jeu, d'un niveau à partir du niveau spécifié.|  
|[DrilldownMember &#40;MDX&#41;](../mdx/drilldownmember-mdx.md)|Extrait vers le bas les membres dans un jeu spécifié, qui sont présents dans un second jeu spécifié. Vous pouvez également la fonction extrait vers le bas sur un jeu de tuples.|  
|[DrilldownMemberBottom &#40;MDX&#41;](../mdx/drilldownmemberbottom-mdx.md)|Extrait vers le bas les membres dans un jeu spécifié qui sont présents dans un second jeu spécifié, ce qui limite l'ensemble de résultats à un nombre spécifique de membres. Cette fonction permet également d'extraire vers le bas un jeu de tuples.|  
|[DrilldownMemberTop &#40;MDX&#41;](../mdx/drilldownmembertop-mdx.md)|Extrait vers le bas les membres dans un jeu spécifié qui sont présents dans un second jeu spécifié, ce qui limite l'ensemble de résultats à un nombre spécifique de membres. Sinon, cette fonction extrait vers le bas sur un jeu de tuples.|  
|[DrillupLevel &#40;MDX&#41;](../mdx/drilluplevel-mdx.md)|Extrait vers le haut les membres d'un jeu situé au-dessous du niveau spécifié.|  
|[DrillupMember &#40;MDX&#41;](../mdx/drillupmember-mdx.md)|Extrait vers le haut les membres d'un ensemble spécifié qui sont présents dans un second jeu.|  
|[À l’exception &#40;MDX&#41;](../mdx/except-mdx-function.md)|Recherche la différence entre deux jeux, en conservant éventuellement les doublons.|  
|[Existe &#40;MDX&#41;](../mdx/exists-mdx.md)|Retourne le jeu des membres d'un jeu qui existe avec un ou plusieurs tuples d'un ou plusieurs autres jeux.|  
|[Extraire &#40;MDX&#41;](../mdx/extract-mdx.md)|Retourne un jeu de tuples à partir d'éléments de dimension extraits.|  
|[Filtre &#40;MDX&#41;](../mdx/filter-mdx.md)|Retourne le jeu résultant du filtrage d'un jeu spécifié selon une condition de recherche.|  
|[Générer &#40;MDX&#41;](../mdx/generate-mdx.md)|Applique un jeu à chaque membre d'un autre jeu, puis effectue la jointure par union des jeux résultants. Cette fonction retourne également une chaîne concaténée créée par l'évaluation d'une expression de chaîne sur un jeu.|  
|[Head &#40;MDX&#41;](../mdx/head-mdx.md)|Retourne le premier nombre spécifié d'éléments dans un jeu, en conservant les doublons.|  
|[Hierarchize &#40;MDX&#41;](../mdx/hierarchize-mdx.md)|Ordonne les membres d'un jeu en hiérachie.|  
|[Intersect &#40;MDX&#41;](../mdx/intersect-mdx.md)|Retourne l'intersection de deux ensembles d'entrée, en conservant éventuellement les doublons.|  
|[LastPeriods &#40;MDX&#41;](../mdx/lastperiods-mdx.md)|Retourne le jeu des membres antérieurs à et incluant un membre spécifié.|  
|[Membres &#40;définir&#41; &#40;MDX&#41;](../mdx/members-set-mdx.md)|Retourne le jeu des membres d'une dimension, d'un niveau ou d'une hiérarchie.|  
|[MTd &#40;MDX&#41;](../mdx/mtd-mdx.md)|Retourne un jeu des membres frères de même niveau qu'un membre donné, commençant par le premier frère et se terminant par le membre donné, conformément à la contrainte du niveau Year de la dimension Time.|  
|[NameToSet &#40;MDX&#41;](../mdx/nametoset-mdx.md)|Retourne un jeu qui contient le membre spécifié par une chaîne au format MDX.|  
|[NonEmptyCrossjoin &#40;MDX&#41;](../mdx/nonemptycrossjoin-mdx.md)|Retourne le produit croisé d'un ou plusieurs jeux sous la forme d'un jeu, en excluant les tuples vides et les tuples qui ne sont associés à aucune table de faits.|  
|[Commande &#40;MDX&#41;](../mdx/order-mdx.md)|Organise les membres d'un jeu spécifié, en conservant ou non la hiérarchie.|  
|[PeriodsToDate &#40;MDX&#41;](../mdx/periodstodate-mdx.md)|Retourne un jeu des membres frères de même niveau qu'un membre donné, commençant par le premier frère et se terminant par le membre donné, conformément à la contrainte du niveau spécifié de la dimension Time.|  
|[Qtd &#40;MDX&#41;](../mdx/qtd-mdx.md)|Retourne un jeu de frères membres d’un même niveau qu’un membre donné, en commençant par le premier frère et se terminant par le membre donné, en tant que contrainte par la *trimestre* au niveau de la dimension Time.|  
|[Frères &#40;MDX&#41;](../mdx/siblings-mdx.md)|Retourne les frères d'un membre spécifié, notamment le membre lui-même.|  
|[StripCalculatedMembers &#40;MDX&#41;](../mdx/stripcalculatedmembers-mdx.md)|Retourne un jeu généré en supprimant les membres calculés à partir d'un jeu spécifique.|  
|[StrToSet &#40;MDX&#41;](../mdx/strtoset-mdx.md)|Retourne le jeu spécifié par une chaîne au format MDX.|  
|[Sous-ensemble &#40;MDX&#41;](../mdx/subset-mdx.md)|Retourne un sous-ensemble de tuples d'un jeu spécifié.|  
|[La fin du &#40;MDX&#41;](../mdx/tail-mdx.md)|Retourne un sous-ensemble de la fin d'un jeu.|  
|[ToggleDrillState &#40;MDX&#41;](../mdx/toggledrillstate-mdx.md)|Fait basculer l'état d'extraction des membres.|  
|[TopCount &#40;MDX&#41;](../mdx/topcount-mdx.md)|Trie un jeu en ordre décroissant et retourne le nombre spécifié d'éléments avec les valeurs les plus élevées.|  
|[TopPercent &#40;MDX&#41;](../mdx/toppercent-mdx.md)|Trie un jeu en ordre décroissant et retourne un jeu de tuples avec les valeurs les plus élevées dont le total cumulé est égal ou inférieur à un pourcentage spécifié.|  
|[TopSum &#40;MDX&#41;](../mdx/topsum-mdx.md)|Trie un jeu et retourne les éléments situés le plus en haut, dont le total cumulatif est au moins une valeur spécifiée.|  
|[Union &#40;MDX&#41;](../mdx/union-mdx.md)|Retourne l'union de deux jeux, en conservant éventuellement les doublons.|  
|[Unorder &#40;MDX&#41;](../mdx/unorder-mdx.md)|Supprime tout classement appliqué d'un jeu spécifié.|  
|[VisualTotals &#40;MDX&#41;](../mdx/visualtotals-mdx.md)|Retourne un jeu généré en effectuant le total des membres enfants de manière dynamique dans un jeu spécifié, en utilisant éventuellement un modèle pour le nom du membre parent dans le jeu de cellules résultant.|  
|[Wtd &#40;MDX&#41;](../mdx/wtd-mdx.md)|Retourne un jeu des membres frères de même niveau qu'un membre donné, commençant par le premier frère et se terminant par le membre donné, conformément à la contrainte du niveau Week de la dimension Time.|  
|[YTD &#40;MDX&#41;](../mdx/ytd-mdx.md)|Retourne un jeu de frères membres d’un même niveau qu’un membre donné, en commençant par le premier frère et se terminant par le membre donné, en tant que contrainte par la *année* au niveau de la dimension Time.|  
  
## <a name="string-functions"></a>Fonctions de chaîne  
  
|Fonction|Description|  
|--------------|-----------------|  
|[CalculationPassValue &#40;MDX&#41;](../mdx/calculationpassvalue-mdx.md)|Retourne la valeur d'une expression MDX évaluée sur le test de calcul spécifié d'un cube.|  
|[CoalesceEmpty &#40;MDX&#41;](../mdx/coalesceempty-mdx.md)|Fusionne la valeur d'une cellule vide avec un nombre ou une chaîne et retourne la valeur fusionnée.|  
|[Générer &#40;MDX&#41;](../mdx/generate-mdx.md)|Applique un jeu à chaque membre d'un autre jeu, puis effectue la jointure par union des jeux résultants. Cette fonction retourne également une chaîne concaténée créée par l'évaluation d'une expression de chaîne sur un jeu.|  
|[IIf &#40;MDX&#41;](../mdx/iif-mdx.md)|Retourne l'une des deux valeurs déterminées par un test logique.|  
|[LookupCube &#40;MDX&#41;](../mdx/lookupcube-mdx.md)|Retourne la valeur d'une expression MDX évaluée sur un autre cube spécifié dans la même base de données.|  
|[MemberToStr &#40;MDX&#41;](../mdx/membertostr-mdx.md)|Retourne une chaîne au format MDX correspondant à un membre spécifié.|  
|[Nom &#40;MDX&#41;](../mdx/name-mdx.md)|Retourne le nom d'une dimension, d'une hiérarchie, d'un niveau ou d'un membre.|  
|[Propriétés &#40;MDX&#41;](../mdx/properties-mdx.md)|Retourne une chaîne ou une valeur fortement typée, contenant une valeur de propriété de membre.|  
|[SetToStr &#40;MDX&#41;](../mdx/settostr-mdx.md)|Retourne une chaîne au format MDX correspondant à un jeu spécifié.|  
|[TupleToStr &#40;MDX&#41;](../mdx/tupletostr-mdx.md)|Retourne une chaîne au format MDX correspondant au tuple spécifié.|  
|[UniqueName &#40;MDX&#41;](../mdx/uniquename-mdx.md)|Retourne le nom unique d'une dimension, d'une hiérarchie, d'un niveau ou d'un membre spécifié.|  
|[Nom d’utilisateur &#40;MDX&#41;](../mdx/username-mdx.md)|Retourne le nom de domaine et le nom d'utilisateur de la connexion en cours.|  
  
## <a name="subcube-functions"></a>Fonctions de sous-cube  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Cela &#40;MDX&#41;](../mdx/this-mdx.md)|Retourne le sous-cube actuel.|  
|[Laisse &#40;MDX&#41;](../mdx/leaves-mdx.md)|Retourne le jeu des membres feuilles situés dans la dimension, le membre ou le tuple spécifié.|  
  
## <a name="tuple-functions"></a>fonctions de tuple  
  
|Fonction|Description|  
|--------------|-----------------|  
|[En cours &#40;MDX&#41;](../mdx/current-mdx.md)|Retourne le tuple actif dans un jeu lors d'une itération.|  
|[Élément &#40;Tuple&#41; &#40;MDX&#41;](../mdx/item-tuple-mdx.md)|Retourne un tuple d'un jeu.|  
|[Racine &#40;MDX&#41;](../mdx/root-mdx.md)|Retourne un tuple qui se compose de la **tous les** membres de chaque hiérarchie d’attribut dans un cube, une dimension ou un tuple.|  
|[StrToTuple &#40;MDX&#41;](../mdx/strtotuple-mdx.md)|Retourne le tuple spécifié par une chaîne au format MDX.|  
  
## <a name="other-functions"></a>Autres fonctions  
  
|Fonction|Description|  
|--------------|-----------------|  
|[Erreur &#40;MDX&#41;](../mdx/error-mdx.md)|Génère une erreur, et fournit éventuellement un message d'erreur spécifié.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence du langage MDX &#40;MDX&#41;](../mdx/mdx-language-reference-mdx.md)  
  
  
