---
title: Ajouter, modifier ou supprimer une carte ou une couche (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.maplayerproperties.general.f1
- "10526"
- sql13.rtp.rptdesigner.mappolygonlayerproperties.general.f1
- "10529"
- "10525"
- "10535"
- sql13.rtp.rptdesigner.mappointlayerproperties.general.f1
- sql13.rtp.rptdesigner.shared.layerfilters.f1
- sql13.rtp.rptdesigner.maplinelayerproperties.general.f1
- "10524"
- sql13.rtp.rptdesigner.maptilelayerproperties.general.f1
- "10532"
- sql13.rtp.rptdesigner.maplayerproperties.analyticaldata.f1
- "10528"
- "10527"
- sql13.rtp.rptdesigner.shared.layervisibility.f1
ms.assetid: 6e89815e-187e-45bf-bf63-3d5c4a246360
caps.latest.revision: 15
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 8e627e570e310239c1973b8b24a44499b0bd47e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs"></a>Ajouter, modifier ou supprimer une carte ou une couche (Générateur de rapports et SSRS)
  Une carte est un ensemble de couches. Quand vous ajoutez une carte à un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , vous définissez la première couche. Vous pouvez créer des couches supplémentaires à l'aide de l'Assistant Couche.  
  
 Pour ajouter, supprimer ou modifier des options pour une couche, la méthode la plus simple consiste à utiliser l'Assistant Couche. Vous pouvez également modifier manuellement des options dans le volet Carte. Pour afficher le volet **Carte** , cliquez dans la carte sur l’aire de conception du rapport. L'illustration suivante affiche les différentes parties du volet :  
  
 ![rsMapLayerZone](../../reporting-services/report-design/media/rsmaplayerzone.gif "rsMapLayerZone")  
  
 Les couches sont dessinées de bas en haut, dans l'ordre dans lequel elles apparaissent dans le volet Carte. Dans l'illustration ci-dessus, la couche de mosaïques est dessinée en premier et la couche de polygones en dernier. Les couches dessinées ultérieurement peuvent masquer des éléments cartographiques de couches dessinées antérieurement. Vous pouvez modifier l'ordre des couches à l'aide des touches de direction de la barre d'outils du volet Carte. Pour afficher ou masquer des couches, activez ou désactivez l'icône de visibilité. Vous pouvez modifier la transparence d’une couche dans la page **Visibilité** de la boîte de dialogue des propriétés **Données de couche** .  
  
 Le tableau suivant présente les icônes de la barre d’outils du volet **Carte** .  
  
|Symbole|Description|Quand l’utiliser|  
|------------|-----------------|-----------------|  
|![rs_IconMapLayerWizard](../../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard")|Assistant Couche|Pour ajouter une couche à l'aide d’un Assistant, cliquez sur **Assistant Nouvelle couche**.|  
|![rs_IconMapAddLayer](../../reporting-services/media/rs-iconmapaddlayer.gif "rs_IconMapAddLayer")|Ajouter une couche|Pour ajouter une couche manuellement, cliquez sur **Ajouter une couche**, puis sur le type de couche à ajouter.|  
|![rs_IconMapPolygonLayer](../../reporting-services/report-design/media/rs-iconmappolygonlayer.gif "rs_IconMapPolygonLayer")|Couche de polygones|Ajoutez une couche qui affiche des zones ou des formes basées sur des jeux de coordonnées de polygones.|  
|![rs_IconMapLineLayer](../../reporting-services/report-design/media/rs-iconmaplinelayer.gif "rs_IconMapLineLayer")|Couche de lignes|Ajoutez une couche qui affiche des chemins ou des itinéraires basés sur des jeux de coordonnées de lignes.|  
|![rs_IconMapPointLayer](../../reporting-services/report-design/media/rs-iconmappointlayer.gif "rs_IconMapPointLayer")|Couche de points|Ajoutez une couche qui affiche des emplacements basés sur des jeux de coordonnées de points.|  
|![rs_IconMapTileLayer](../../reporting-services/report-design/media/rs-iconmaptilelayer.gif "rs_IconMapTileLayer")|Couche de mosaïques|Ajoutez une couche qui affiche des mosaïques Bing correspondant à la zone de la vue cartographique actuelle telle que définie par la fenêtre d'affichage.|  
  
 La zone de la vue cartographique est affichée au bas du volet Carte. Pour modifier le centre ou les options de zoom de la carte, utilisez les touches de direction pour ajuster le centre d'affichage et le curseur pour ajuster le niveau de zoom.  
  
 Pour plus d’informations sur les couches, consultez [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="AddLayer"></a> Pour ajouter une couche à partir de l’Assistant Couche  
  
-   Dans le ruban, dans le menu **Insérer** , cliquez sur **Carte**, puis sur **Carte Wizard**. L'Assistant vous permet d'ajouter une couche à la carte existante. La plupart des pages des Assistants Carte et Couche sont identiques.  
  
     Pour plus d’informations, consultez [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md).  
  
##  <a name="ChangeLayer"></a> Pour modifier les options d’une couche à l’aide de l’Assistant Couche  
  
-   Exécutez l'Assistant Couche. Cet Assistant vous permet de modifier des options pour une couche que vous avez créée à l'aide de l'Assistant Couche. Dans le volet Carte, cliquez avec le bouton droit sur la couche, puis dans la barre d’outils, cliquez sur le bouton de l’Assistant Couche (![rs_IconMapLayerWizard](../../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard")).  
  
     Pour plus d’informations, consultez [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md).  
  
##  <a name="AddVectorLayer"></a> Pour ajouter une couche de points, de lignes ou de polygones à partir de la barre d'outils du volet Carte  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Dans la barre d’outils, cliquez sur le bouton **Ajouter une couche** puis, dans la liste déroulante, cliquez sur le type de couche que vous voulez ajouter : **Point**, **Ligne**ou **Polygone**.  
  
    > [!NOTE]  
    >  Bien qu'il soit possible d'ajouter une couche et de la configurer manuellement, nous vous recommandons d'utiliser l'Assistant Couche pour ajouter de nouvelles couches. Pour lancer l’Assistant à partir de la barre d’outils du volet Carte, cliquez sur le bouton de l’Assistant Couche (![rs_IconMapLayerWizard](../../reporting-services/media/rs-iconmaplayerwizard.gif "rs_IconMapLayerWizard")).  
  
3.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de couche**.  
  
4.  Dans **Utiliser les données spatiales de**, sélectionnez la source de données spatiales. Les options varient selon votre sélection.  
  
     Si vous souhaitez visualiser des données analytiques de votre rapport sur cette couche, procédez comme suit :  
  
    1.  Cliquez sur **Données analytiques**.  
  
    2.  Dans **Dataset analytique**, cliquez sur le nom du dataset qui contient les données analytiques et les champs de correspondance pour générer une relation entre des données analytiques et spatiales.  
  
    3.  Cliquez sur **Ajouter**.  
  
    4.  Tapez le nom du champ de correspondance du dataset spatial.  
  
    5.  Tapez le nom du champ de correspondance du dataset analytique.  
  
     Pour plus d’informations sur la liaison des données spatiales et analytiques, consultez [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="FilterAnalyticalData"></a> Pour filtrer des données analytiques pour la couche  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Dans le volet Carte, cliquez avec le bouton droit sur la couche, puis cliquez sur  **Données de couche**.  
  
3.  Cliquez sur **Filtres**.  
  
4.  Définissez une équation de filtre pour limiter les données analytiques utilisées dans l'affichage de la carte. Pour plus d’informations, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
##  <a name="PointProperties"></a> Pour contrôler les propriétés des points d'une couche de points ou pour les points centraux de polygones  
  
1.  Sélectionnez **Général** dans la boîte de dialogue **Propriétés des points de la carte** pour modifier les options d’étiquette, d’info-bulle et de type de marqueur pour les éléments cartographiques suivants :  
  
    -   Tous les points dynamiques ou incorporés sur une couche de points. Les règles de couleur, de taille et de type de marqueur relatives aux points remplacent ces options. Pour remplacer les options d’un point incorporé spécifique, utilisez la page [Boîte de dialogue Propriétés des points incorporés de la carte, Marqueur](http://msdn.microsoft.com/library/3c5eb1c5-d40a-424f-aa7c-43b112f42dec) .  
  
    -   Le point central de tous les polygones dynamiques ou incorporés sur une couche de polygones. Les règles de couleur, de taille et de type de marqueur relatives aux points centraux remplacent ces options. Pour remplacer les options d’un point central spécifique, utilisez la page [Boîte de dialogue Propriétés des points incorporés de la carte, Marqueur](http://msdn.microsoft.com/library/3c5eb1c5-d40a-424f-aa7c-43b112f42dec) .  
  
##  <a name="Embedded"></a> Pour spécifier des données incorporées comme source de données spatiales  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de couche**.  
  
3.  Dans **Utiliser les données spatiales de**, sélectionnez **Données incorporées dans le rapport**.  
  
4.  Pour charger les éléments cartographiques d’un rapport existant ou pour créer des éléments cartographiques à partir d’un fichier ESRI, cliquez sur **Parcourir**, pointez sur le fichier, puis cliquez sur **Ouvrir**. Les éléments cartographiques sont incorporés dans cette définition de rapport. Les données spatiales sur lesquelles vous pointez doivent correspondre au type de couche. Par exemple, pour une couche de points, vous devez pointer sur les données spatiales qui spécifient des jeux de coordonnées de points.  
  
5.  Dans **Champ spatial**, spécifiez le nom du champ qui contient les données spatiales. Vous devrez peut-être déterminer ce nom à partir de la source de données spatiales.  
  
    > [!NOTE]  
    >  Si vous ne connaissez pas le nom du champ et que vous avez ouvert un fichier de forme ESRI, utilisez plutôt l’option **Lien vers le fichier de forme ESRI** .  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="ESRI"></a> Pour spécifier un fichier de forme ESRI comme source de données spatiales  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de couche**.  
  
3.  Dans **Utiliser les données spatiales de**, sélectionnez **Lien vers le fichier de forme ESRI**.  
  
4.  Dans **Nom de fichier**, tapez l’emplacement d’un fichier de forme ESRI ou cliquez sur **Parcourir** pour sélectionner un fichier de forme ESRI.  
  
    > [!NOTE]  
    >  Si le fichier de forme se trouve sur votre ordinateur local, les données spatiales sont incorporées dans la définition de rapport. Pour récupérer les données dynamiquement lors du traitement du rapport, vous devez télécharger le fichier ESRI .shp et son fichier de support .dbf sur le serveur de rapports. Pour plus d’informations, consultez « Procédure : télécharger un fichier ou un rapport (Gestionnaire de rapports) » dans la [section Reporting Services](http://go.microsoft.com/fwlink/?linkid=121312) de la documentation en ligne SQL Server.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="DatasetField"></a> Pour spécifier un champ de dataset de rapport comme source de données spatiales  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de couche**.  
  
3.  Dans **Utiliser les données spatiales de**, sélectionnez **Champ spatial dans un dataset**.  
  
4.  Dans **Nom du dataset**, cliquez sur le nom d’un dataset du rapport qui contient les données spatiales souhaitées.  
  
5.  Dans **Nom du champ spatial**, cliquez sur le nom du champ du dataset qui contient les données spatiales.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="TileLayer"></a> Pour ajouter une couche de mosaïques  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Dans la barre d’outils, cliquez sur le bouton **Ajouter une couche** puis, dans la liste déroulante, cliquez sur **Couche de mosaïques**.  
  
    > [!NOTE]  
    >  Pour plus d'informations sur l'utilisation de mosaïques Bing dans votre rapport, consultez [Conditions supplémentaires d'utilisation](http://go.microsoft.com/fwlink/?LinkId=151371).  
  
3.  Cliquez avec le bouton droit sur la couche de mosaïques dans le volet Carte, puis sélectionnez **Propriétés des mosaïques**.  
  
4.  Dans **Options de mosaïque**, sélectionnez un style de mosaïque. Si les mosaïques Bing sont disponibles, la couche sur l'aire de conception est mise à jour avec le style que vous sélectionnez.  
  
    > [!NOTE]  
    >  Une couche de mosaïques peut également être ajoutée lorsque vous ajoutez une couche de polygones, de lignes ou de points dans l'Assistant Carte ou Couche. Dans la page **Choisir des options de vue cartographique et de données spatiales** , sélectionnez l’option **Ajouter un arrière-plan Bing Maps pour cette vue cartographique**.  
  
##  <a name="DrawingOrder"></a> Pour modifier l'ordre de dessin d'une couche  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez sur la couche dans le volet Carte pour la sélectionner.  
  
3.  Dans la barre d'outils du volet Carte, cliquez sur la flèche Haut ou Bas pour modifier l'ordre de dessin de chaque couche.  
  
##  <a name="Transparency"></a> Pour modifier la transparence d'une couche de polygones, de lignes ou de points  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de couche**.  
  
3.  Cliquez sur **Visibilité**.  
  
4.  Dans **Options de transparence**, tapez une valeur qui représente le pourcentage de transparence, par exemple **40**. Un facteur de transparence de zéro (0 %) signifie que la couche est opaque. Un facteur de transparence de 100 % signifie que vous ne verrez pas la couche dans le rapport.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="TileTransparency"></a> Pour modifier la transparence d'une couche de mosaïques  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche, puis cliquez sur **Propriétés des mosaïques**.  
  
3.  Cliquez sur **Visibilité**.  
  
4.  Dans **Options de transparence**, tapez une valeur qui représente le pourcentage de transparence, par exemple **40**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="Secure"></a> Pour spécifier une connexion sécurisée pour une couche de mosaïques  
  
1.  Cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Dans le volet Carte, cliquez sur la couche de mosaïques pour la sélectionner. Le volet Propriétés affiche les propriétés de la couche de mosaïques.  
  
3.  Dans le volet Propriétés, attribuez à UseSecureConnection la valeur **True**.  
  
 La connexion au service Web Bing Maps utilise le service HTTP SSL (Secure Sockets Layer) pour récupérer des mosaïques Bing pour cette couche.  
  
##  <a name="Language"></a> Pour spécifier la langue des étiquettes de mosaïque  
  
1.  Par défaut, pour les styles de mosaïque qui affichent des étiquettes, la langue est déterminée en fonction des paramètres régionaux par défaut définis pour le Générateur de rapports. Vous pouvez personnaliser le paramètre de langue pour les étiquettes de mosaïque des façons suivantes.  
  
    -   Cliquez sur la carte à l'extérieur de la fenêtre d'affichage afin de sélectionner la carte. Dans le volet Propriétés, pour la propriété TileLanguage, sélectionnez une valeur de culture dans la liste déroulante.  
  
    -   Cliquez sur l'arrière-plan du rapport pour sélectionner le rapport. Dans le volet Propriétés, pour la propriété Language, sélectionnez une valeur de culture dans la liste déroulante.  
  
     L’ordre de priorité pour la définition de la langue des étiquettes de mosaïque est le suivant : propriété de rapport Language, paramètres régionaux par défaut définis pour le Générateur de rapports et propriété de carte TileLanguage.  
  
##  <a name="ConditionalHide"></a> Pour masquer de manière conditionnelle une couche en fonction du niveau de zoom de la fenêtre d'affichage  
  
1.  Définissez les options de **Visibilité** pour contrôler l’affichage d’une couche.  
  
    -   Dans le volet Couches, cliquez avec le bouton droit sur une couche pour la sélectionner puis, dans la barre d’outils Couches, cliquez sur Propriétés pour ouvrir **Propriétés de la couche**.  
  
    -   Cliquez sur **Visibilité**.  
  
    -   Dans Visibilité de la couche, sélectionnez **Afficher ou masquer en fonction d’une valeur de zoom**.  
  
    -   Entrez les valeurs de zoom maximale et minimale pour l'affichage de la couche.  
  
    -   Facultatif. Entrez une valeur pour la transparence.  
  
     Vous pouvez également masquer la couche de manière conditionnelle. Pour plus d’informations, consultez [Masquer un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/hide-an-item-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Résoudre les problèmes liés aux rapports : rapports cartographiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)  
  
  
