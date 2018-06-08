---
title: Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques | Microsoft Docs
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
- "10538"
- "10537"
- sql13.rtp.rptdesigner.mapembeddedpolygonlayerproperties.general.f1
- MICROSOFT.REPORTDESIGNER.MAPMARKER.MARKERSTYLE
- sql13.rtp.rptdesigner.mapembeddedlinelayerproperties.general.f1
- sql13.rtp.rptdesigner.mapembeddedpointlayerproperties.general.f1
- "10531"
- "10536"
- sql13.rtp.rptdesigner.maplinelayerproperties.widthrules.f1
ms.assetid: 7f1f5584-37b4-4fa2-ae44-8988c5f0c744
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 3e78b0319639852d8bb4cac5be3f3b2157ac0703
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="vary-polygon-line-and-point-display-by-rules-and-analytical-data"></a>Vary Polygon, Line, and Point Display by Rules and Analytical Data
  Les options d'affichage pour les polygones, les lignes et les points d'une couche sont contrôlées en définissant des options pour la couche, en établissant des règles pour les éléments cartographiques de la couche ou en remplaçant des options pour des éléments cartographiques incorporés spécifiques sur une couche.  
  
 Les options d'affichage sont appliquées avec une priorité spécifique, répertoriée de la priorité la plus basse à la plus élevée :  
  
1.  Les options définies sur une couche de polygones, de lignes et de points s'appliquent à tous les éléments cartographiques de cette couche, qu'ils soient ou non incorporés dans la définition de rapport.  
  
2.  Les options définies pour les règles s'appliquent à tous les éléments cartographiques d'une couche. Toutes les options de visualisation des données s'appliquent uniquement aux éléments cartographiques associés aux données spatiales. Une option de visualisation des données requiert la spécification d'un champ de données sur lequel les variations d'affichage seront basées. Des champs de correspondance doivent être définis pour les données analytiques et spatiales avant de pouvoir appliquer des règles de visualisation des données. Pour plus d’informations, consultez [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
3.  Options que vous définissez pour des éléments cartographiques incorporés sélectionnés. Notez que lorsque vous remplacez les options de couche, les modifications que vous apportez à la définition de rapport sont permanentes. Vous pouvez modifier les valeurs des champs de données et aussi remplacer les options d'affichage pour personnaliser la façon dont des polygones, lignes et points spécifiques s'affichent sur une couche.  
  
 Outre le contrôle de l'affichage d'éléments cartographiques sur une couche, vous pouvez contrôler la transparence des couches pour permettre à des couches dessinées antérieurement d'être visibles à travers les couches dessinées ultérieurement. Pour plus d’informations sur la modification des options qui affectent la carte ou la couche dans son ensemble, consultez [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="Rules"></a> Fonctionnement des règles  
 Vous pouvez définir quatre types de règles permettant au processeur de rapports d'ajuster automatiquement les propriétés d'affichage des éléments cartographiques sur une couche. Les règles varient en fonction du type d'élément cartographique : polygones, lignes ou points.  
  
-   **Polygones.** Modifiez la couleur des polygones.  
  
    -   **Points centraux de polygone.** Pour les marqueurs affichés au point central de chaque polygone, modifiez la couleur, la taille et le type de marqueur.  
  
-   **Lignes.** Modifiez la couleur et la largeur de ligne.  
  
-   **Points.** Pour les marqueurs affichés pour chaque point, modifiez la couleur, la taille et le type de marqueur.  
  
##  <a name="Color"></a> Fonctionnement des règles de couleur  
 Les règles de couleur s'appliquent aux couleurs de remplissage des polygones, des lignes et des marqueurs qui représentent des points ou des points centraux de polygone.  
  
 Les règles de couleur prennent en charge quatre options :  
  
-   Appliquer un style de modèle. Le thème que vous avez choisi dans l'Assistant définit le style de modèle de la couche. Le thème définit le style de police et de bordure et la palette.  
  
-   Visualiser les données à l'aide de la palette de couleurs. Vous spécifiez une palette par son nom. Le processeur de rapports définit la couleur de chaque élément cartographique dans une couche en exécutant pas à pas chaque couleur de la palette et en appliquant successivement des tons plus clairs de chaque couleur.  
  
-   Visualiser les données à l'aide de plages de couleurs. Vous spécifiez une couleur de début, intermédiaire et de fin. Vous spécifiez ensuite des options de distribution. Le processeur de rapports utilise les valeurs de l'option de distribution pour créer un jeu de couleurs produisant un affichage semblable à une carte de chaleur. Une carte de chaleur affiche des couleurs en fonction de la température. Par exemple, sur une échelle de 0 à 100, la carte utilise le bleu pour représenter les valeurs basses (froid) et le rouge pour représenter les valeurs élevées (chaud).  
  
-   Visualiser les données à l'aide de couleurs personnalisées. Vous spécifiez un jeu de couleurs. Le processeur de rapports définit la couleur de chaque élément cartographique de la couche en exécutant pas à pas les valeurs que vous spécifiez.  
  
 La palette par défaut inclut la couleur blanche. Pour éviter le contraste entre le blanc et d'autres couleurs de la palette, spécifiez une couleur de début claire dans la palette.  
  
 Pour afficher des éléments cartographiques qui ne sont pas associés aux données comme incolores, définissez la couleur par défaut sur **Aucune couleur** pour ces éléments cartographiques de la couche.  
  
### <a name="color-scale"></a>Échelle de couleurs  
 Par défaut, toutes les valeurs des règles de couleur figurent dans l'échelle de couleurs, en plus d'être affichées dans la première légende. L'échelle de couleurs est conçue pour afficher une plage de couleurs. Choisissez les données les plus importantes à afficher dans l'échelle de couleurs.  
  
 Pour supprimer des valeurs indésirables de l'échelle de couleurs, effacez l'option d'échelle de couleurs pour chaque règle de couleur sur chaque couche.  
  
##  <a name="Width"></a> Fonctionnement des règles de largeur de ligne  
 Les règles de largeur s'appliquent aux lignes. Les règles de largeur prennent en charge deux options :  
  
-   Utiliser une largeur de ligne par défaut. Vous spécifiez la largeur de ligne en points.  
  
-   Visualiser les données à l'aide de la largeur de ligne. Vous définissez les largeurs minimale et maximale de la ligne, spécifiez le champ de données à utiliser pour faire varier la largeur, puis spécifiez les options de distribution à appliquer à ces données.  
  
##  <a name="Size"></a> Fonctionnement des règles de taille de marqueur  
 Les règles de taille s'appliquent aux marqueurs qui représentent des points ou des points centraux de polygone. Les règles de taille prennent en charge deux options :  
  
-   Utiliser une taille de marqueur par défaut. Vous spécifiez la taille en points.  
  
-   Visualiser les données à l'aide de la taille. Vous définissez les tailles minimale et maximale du marqueur, spécifiez le champ de données à utiliser pour faire varier la taille, puis spécifiez les options de distribution à appliquer à ces données.  
  
##  <a name="Marker"></a> Fonctionnement des règles de type de marqueur  
 Les règles de type de marqueur s'appliquent aux marqueurs qui représentent des points ou des points centraux de polygone. Les règles de type de marqueur prennent en charge deux options :  
  
-   Utiliser un type de marqueur par défaut. Vous spécifiez l'un des types de marqueur disponibles.  
  
-   Visualiser les données à l'aide de marqueurs. Vous spécifiez un jeu de marqueurs et l'ordre dans lequel vous souhaitez les utiliser. Les types de marqueurs incluent **Cercle**, **Losange**, **Pentagone**, **Punaise**, **Rectangle**, **Étoile**, **Triangle**, **Trapèze**et **Secteur**.  
  
##  <a name="Distribution"></a> Fonctionnement des options de distribution  
 Pour créer une distribution de valeurs, vous pouvez diviser vos données en plages. Vous spécifiez le type de distribution, le nombre de sous-plages et les valeurs de plage minimale et maximale.  
  
 Dans la liste suivante, supposez que vous avez trois éléments cartographiques et six valeurs analytiques connexes qui varient de 1 à 9 999 avec les valeurs suivantes : 1, 10, 200, 2 000, 4 777, 8 999.  
  
-   **EqualInterval.** Créez des plages qui divisent les données en intervalles de plage égaux. Pour l'exemple, les trois plages seraient 0-2999, 3000-5999, 6000-8999. Sous-plage 1 : 1, 10, 200, 500. Sous-plage 2 : 4777. Sous-plage 3 : 8999. Cette méthode ne prend pas en considération la distribution des données. Les valeurs très élevées ou très basses peuvent dénaturer les résultats de distribution.  
  
-   **EqualDistribution.** Créez des plages qui divisent les données de sorte que chaque plage ait un même nombre d'éléments. Dans l'exemple de données, les trois plages seraient 0-10, 11-500 et 501-8999. Sous-plage 1 : 1, 10. Sous-plage 2 : 200, 500. Sous-plage 3 : 4777, 8999. Cette méthode peut déformer la distribution en créant des divisions couvrant des plages très grandes ou très petites.  
  
-   **Optimal.** Créez des plages qui ajustent automatiquement la distribution pour créer des sous-plages équilibrées. Le nombre de sous-plages est déterminé par l'algorithme.  
  
-   **Custom.** Spécifiez votre propre nombre de plages pour contrôler la distribution de valeurs. Pour les données d'exemple, vous pouvez spécifier 3 plages : 1-2, 3-8, 9.  
  
 Les valeurs de distribution sont utilisées par les règles pour faire varier les valeurs d'affichage des éléments cartographiques.  
  
##  <a name="Legends"></a> Fonctionnement des légendes et éléments de légende  
 Les éléments de légende sont créés automatiquement à partir des règles que vous spécifiez pour chaque couche. Les options des règles contrôlent le nombre d'éléments créés et la légende dans laquelle ils apparaissent. Par défaut, tous les éléments de toutes les règles sont ajoutés à la première légende. Pour déplacer des éléments en dehors de la première légende, créez autant de légendes supplémentaires que nécessaire et pour chaque règle, spécifiez la légende à utiliser pour afficher les éléments résultant de la règle. Pour masquer des éléments selon une règle, spécifiez un nom de légende vierge.  
  
 Pour contrôler l'emplacement d'affichage d'une légende, utilisez la boîte de dialogue Propriétés de la légende pour spécifier une position par rapport au point de vue de la carte. Pour plus d’informations, consultez [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
 Les légendes sont automatiquement développées pour afficher le titre ou le texte de la légende. Pour mettre en forme le texte des éléments de la légende, utilisez des mots clés de légende de carte et des mises en forme personnalisées. Pour plus d’informations, consultez [Pour modifier le format du contenu d’une légende](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md#ChangeFormatItems).  
  
 Les tableaux suivants illustrent des exemples de formats différents que vous pouvez utiliser.  
  
|Mot clé et format|Description|Exemple de texte apparaissant dans la légende|  
|------------------------|-----------------|---------------------------------------------------|  
|`#FROMVALUE {C0}`|Affiche la monnaie de la valeur totale sans décimales.|400 $|  
|`#FROMVALUE {C2}`|Affiche la monnaie de la valeur totale avec deux décimales.|400,55 $|  
|`#TOVALUE`|Affiche la valeur numérique réelle du champ de données.|10000|  
|`#FROMVALUE{N0} - #TOVALUE{N0}`|Affiche les valeurs numériques réelles du début et de la fin de la plage.|10 - 790|  
  
## <a name="see-also"></a> Voir aussi  
 [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md)   
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)  
  
  
