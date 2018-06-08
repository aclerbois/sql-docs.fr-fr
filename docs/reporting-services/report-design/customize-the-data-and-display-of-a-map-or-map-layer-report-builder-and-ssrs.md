---
title: Personnaliser des données et l’affichage d’une carte ou d’une couche (Générateur de rapports et SSRS) | Microsoft Docs
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
- "10521"
- sql13.rtp.rptdesigner.mapgroupproperties.filter.f1
- "10515"
- "10512"
- "10520"
- sql13.rtp.rptdesigner.shared.font.f1
- "10523"
- sql13.rtp.rptdesigner.mapgroupproperties.general.f1
- sql13.rtp.rptdesigner.shared.number.f1
- sql13.rtp.rptdesigner.shared.shadowdv.f1
- sql13.rtp.rptdesigner.mapgroupproperties.variables.f1
- "10507"
ms.assetid: fdd9b994-d138-4990-a291-279b0249eb72
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 2965210603f3727edd2563d5f0b9301e31efee0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs"></a>Personnaliser des données et l'affichage d'une carte ou d'une couche (Générateur de rapports et SSRS)
  Après avoir ajouté une carte ou une couche à un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] en utilisant un Assistant, vous pouvez modifier l’apparence de la carte dans le rapport. Vous pouvez apporter des améliorations en considérant les points suivants :  
  
-   Pour aider vos utilisateurs à interpréter l'affichage des données sur une carte, vous pouvez ajouter des légendes et une échelle de couleurs, ainsi que des étiquettes et des info-bulles.  
  
-   Pour faciliter la lecture de la carte, modifiez le centre et le niveau de zoom, ajoutez une échelle des distances et affichez une grille d'arrière-plan.  
  
-   Pour mieux contrôler le temps nécessaire au rendu du rapport lorsque vous l'exécutez, vous pouvez ajuster la résolution pour simplifier les éléments cartographiques.  
  
-   Vous pouvez incorporer des éléments cartographiques dans la définition de rapport, puis modifier la façon dont des éléments individuels s'affichent. Par exemple, vous pouvez afficher l'agence principale par une punaise et les autres agences par des cercles.  
  
-   Vous pouvez ajouter des régions personnalisées en spécifiant vos propres expressions de groupe de données.  
  
-   Vous pouvez ajouter un emplacement personnalisé à un point que vous spécifiez sur une couche qui inclut des points incorporés. Vous pouvez définir la valeur et les propriétés d'affichage pour des points personnalisés indépendamment d'autres points sur une couche de points.  
  
-   Pour fournir plus de détails, vous pouvez ajouter des liens vers des éléments cartographiques de chaque couche, sur lesquels un utilisateur peut cliquer pour ouvrir des rapports connexes.  
  
 Pour plus d’informations sur l’amélioration d’un rapport, consultez [Planification d’un rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md).  
  
 Les options d'affichage affectent la façon dont une carte ou les parties d'une carte apparaissent lorsque vous consultez le rapport. Certaines options contrôlent l'apparence de la carte, telle que les bordures et polices ou la zone représentée sur la carte. D'autres options contrôlent le contenu de chaque couche, telles que les tailles de bulle, types de marqueur, étiquettes ou info-bulles.  
  
 Un élément de rapport cartographique inclut les parties suivantes : la carte elle-même, un point de vue de la carte, un jeu de titres, un jeu de légendes (légende, échelle de couleurs et échelle des distances), un jeu de couches, un jeu d'éléments cartographiques sur chaque couche (polygones ou lignes ou points). Utilisez les informations des sections suivantes pour identifier la boîte de dialogue de propriétés qui contrôle les options d'affichage pour les différentes parties d'une carte.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="Map"></a> Modifier les options d'une carte  
 Sur un élément de rapport cartographique, vous pouvez contrôler les éléments suivants :  
  
-   ajouter plusieurs titres ;  
  
-   ajouter plusieurs légendes. Pour modifier le contenu d'une légende, vous devez créer des légendes supplémentaires, puis modifier les règles pour spécifier dans quelle légende vous devrez entrer les éléments de légende créés par chaque règle ;  
  
-   ajouter d'autres couches ;  
  
-   masquer ou afficher l'échelle des distances ou l'échelle de couleurs ;  
  
-   donner une illusion de profondeur en spécifiant une ombre.  
  
 Pour modifier ces options, cliquez avec le bouton droit sur la carte, cliquez sur **Carte**, puis modifiez les options.  
  
##  <a name="Viewport"></a> Modifier les options de la fenêtre d'affichage  
 Utilisez les options de la fenêtre d'affichage pour modifier la vue de la carte qui s'affiche dans votre rapport.  
  
 La source de données spatiales peut fournir une zone plus étendue que celle dont vous avez besoin dans le rapport. Vous pouvez utiliser la fenêtre d'affichage pour définir le centre et le niveau de zoom, et rogner la zone affichée dans la carte.  
  
 Les options suivantes peuvent être définies pour la fenêtre d'affichage :  
  
-   Choisissez le système de coordonnées et, pour un système de coordonnées géographiques, choisissez la projection.  
  
-   Choisissez le centre de la carte.  
  
-   Rognez la vue de la carte.  
  
-   Définissez le niveau de zoom.  
  
-   Résolution et simplification. Choisissez un compromis entre le temps de dessin et des plans détaillés pour les lignes et les polygones.  
  
 Pour modifier ces options, cliquez avec le bouton droit sur le point de vue de la carte, utilisez la page [Boîte de dialogue Propriétés du point de vue de la carte, Général](http://msdn.microsoft.com/library/6c9c773e-5c56-4571-95ed-8a157cfdfe52) et les pages connexes.  
  
##  <a name="Legends"></a> Modifier les options des légendes  
 Les légendes aident les utilisateurs à interpréter les données sur une carte.  
  
 Par défaut, toutes les règles que vous spécifiez pour une couche ajoutent des éléments à la première légende. De plus, toutes les règles de couleur affichent des valeurs dans l'échelle de couleurs.  
  
-   Pour modifier les options d'affichage déterminant l'apparence de la légende, notamment sa position par rapport à la fenêtre d'affichage, définissez les options sur la légende elle-même.  
  
-   Pour modifier le contenu ou le format du contenu d'une légende, modifiez les options de légende dans les règles correspondantes pour une couche.  
  
 Pour plus d’informations, consultez [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="Layer"></a> Modifier les options de la couche  
 Pour afficher les couches d'une carte, cliquez sur la carte pour la sélectionner. Le volet Carte s'affiche. Pour modifier des options pour une couche, cliquez avec le bouton droit sur la couche et utilisez le menu contextuel.  
  
 Une couche peut être de trois types, selon les données spatiales retournées par la source de données spatiales : couche de polygones, couche de lignes ou couche de points.  
  
 Les options suivantes peuvent être définies pour la couche :  
  
-   Données analytiques associées et champs de correspondance. La source des données spatiales apparaît sur le volet Carte sous le nom de la couche. Lorsque la source est incorporée, les éléments cartographiques de la couche font partie de la définition de rapport. Si la source n'est pas incorporée, les données spatiales sont récupérées au moment de l'exécution et le processeur de rapports crée les éléments cartographiques de la couche au moment du traitement du rapport. Pour modifier les options de données sur la couche, utilisez la page Données analytiques dans la boîte de dialogue Couche.  
  
-   Ordre de dessin des couches. L'ordre dans lequel les couches sont répertoriées dans le volet Carte est l'ordre de dessin inverse des couches. La dernière couche de la liste est dessinée en premier. Par exemple, si vous souhaitez que les points d'une couche de points s'affichent au-dessus des polygones de la couche de polygones, la couche de points doit être en premier et la couche de polygones en deuxième.  
  
-   Visibilité et transparence de la couche. Pour qu'une couche laisse transparaître une autre couche, définissez la transparence sur une valeur supérieure à 0. Une valeur de 100 % signifie que la couche n'est pas visible. Pour travailler sur une couche individuelle, vous pouvez facilement afficher ou masquer chaque couche individuelle en utilisant l’icône **Visibilité** dans le volet Carte. Vous pouvez également définir les options de niveau de zoom pour spécifier quand afficher ou masquer des éléments cartographiques sur la couche en fonction du niveau de zoom.  
  
-   Ajoutez une couche de mosaïques Bing pour le centre d'affichage et le niveau de zoom de la fenêtre actuelle. Pour une couche de mosaïques, vous n'avez pas besoin de spécifier de coordonnées géographiques. Les mosaïques sont chargées automatiquement pour correspondre à la zone de fenêtre d'affichage lorsque le système de coordonnées est Géographique, la projection est Mercator, les serveurs Bing Maps sont disponibles et le serveur de rapports a été configuré pour prendre en charge cette fonctionnalité. Pour chaque rapport, vous pouvez spécifier s'il convient d'utiliser une connexion sécurisée pour récupérer des mosaïques.  
  
 Pour plus d’informations sur les couches, consultez [Ajouter, modifier ou supprimer une carte ou une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
##  <a name="DataGrouping"></a> Modifier le groupement de données de la couche  
 Vous pouvez personnaliser la façon d'agréger des données spatiales pour vos propres formes. Pour définir les propriétés de groupe pour une couche, sélectionnez la couche dans le volet Carte et, dans le volet Propriétés de la couche, cliquez sur **Groupe**, puis sur les points de suspension (…) pour ouvrir les propriétés du groupe. Dans cette boîte de dialogue, vous pouvez spécifier des expressions de groupe, créer des variables de groupe et filtrer les données utilisées pour le regroupement.  
  
 L'expression de groupe spécifie comment les données analytiques en relation avec les données spatiales sont regroupées pour chaque élément cartographique de la couche. Par défaut, l'expression de groupe est le jeu de champs de correspondance spécifié pour la relation entre les données spatiales et les données analytiques. Par exemple, pour une carte à bulles affichant les villes et la taille de la population d'un pays ou d'une région, les champs de correspondance incluent le nom de ville [Ville] et le nom de région [Région] parce que plusieurs villes peuvent avoir le même nom. L'expression de groupe correspondante inclut deux champs : [Ville] et [Région].  
  
 Pour plus d’informations, consultez [Map Tips: How To Import Shapefiles Into SQL Server and Aggregate Spatial Data](http://go.microsoft.com/fwlink/?LinkID=214991).  
  
##  <a name="MapElements"></a> Modifier les options des éléments cartographiques sur la couche  
 Les éléments cartographiques sont les points, lignes ou polygones d'une couche basés sur les données spatiales. Pour les éléments cartographiques, les options suivantes peuvent être définies. Ces options s'appliquent à tous les éléments cartographiques de la couche, qu'ils soient incorporés ou non :  
  
-   étiquettes, visibilité des étiquettes, décalage des étiquettes et mise en forme ;  
  
-   bordures et remplissage ;  
  
-   actions d'extraction ;  
  
-   options d'affichage.  
  
 Les options d'affichage des éléments cartographiques suivent un ordre de priorité basé sur la couche, l'élément cartographique, les règles définies pour l'élément cartographique et les options de remplacement pour les éléments cartographiques incorporés.  
  
 Pour modifier ces options, cliquez avec le bouton droit sur l'élément cartographique, utilisez la boîte de dialogue de propriétés incorporées. Par exemple, pour un polygone incorporé, utilisez la boîte de dialogue Propriétés des polygones incorporés, la page Général et les pages associées.  
  
##  <a name="Precedence"></a> Ordre de priorité des options d'affichage  
 Pour contrôler l'apparence de l'affichage d'un point, d'une ligne ou d'un polygone sur une couche, vous devez savoir où les options d'affichage peuvent être définies et quelles options ont une priorité plus élevée. Les options d'affichage suivantes sont répertoriées dans l'ordre de priorité de la plus basse à la plus élevée. Les options d'affichage répertoriées plus haut remplacent les options d'affichage plus bas dans cette liste :  
  
-   Options relatives aux couches.  
  
-   Options relatives aux points, lignes ou polygones sur chaque couche. Ces options s'appliquent aussi bien si les éléments cartographiques sont récupérés dynamiquement lors du traitement du rapport que s'ils sont incorporés dans la définition de rapport. Par exemple, vous spécifiez une couleur de remplissage pour tous les éléments d'une couche.  
  
-   Règles. Vous pouvez définir des règles pour contrôler la couleur, la taille, la largeur ou le type de marqueur utilisés pour tous les éléments cartographiques d'une couche. Les règles que vous pouvez définir dépendent du type d'élément cartographique.  
  
    -   Règles de couleur. Appliquez ces règles aux points, aux lignes, aux polygones, ainsi qu'aux marqueurs utilisés pour les points centraux de polygone.  
  
    -   Règles de taille. Appliquez ces règles aux points et aux marqueurs utilisés pour les points centraux de polygone.  
  
    -   Règles de largeur. Appliquez ces règles aux largeurs de ligne.  
  
    -   Règles de type de marqueur. Appliquez ces règles aux points et aux marqueurs utilisés pour les points centraux de polygone.  
  
-   Options de remplacement pour des points, lignes ou polygones incorporés individuels sur une couche. Les modifications que vous apportez sont permanentes. Pour annuler ces modifications, vous devez recharger les données de la couche.  
  
 Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)   
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
  
