---
title: Fonctionnement des groupes (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- "10056"
- "10424"
ms.assetid: c32d4d89-45e4-4f77-a3e9-0429f53f9d6f
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 43dc5ff283b55d76aa31e9317131508fcafdc395
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-groups-report-builder-and-ssrs"></a>Fonctionnement des groupes (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , un groupe est un jeu de données nommé provenant du dataset du rapport qui est lié à une région de données. En principe, un groupe organise une vue d'un dataset du rapport. Tous les groupes d'une région de données spécifient des vues différentes du même dataset de rapport.  
  
 Pour mieux visualiser ce qu’est un groupe, reportez-vous à l’illustration suivante qui montre la région de données de tableau matriciel dans l’aperçu. Dans cette illustration, les groupes de lignes classent le dataset par type de produit et les groupes de colonnes par région géographique et année.  
  
 ![Tablix data region areas](../../reporting-services/report-design/media/rs-tablixareas.gif "Tablix data region areas")  
  
 Les sections suivantes décrivent les divers aspects des groupes.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="what-makes-a-group"></a>Quels sont les composants d'un groupe ?  
 Un groupe possède un nom et un ensemble d'expressions de groupe que vous spécifiez. L'ensemble des expressions de groupe peut être une référence de champ de dataset unique ou une combinaison de plusieurs expressions. Lors de l'exécution, les expressions de groupe sont combinées (si le groupe possède plusieurs expressions) et appliquées aux données d'un groupe. Par exemple, vous avez un groupe qui utilise un champ de date pour organiser les données dans la région de données. Lors de l'exécution, les données sont organisées par date, puis affichées avec les totaux d'autres valeurs de dataset pour chaque date.  
  
## <a name="when-do-i-create-groups"></a>Quand dois-je créer des groupes ?  
 Dans la plupart des cas, le Générateur de rapports et le Concepteur de rapports créent automatiquement un groupe lorsque vous concevez une région de données. Pour un tableau, une matrice ou une liste, des groupes sont créés lorsque vous déposez des champs dans le volet de regroupement. Pour un graphique, des groupes sont créés lorsque vous déposez des champs dans les zones de dépôt du graphique. Pour une jauge, vous devez utiliser la boîte de dialogue des propriétés de la jauge. Pour une table, une matrice ou une liste, vous pouvez également créer manuellement un groupe. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md). Pour obtenir un exemple montrant comment ajouter des groupes quand vous créez un rapport, consultez [Didacticiel : création d’un rapport de tableau de base &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md) ou [Créer un rapport de table de base &#40;Didacticiel SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  
  
## <a name="how-can-i-modify-a-group"></a>Comment puis-je modifier un groupe ?  
 Après avoir créé un groupe, vous pouvez définir les propriétés spécifiques à la région de données, telles que les expressions de filtre et de tri, les sauts de page et les variables de groupe pour accepter des données spécifiques à l'étendue. Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md).  
  
 Pour modifier un groupe existant, ouvrez la boîte de dialogue de propriétés du groupe appropriée. Vous pouvez modifier le nom du groupe. De même, vous pouvez spécifier des expressions de groupe basées sur un champ unique ou plusieurs champs ou sur un paramètre de rapport qui spécifie une valeur au moment de l'exécution. Vous pouvez également baser un groupe sur un ensemble d'expressions, tel que l'ensemble d'expressions qui spécifie des tranches d'âge pour les données démographiques. Pour plus d’informations, consultez [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  Si vous modifiez le nom d'un groupe, vous devez mettre à jour manuellement toutes les expressions de groupe qui font référence au nom précédent du groupe.  
  
## <a name="how-are-groups-organized"></a>Comment les groupes sont-ils organisés ?  
 Comprendre le principe de l'organisation des groupes peut vous aider à concevoir des régions de données qui affichent des vues différentes des mêmes données en spécifiant des expressions de groupe identiques.  
  
 Les groupes sont organisés en interne sous la forme de membres d'une ou de plusieurs hiérarchies pour chaque région de données. Une hiérarchie de groupes comporte des groupes parents/enfants qui sont imbriqués et peuvent avoir des groupes adjacents.  
  
 Si vous considérez les groupes parents/enfants comme une arborescence, chaque hiérarchie de groupes constitue une forêt d'arborescences. Une région de données de tableau matriciel inclut une hiérarchie de groupes de lignes et une hiérarchie de groupes de colonnes. Les données associées aux membres de groupe de lignes s'affichent horizontalement sur la page et les données associées aux membres de groupe de colonnes verticalement. Le volet Regroupement affiche les membres de groupes de lignes et de groupes de colonnes pour la région de données de tableau matriciel actuellement sélectionnée sur l'aire de conception. Pour plus d’informations, consultez [Volet de regroupement &#40;Générateur de rapports&#41;](../../reporting-services/report-design/grouping-pane-report-builder.md).  
  
 Une région des données de graphique inclut une hiérarchie de groupes de catégories et une hiérarchie de groupes de séries. Les membres de groupes de catégorie sont affichés sur l'axe des abscisses et les membres de groupes de séries sur l'axe des ordonnées.  
  
 Bien qu'ils ne soient généralement pas nécessaires pour les régions de données de jauge, les groupes vous permettent d'indiquer la manière de regrouper les données à agréger sur la jauge.  
  
## <a name="what-types-of-groups-are-available-per-data-region"></a>Quels sont les types de jauge disponibles par région de données ?  
 Les régions de données qui se développent sous la forme de grille prennent en charge des groupes différents de ceux pris en charge par les régions de données qui affichent visuellement des données de synthèse. Par conséquent, une région de données de tableau matriciel, et les tables, les listes et les matrices qui sont basées sur la région de données de tableau matriciel prennent en charge des groupes différents de ceux pris en charge par un graphique ou une jauge. Les sections suivantes présentent le type et le but du regroupement dans chaque type de région de données.  
  
> [!NOTE]  
>  Bien que les groupes aient des noms différents dans les différentes régions de données, les principes qui sous-tendent la création et l'utilisation de ces groupes sont identiques. Lorsque vous créez un groupe pour une région de données, vous spécifiez une méthode pour organiser les données de détail du dataset qui est lié à la région de données. Chaque région de données prend en charge une structure de groupe sur laquelle afficher des données groupées.  
  
### <a name="groups-in-a-tablix-data-region-details-row-and-column-groups"></a>Groupes dans une région de données de tableau matriciel : groupes de détails, de lignes et de colonnes  
 Comme indiqué précédemment dans cette rubrique, une région de données de tableau matriciel vous permet d'organiser des données en groupes par lignes ou par colonnes. Toutefois, les groupes de lignes et de colonnes ne sont pas les seuls groupes disponibles dans une région de données de tableau matriciel. Cette région de données peut comporter les types de groupes suivants :  
  
-   **Groupe Détails** : le groupe Détails se compose de toutes les données extraites d’un dataset de rapport après que le Générateur de rapports et le Concepteur de rapports ont appliqué les filtres de datasets et de régions de données. Le groupe de détails est donc le seul groupe qui ne comporte aucune expression de groupe.  
  
     En principe, le groupe de détails spécifie les données qui s'affichent lorsque vous exécutez une requête de dataset dans un concepteur de requêtes. Par exemple, vous avez une requête qui extrait toutes les colonnes d'un tableau de commandes client. Les données dans ce groupe de détails incluent donc toutes les valeurs de chaque ligne pour toutes les colonnes du tableau. Les données dans ce groupe de détails incluent également des valeurs pour tous les champs de dataset calculés que vous avez créés.  
  
    > [!NOTE]  
    >  Les données d'un groupe de détails peuvent également inclure des agrégats de serveurs, qui sont des agrégats calculés sur la source de données et récupérés dans votre requête. Par défaut, le Générateur de rapports et le Concepteur de rapports traitent les agrégats de serveurs comme des données de détail, sauf si votre rapport inclut une expression qui utilise la fonction d’agrégation. Pour plus d’informations, consultez [Agrégation](../../reporting-services/report-design/report-builder-functions-aggregate-function.md).  
  
     Par défaut, lorsque vous ajoutez un tableau ou une liste à votre rapport, le Générateur de rapports et le Concepteur de rapports créent automatiquement le groupe de détails et ajoute une ligne pour afficher les données de détail. Par défaut, lorsque vous ajoutez des champs de dataset aux cellules de cette ligne, vous voyez des expressions simples pour les champs, par exemple, [Sales]. Lorsque vous consultez la région de données, la ligne de détails n'est utilisée qu'à une seule reprise pour chaque valeur du jeu de résultats.  
  
-   **Groupes de lignes et groupes de colonnes** : vous pouvez organiser des données en groupes par lignes ou par colonnes. Les groupes de lignes apparaissent verticalement sur une page. Les groupes de colonnes apparaissent horizontalement sur une page. Les groupes peuvent être imbriqués, par exemple, vous pouvez regrouper d'abord par [Year], puis par [Quarter], puis par [Month]. Les groupes peuvent également être adjacents, par exemple, avec l'instruction de regrouper sur [Territory] et indépendamment sur [ProductCategory].  
  
     Lorsque vous créez un groupe pour une région de données, le Générateur de rapports et le Concepteur de rapports ajoutent automatiquement des lignes ou des colonnes à la région de données et utilisent ces lignes ou colonnes pour afficher des données de groupe.  
  
-   **Groupes de hiérarchies récursives** : un groupe de hiérarchies récursives organise les données à partir d’un seul dataset de rapport qui inclut plusieurs niveaux. Par exemple, un groupe de hiérarchies récursives peut afficher un organigramme fonctionnel, par exemple, [Employee] qui est sous la responsabilité de [Employee]. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit des propriétés de groupe et des fonctions intégrées qui vous permettent de créer des groupes pour ce type de données de rapport. Pour plus d’informations, consultez [Création de groupes de hiérarchies récursives &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
 La liste suivante résume la façon dont vous utilisez les groupes pour chaque région de données :  
  
-   **Table** : définit des groupes des lignes imbriquées, des groupes de lignes adjacentes et des groupes de lignes de hiérarchie récursive (comme pour un organigramme fonctionnel). Par défaut, une table inclut un groupe de détails. Ajoutez des groupes en faisant glisser des champs de dataset sur le volet de regroupement pour une table sélectionnée.  
  
-   **Matrice** : définit des groupes de lignes et de colonnes imbriquées, et des groupes de lignes et de colonnes adjacentes. Ajoutez des groupes en faisant glisser des champs de dataset sur le volet de regroupement pour une matrice sélectionnée.  
  
-   **Liste** : par défaut, prend en charge le groupe Détails. Généralement utilisée pour prendre en charge un niveau unique de regroupement. Ajoutez des groupes en faisant glisser des champs de dataset sur le volet de regroupement pour une liste sélectionnée.  
  
 Après que vous avez ajouté un groupe, les handles de ligne et de colonne de la région de données se modifient pour refléter l'appartenance aux groupes. Lorsque vous supprimez un groupe, vous avez le choix entre supprimer la définition de groupe uniquement et supprimer le groupe et toutes ses lignes et colonnes associées. Pour plus d’informations, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
 Pour limiter les données à afficher ou utiliser dans les calculs pour les données de détail ou de groupe, définissez des filtres sur le groupe. Pour plus d’informations, consultez [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
 Par défaut, lorsque vous créez un groupe, l'expression de tri pour ce groupe est identique à l'expression de groupe. Pour modifier l'ordre de tri, modifiez l'expression de tri. Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md).  
  
#### <a name="understanding-group-membership-for-tablix-cells"></a>Fonctionnement de l'appartenance aux groupes pour les cellules de tableau matriciel  
 Les cellules d'une ligne ou d'une colonne d'une région de données de tableau matriciel peuvent appartenir à plusieurs groupes de lignes et de colonnes. Lorsque vous définissez une expression dans la zone de texte d'une cellule qui utilise une fonction d'agrégation (par exemple, `=Sum(Fields!FieldName.Value`), l'étendue de groupe par défaut pour une cellule est le groupe enfant le plus profond auquel il appartient. Lorsqu'une cellule appartient à la fois à des groupes de lignes et de colonnes, l'étendue correspond aux deux groupes les plus profonds. Vous pouvez également écrire des expressions qui calculent des sous-totaux agrégés étendus à un groupe relatif à un autre groupe de données. Par exemple, vous pouvez calculer le pourcentage d'un groupe par rapport au groupe de colonnes ou à toutes les données de la région de données (par exemple, `=Sum(Fields!FieldName.Value)/Sum(Fields!FieldName.Value,"ColumnGroup")`). Pour plus d’informations, consultez [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md)   
 [Ajouter un total à un groupe ou à une région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-total-to-a-group-or-tablix-data-region-report-builder-and-ssrs.md)   
 [Trier des données dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [Action d’exploration &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drilldown-action-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
