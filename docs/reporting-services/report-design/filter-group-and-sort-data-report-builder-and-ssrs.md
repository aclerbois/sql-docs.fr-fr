---
title: Filtrer, regrouper et trier des données (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.categorygroupproperties.general.f1
- "10403"
- sql13.rtp.rptdesigner.categorygroupproperties.sorting.f1
- sql13.rtp.rptdesigner.seriesgroupproperties.general.f1
- "10402"
- "10410"
- sql13.rtp.rptdesigner.seriesgroupproperties.sorting.f1
- "10412"
ms.assetid: 4dda2a7f-3f31-47e9-a88b-28d770ebd65e
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c2173ba773d10cb443c3c8b973cd64cd453ce567
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filter-group-and-sort-data-report-builder-and-ssrs"></a>Filtrer, regrouper et trier des données (Générateur de rapports et SSRS)
  Dans un rapport, les expressions sont utilisées pour aider à contrôler, organiser et trier les données de rapport. Par défaut, lorsque vous créez des datasets et concevez la mise en page de rapport, les propriétés des éléments de rapport prennent automatiquement la valeur d'expressions en fonction des champs, paramètres et autres éléments de dataset qui s'affichent dans le volet des données de rapport. Vous pouvez également ajouter un bouton de tri interactif à une cellule de tableau ou de matrice afin de permettre à un utilisateur de modifier interactivement l'ordre de tri des lignes pour des groupes ou pour des lignes situées dans des groupes.  
  
-   **Expressions de filtre** Une expression de filtre teste les données à inclure ou à exclure selon une comparaison que vous spécifiez. Les filtres sont appliqués aux données d'un rapport une fois que ces dernières ont été récupérées à partir d'une connexion de données. Vous pouvez ajouter n'importe quelle combinaison de filtres aux éléments suivants : définition de dataset partagé sur le serveur de rapports, instance de dataset partagé ou dataset incorporé dans un rapport, région de données telle qu'un tableau ou un graphique, ou groupe de régions de données, par exemple un groupe de lignes dans un tableau ou un groupe de catégories dans un graphique.  
  
-   **Expressions de groupe** Une expression de groupe organise les données en fonction d'un champ de dataset ou de toute autre valeur. Les expressions de groupe sont créées automatiquement lorsque vous générez la mise en page de rapport. Le processeur de rapports évalue les expressions de groupe une fois que les filtres sont appliqués aux données, et lorsque les données de rapport et les régions de données sont combinées. Vous pouvez personnaliser une expression de groupe après sa création.  
  
-   **Expressions de tri** Une expression de tri contrôle l'ordre dans lequel les données s'affichent dans une région de données. Les expressions de tri sont créées automatiquement lorsque vous générez la mise en page de rapport. Par défaut, l'expression de tri d'un groupe a la même valeur que l'expression de groupe. Vous pouvez personnaliser une expression de tri après sa création.  
  
-   **Tri interactif** Pour permettre à un utilisateur de trier ou d'inverser l'ordre de tri d'une colonne, vous pouvez ajouter un bouton de tri interactif à une cellule d'en-tête de colonne ou d'en-tête de groupe dans un tableau ou une matrice.  
  
 Pour aider vos utilisateurs à personnaliser les expressions de filtre, de groupe ou de tri, vous pouvez modifier une expression afin d'ajouter une référence à un paramètre de rapport. Pour plus d'informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
 Pour plus d'informations et pour obtenir des exemples, consultez les rubriques suivantes :  
  
-   [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)  
  
-   [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)  
  
-   [Didacticiels du Générateur de rapports](../../reporting-services/report-builder-tutorials.md)  
  
-   [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)  
  
-   [Exemples de rapports (Générateur de rapports et SSRS)](http://go.microsoft.com/fwlink/?LinkId=198283)  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="Filtering"></a> Filtrage des données dans le rapport  
 Les filtres sont les éléments d'un rapport qui permettent de contrôler les données une fois qu'elles ont été récupérées à partir de la connexion de données. Utilisez des filtres lorsque vous ne pouvez pas modifier une requête de dataset pour filtrer les données avant qu'elles ne soient récupérées à partir d'une source de données externe.  
  
 Lorsque c'est possible, générez des requêtes de dataset qui retournent uniquement les données que vous devez afficher dans le rapport. Lorsque vous réduisez la quantité des données qui doivent être récupérées et traitées, vous contribuez à améliorer les performances du rapport. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
 Après avoir récupéré les données à partir de la source de données externe, vous pouvez ajouter des filtres aux datasets, aux régions de données et aux groupes de régions de données, notamment les groupes de détails. Les filtres sont appliqués dans un premier temps au moment de l'exécution sur le dataset, puis sur la région de données, puis sur le groupe, dans l'ordre de haut en bas des hiérarchies de groupe. Dans une table, une matrice ou une liste, les filtres des groupes de lignes, des groupes de colonnes et des groupes adjacents sont appliqués indépendamment. Dans un graphique, les filtres des groupes de catégories et des groupes de séries sont appliqués indépendamment. Pour plus d’informations, consultez [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
 Pour chaque filtre, vous spécifiez une *équation de filtre*. Une équation de filtre comprend un champ ou une expression de dataset qui identifie les données à filtrer, un opérateur et une valeur de comparaison. Seules les valeurs de données qui correspondent à la condition de filtre sont incluses lorsque l'élément est traité.  
  
 Pour permettre à vos utilisateurs de contrôler les données d'un rapport, vous pouvez inclure des paramètres dans les expressions de filtre. Pour plus d’informations, consultez [Informations de référence sur la collection de paramètres &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-parameters-collection-references-report-builder.md).  
  
 Pour personnaliser une vue pour chaque utilisateur, vous pouvez inclure une référence à un champ UserID dans un filtre. Pour plus d’informations, consultez [Références à des champs Globals et Users prédéfinis &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
  
##  <a name="Grouping"></a> Regroupement des données dans le rapport  
 Les groupes permettent d'organiser les données dans un rapport afin de les afficher ou de calculer des valeurs d'agrégat. En comprenant comment définir des groupes et utiliser leurs fonctionnalités, vous parviendrez à concevoir des rapports plus concis.  
  
 Les expressions de groupe sont créées automatiquement lorsque vous effectuez les opérations suivantes :  
  
-   réorganiser des champs de dataset dans un Assistant Tableau, Matrice ou Graphique, ou faire correspondre des champs dans l'Assistant Carte ;  
  
-   ajouter un champ à la zone Groupes de lignes ou Groupes de colonnes du volet de regroupement dans un tableau, une matrice ou une liste ;  
  
-   ajouter un champ à la zone Groupes d'abscisses ou Groupes de séries du volet Données du graphique dans un graphique ;  
  
-   spécifier un champ pour faire correspondre des éléments cartographiques à des données analytiques dans l'élément de menu contextuel Données de couche, dans une carte.  
  
 Un groupe fait partie de la définition de rapport. Chaque groupe a un nom. Par défaut, le nom de groupe correspond au champ de dataset sur lequel il est basé.  
  
 Dans une région de données de table ou de matrice, vous pouvez créer plusieurs groupes de lignes et groupes de colonnes. Vous pouvez afficher vos données selon une hiérarchie visuelle en organisant des groupes imbriqués, des groupes adjacents et des groupes de hiérarchies récursives (par exemple un organigramme).  
  
 Le nom de groupe identifie une étendue d'expression. Vous pouvez spécifier le nom d'un groupe en tant qu'étendue dans laquelle calculer des agrégats, organiser des données hiérarchiquement et activer/désactiver l'affichage des nœuds enfants des nœuds parents dans un rapport d'extraction, afin d'afficher des vues différentes des mêmes données sur plusieurs régions de données, et de visualiser les données de synthèse dans un tableau, une matrice, un graphique, une jauge ou une carte. Pour plus d’informations, consultez [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Pour effectuer un regroupement sur plusieurs champs du dataset, ajoutez chaque champ à l'ensemble d'expressions de groupe. Vous pouvez également écrire vos propres expressions de groupe dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Par exemple, vous pouvez effectuer un regroupement selon une plage de valeurs ou utiliser un paramètre de rapport pour permettre à l'utilisateur de sélectionner le mode de regroupement de données dans une région de données. Pour plus d’informations, consultez [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md).  
  
 Pour la présentation du rapport, vous pouvez ajouter des sauts de page avant et après chaque groupe ou instance d'un groupe afin de réduire le volume de données sur chaque page et mieux gérer les performances de rendu de rapport. Pour plus d’informations, consultez [Ajouter un saut de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-page-break-report-builder-and-ssrs.md).  
  
 La création de groupes de régions de données est une façon d'organiser les données d'un rapport. Il existe plusieurs autres façons d'organiser les données, avec chacune leurs avantages. Pour plus d’informations, consultez [Extraction, exploration, sous-rapports et régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drillthrough-drilldown-subreports-and-nested-data-regions.md).  
  
### <a name="defining-group-variables"></a>Définition de variables de groupe  
 Lorsque vous définissez un groupe, vous pouvez créer une variable de groupe à utiliser dans les expressions qui se limitent à ce groupe et sont accessibles à partir de groupes imbriqués. Une variable de groupe est calculée une fois par instance de groupe et est accessible à partir des expressions des groupes enfants. Par exemple, pour les données regroupées par région et sous-région, vous pouvez calculer une taxe pour chaque région et utiliser cette taxe dans les calculs du groupe de sous-régions.  
  
 Pour plus d’informations, consultez [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
### <a name="groups-and-scope-in-data-regions"></a>Groupes et étendue dans les régions de données  
 Pour fournir plusieurs vues de données à partir du même dataset, vous pouvez spécifier les mêmes expressions de groupe pour chaque région de données. Par exemple, vous pouvez afficher des données par catégories dans un tableau afin d'afficher toutes les données de détail, et faire de même dans un graphique à secteurs afin d'afficher des agrégats, ce qui facilite la visualisation de chaque catégorie par rapport à l'ensemble du dataset. Pour plus d’informations, consultez [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md).  
  
 Lorsque vous imbriquez une région de données dans une cellule de tableau, de matrice ou de liste, vous limitez automatiquement l'étendue des données aux appartenances aux groupes les plus profondes de la cellule. Par exemple, supposons que vous ajoutiez un graphique à une cellule située à la fois dans un groupe de lignes et dans un groupe de colonnes. Les données disponibles dans ce graphique sont limitées à l'instance de groupe de lignes la plus profonde et à l'instance de groupe de colonnes la plus profonde au moment de l'exécution. Pour plus d’informations, consultez [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
  
##  <a name="Sorting"></a> Tri des données dans le rapport  
 Pour maîtriser l'ordre de tri des données dans votre rapport, vous pouvez trier les données dans une requête de dataset ou définir une expression de tri pour un groupe ou une région de données. Vous pouvez également ajouter des boutons de tri interactif aux tableaux et aux matrices pour permettre à un utilisateur de modifier l'ordre de tri des lignes.  
  
 Les trois types de tri peuvent être associés dans un même rapport. Par défaut, l'ordre de tri est déterminé par l'ordre dans lequel les données sont retournées par la requête de dataset. Les expressions de tri sont appliquées dans la région de données et le groupe de régions de données. Les tris interactifs sont appliqués après les expressions de tri.  
  
 Pour les expressions qui contiennent des fonctions d'agrégation, la plupart des résultats ne sont pas affectés par l'ordre de tri. Les valeurs de retour des fonctions d’agrégation suivantes sont affectées par l’ordre de tri : First, Last et Previous. Pour plus d’informations, consultez [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
### <a name="sorting-data-in-a-dataset-query"></a>Tri des données dans une requête de dataset  
 Incluez l'ordre de tri dans la requête de dataset afin de pré-trier les données avant leur extraction pour un rapport. Le tri des données dans la requête est effectué par la source de données et non pas par le processeur de rapports.  
  
 Pour une source de données de type [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez ajouter une clause ORDER BY à la requête de dataset. Par exemple, la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante trie les colonnes Sales et Region by Sales dans la table SalesOrders par ordre décroissant : `SELECT Sales, Region FROM SalesOrders ORDER BY Sales DESC`. Pour plus d'informations, consultez la rubrique relative au tri des lignes à l'aide de ORDER BY dans la [documentation en ligne de SQL Server](http://go.microsoft.com/fwlink/?linkid=98335).  
  
> [!NOTE]  
>  Toutes les sources de données ne permettent pas de spécifier l'ordre de tri dans la requête.  
  
### <a name="sorting-data-with-sort-expressions"></a>Tri des données avec des expressions de tri  
 Pour trier des données dans le rapport après leur extraction de la source de données, vous pouvez définir des expressions de tri pour une région de données du tableau matriciel ou un groupe, notamment le groupe de détails. La liste suivante décrit l'effet de la définition d'expressions de tri sur différents éléments :  
  
-   **Région de données de tableau matriciel.** Définissez des expressions de tri sur une région de données de type liste, table ou matrice pour contrôler l'ordre de tri des données dans cette région de données après l'application de filtres de dataset et de région de données lors de l'exécution.  
  
-   **Groupe de régions de données de tableau matriciel.** Définissez des expressions de tri pour chaque groupe, dont le groupe de détails, pour contrôler l'ordre de tri des instances de groupe. Par exemple, dans le groupe de détails, vous contrôlez l'ordre des lignes de détails. Pour un groupe enfant, vous contrôlez l'ordre des instances de groupe à l'intérieur du groupe parent. Par défaut, lorsque vous créez un groupe, l'expression de tri est définie sur l'expression de groupe et l'ordre croissant.  
  
     Si vous n'avez qu'un seul groupe de détails, vous pouvez indifféremment définir une expression de tri dans la requête, dans la région de données ou dans le groupe de détails.  
  
-   **Région de données de graphique.** Définissez une expression de tri pour les groupes de catégories et de séries afin de contrôler l'ordre de tri des points de données. Par défaut, l'ordre des points de données est également l'ordre des couleurs dans la légende du graphique. Pour plus d’informations, consultez [Mise en forme des couleurs des séries d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
-   **Élément de rapport cartographique.** En règle générale, vous n'avez pas besoin de trier des données pour une région de données cartographiques, car la carte regroupe les données à afficher sur les éléments cartographiques.  
  
-   **Région de données de jauge.** Vous n'avez généralement pas besoin de trier les données dans une région de données de jauge, la jauge affichant une valeur unique relative à une plage. Si vous devez trier les données dans une jauge, définissez d'abord un groupe, puis l'expression de tri pour ce dernier.  
  
#### <a name="sorting-by-a-different-value"></a>Tri en fonction d'une autre valeur  
 Vous pouvez trier les lignes d'une région de données en fonction d'une autre valeur que la valeur de champ. Par exemple, le champ Size contient des valeurs texte qui correspondent à small, medium, large et extra large. Par défaut, l'expression de tri d'un groupe de lignes en fonction de Size est également [Size]. Pour avoir un meilleur contrôle de la façon dont les données sont triées, vous pouvez ajouter un champ à la requête de dataset dans le but de définir l'ordre de tri souhaité.  
  
 Vous pouvez également définir un dataset qui inclut uniquement les tailles et une valeur qui spécifie l'ordre souhaité. Vous pouvez modifier l'expression de tri afin d'utiliser la fonction Lookup pour la valeur d'ordre de tri.  
  
 Par exemple, la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante définit un dataset nommé Sizes. La requête utilise une instruction CASE pour définir une valeur d'ordre de tri SizeSortOrder pour chaque valeur de Size :  
  
```  
SELECT Size,   
  CASE Size  
        WHEN 'S' THEN 1  
        WHEN 'M' THEN 2    
        WHEN 'L' THEN 3  
        WHEN 'XL' THEN 4  
        ELSE 0  
  END as SizeSortOrder  
FROM Production.Product  
```  
  
 Dans un tableau qui comporte un groupe de lignes basé sur `[Size]`, vous pouvez modifier l'expression de tri du groupe afin d'utiliser une fonction Lookup pour rechercher le champ numérique qui correspond à la valeur de size. L'expression est semblable à ceci :  
  
```  
=Lookup(Fields!Size.Value, Fields!Size.Value, Fields!SizeSortOrder.Value, "Sizes")  
```  
  
 Pour plus d’informations, consultez [Trier des données dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md) et [Fonction Lookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md).  
  
###  <a name="Interactive"></a> Ajout du tri interactif pour l'utilisateur  
 Pour permettre à un utilisateur de modifier l'ordre de tri des données de rapport dans un tableau ou une matrice, vous pouvez ajouter des boutons de tri interactif aux en-têtes de colonnes ou aux en-têtes de groupes. Les utilisateurs peuvent cliquer sur le bouton pour basculer l'ordre de tri. Le tri interactif est pris en charge dans les formats de rendu qui permettent l'intervention de l'utilisateur, tels que le format HTML.  
  
 Vous ajoutez des boutons de tri interactif à une zone de texte dans une cellule de région de données de tableau matriciel. Par défaut, chaque cellule contient une zone de texte. Dans les propriétés de la zone de texte, spécifiez quelle partie d'une région de données de table ou de matrice doit être triée (les valeurs du groupe parent, les valeurs du groupe enfant ou les lignes de détails), les éléments d'après lesquels effectuer le tri et si l'expression de tri doit être appliquée à d'autres éléments de rapport qui ont une relation d'égal à égal. Par exemple, si une table et un graphique qui fournissent des vues sur le même dataset sont contenus dans un rectangle, ils constituent des régions de données homologues. Lorsqu'un utilisateur bascule l'ordre de tri de la table, l'ordre de tri du graphique bascule également. Pour plus d’informations, consultez [Tri interactif &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-report-builder-and-ssrs.md).  
  
  
##  <a name="HowTo"></a> Rubriques de procédures  
 [Laisser les en-têtes visibles lors du défilement d’un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/keep-headers-visible-when-scrolling-through-a-report-report-builder-and-ssrs.md)  
  
 [Afficher des en-têtes et des pieds de page de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)  
  
 [Ajouter un tri interactif à un tableau ou une matrice &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-interactive-sort-to-a-table-or-matrix-report-builder-and-ssrs.md)  
  
 [Définir un message d’absence de données pour une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/set-a-no-data-message-for-a-data-region-report-builder-and-ssrs.md)  
  
 [Créer un groupe de hiérarchies récursives &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-recursive-hierarchy-group-report-builder-and-ssrs.md)  
  
 [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md)  
  
 [Afficher des en-têtes et des pieds de page de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)  
  
 [Ajouter ou supprimer un groupe dans un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-chart-report-builder-and-ssrs.md)  
  
 [Ajouter un total à un groupe ou à une région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-total-to-a-group-or-tablix-data-region-report-builder-and-ssrs.md)  
  
##  <a name="Section"></a> Dans cette section  
 [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)  
  
 [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)  
  
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)  
  
##  <a name="Related"></a> Sections connexes  
 [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)  
  
 [Création de groupes de hiérarchies récursives &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md)  
  
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
 [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)  
  
 [Affichage d’une série avec plusieurs plages de données sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/displaying-a-series-with-multiple-data-ranges-on-a-chart.md)  
  
 [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Graphiques sparkline et barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)   
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
