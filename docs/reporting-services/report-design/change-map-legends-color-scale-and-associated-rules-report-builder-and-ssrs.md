---
title: Changer les légendes de carte, l’échelle de couleurs et les règles associées dans le Générateur de rapports et SSRS | Microsoft Docs
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
- sql13.rtp.rptdesigner.mapcolorscaleproperties.labels.f1
- sql13.rtp.rptdesigner.mappointlayerproperties.typerules.f1
- sql13.rtp.rptdesigner.shared.maprulesdistribution.f1
- "10512"
- "10539"
- "10533"
- sql13.rtp.rptdesigner.maplegendtitleproperties.general.f1
- "10534"
- "10516"
- sql13.rtp.rptdesigner.mapdistancescaleproperties.general.f1
- sql13.rtp.rptdesigner.mapcolorscaleproperties.general.f1
- sql13.rtp.rptdesigner.mapcolorscaletitleproperties.general.f1
- "10514"
- sql13.rtp.rptdesigner.mappointlayerproperties.sizerules.f1
- "10513"
- sql13.rtp.rptdesigner.shared.mapruleslegend.f1
- sql13.rtp.rptdesigner.shared.embeddedlabels.f1
- "10510"
- "10509"
- sql13.rtp.rptdesigner.maplegendproperties.general.f1
- "10540"
- "10517"
ms.assetid: a1d691b2-c5ae-420f-af60-b7c54a7385a4
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 96de567e40763ca1db0b2aae8f13af3d8a17ffb5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs"></a>Modifier les légendes de carte, l'échelle de couleurs et les règles associées (Générateur de rapports et SSRS)
  Dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , une carte peut contenir des légendes de carte, une échelle de couleurs et une échelle des distances. Ces parties d'une carte aident les utilisateurs à interpréter la visualisation des données sur la carte.  
  
 Les légendes incluent les parties suivantes d'une carte :  
  
-   **Légende de carte** Affiche un guide pour faciliter l'interprétation des données analytiques qui font varier l'affichage d'un élément cartographique sur une couche. Une carte peut avoir plusieurs légendes. Pour chaque couche, vous devez spécifier la légende à utiliser. Une légende peut fournir un guide pour plusieurs couches.  
  
-   **Échelle de couleurs** Affiche un guide pour faciliter l'interprétation des couleurs sur la carte. Une carte a une seule échelle de couleurs. Plusieurs couches peuvent fournir les données de l'échelle de couleurs.  
  
-   **Échelle des distances** Affiche un guide pour faciliter l'interprétation de l'échelle de la carte. Une carte a une seule échelle des distances. La valeur de zoom du point de vue de la carte active détermine l'échelle des distances.  
  
 ![rs_MapElements](../../reporting-services/report-design/media/rs-mapelements.gif "rs_MapElements")  
  
##  <a name="Viewport"></a> Pour modifier la position d'une légende par rapport à la fenêtre d'affichage  
  
#### <a name="to-change-the-position-of-a-legend-relative-to-the-viewport"></a>Pour modifier la position d'une légende par rapport à la fenêtre d'affichage  
  
1.  En mode Conception, cliquez avec le bouton droit sur la légende et ouvrez la page *\<***Propriétés de l’élément de rapport**.  
  
2.  Dans **Position**, cliquez sur l'emplacement qui spécifie où afficher la légende par rapport à la fenêtre d'affichage.  
  
3.  Pour afficher la légende à l’extérieur de la fenêtre d’affichage, sélectionnez **Afficher \<élément de rapport> en dehors de la fenêtre d’affichage**.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Dans l'aperçu, les légendes de carte et l'échelle de couleurs s'affichent uniquement lorsqu'il existe des résultats des règles connexes à cette légende. S'il n'y a pas d'éléments à afficher, la légende ne s'affiche pas dans le rapport rendu.  
  
##  <a name="MapLegend"></a> Pour modifier la disposition d'une légende de carte  
  
#### <a name="to-change-the-layout-of-a-map-legend"></a>Pour modifier la disposition d'une légende de carte  
  
1.  En mode Conception, cliquez avec le bouton droit sur la légende et ouvrez la page **Propriétés de la légende** .  
  
2.  Dans **Disposition de la légende**, cliquez sur la disposition de table que vous souhaitez utiliser pour la légende. Lorsque vous cliquez sur différentes options, la disposition sur l'aire de conception change.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="MapLegendTitle"></a> Pour afficher ou masquer un titre de légende de carte  
  
#### <a name="to-show-or-hide-a-map-legend-title"></a>Pour afficher ou masquer un titre de légende de carte  
  
-   Cliquez avec le bouton droit sur la légende de carte sur l’aire de conception, puis cliquez sur **Afficher le titre de la légende**.  
  
##  <a name="ColorScaleTitle"></a> Pour afficher ou masquer un titre d'échelle de couleurs  
  
#### <a name="to-show-or-hide-a-color-scale-title"></a>Pour afficher ou masquer un titre d'échelle de couleurs  
  
-   Cliquez avec le bouton droit sur l’échelle de couleurs sur l’aire de conception, puis cliquez sur **Afficher le titre de l’échelle de couleurs**.  
  
##  <a name="MoveItems"></a> Pour déplacer des éléments hors de la première légende  
 Créez autant de légendes supplémentaires que nécessaire, puis mettez à jour les règles de chaque couche en spécifiant la légende dans laquelle afficher les résultats des règles.  
  
#### <a name="to-create-a-new-legend"></a>Pour créer une légende  
  
-   En mode Conception, cliquez avec le bouton droit sur la carte à l’extérieur du point de vue de la carte, puis cliquez sur **Ajouter une légende**.  
  
     Une nouvelle légende apparaît sur la carte.  
  
#### <a name="to-display-rule-results-in-a-legend"></a>Pour afficher des résultats des règles dans une légende  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Légende**.  
  
4.  Dans la liste déroulante **Afficher dans cette légende** , cliquez sur le nom de la légende dans laquelle afficher les résultats des règles.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="TemplateStyle"></a> Pour faire varier les couleurs d'éléments cartographiques en fonction d'un style de modèle  
  
#### <a name="to-vary-map-element-colors-based-on-a-template-style"></a>Pour faire varier les couleurs d'éléments cartographiques en fonction d'un style de modèle  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Appliquer le style du modèle**.  
  
     Un style de modèle spécifie la police, le style de bordure et la palette de couleurs. À chaque élément cartographique est attribuée une couleur différente de la palette de couleurs correspondant au thème spécifié dans l'Assistant Carte ou Couche. Il s'agit de la seule option s'appliquant aux couches qui n'ont pas de données analytiques associées.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="ColorPalette"></a> Pour faire varier les couleurs d'éléments cartographiques en fonction de la palette de couleurs  
  
#### <a name="to-vary-map-element-colors-based-on-color-palette"></a>Pour faire varier les couleurs d'éléments cartographiques en fonction de la palette de couleurs  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Visualiser les données à l'aide de la palette de couleurs**.  
  
     Cette option utilise une palette intégrée ou personnalisée que vous spécifiez. En fonction des données analytiques connexes, une couleur différente ou un ton de couleur différent de la palette est attribué à chaque élément cartographique.  
  
4.  Dans **Champ de données**, tapez le nom du champ contenant les données analytiques que vous souhaitez visualiser à l'aide de couleurs.  
  
5.  Dans **Palette**, sélectionnez le nom de la palette à utiliser dans la liste déroulante.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="ColorRanges"></a> Pour faire varier les couleurs d'éléments cartographiques en fonction des plages de couleurs  
  
#### <a name="to-vary-map-element-colors-based-on-color-ranges"></a>Pour faire varier les couleurs d'éléments cartographiques en fonction des plages de couleurs  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Visualiser les données à l'aide de plages de couleurs**.  
  
     Cette option, combinée aux couleurs de début, intermédiaire et de fin que vous spécifiez dans cette page et aux options que vous spécifiez dans la page **Distribution** , divise les données analytiques connexes en plages. Le processeur de rapports attribue la couleur appropriée à chaque élément cartographique en fonction de ses données associées et de la plage dans laquelle il est situé.  
  
4.  Dans **Champ de données**, tapez le nom du champ contenant les données analytiques que vous souhaitez visualiser à l'aide de couleurs.  
  
5.  Dans **Couleur de début**, spécifiez la couleur à utiliser pour la plage la plus basse.  
  
6.  Dans **Couleur intermédiaire**, spécifiez la couleur à utiliser pour la plage centrale.  
  
7.  Dans **Couleur de fin**, spécifiez la couleur à utiliser pour la plage la plus élevée.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="CustomColors"></a> Pour faire varier les couleurs d'éléments cartographiques en fonction de couleurs personnalisées  
  
#### <a name="to-vary-map-element-colors-based-on-custom-colors"></a>Pour faire varier les couleurs d'éléments cartographiques en fonction de couleurs personnalisées  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Visualiser les données à l'aide de couleurs personnalisées**.  
  
     Cette option utilise une liste de couleurs que vous spécifiez. En fonction des données analytiques connexes, une couleur de la liste est affectée à chaque élément cartographique. S'il y a plus d'éléments cartographiques que de couleurs, aucune couleur n'est attribuée.  
  
4.  Dans **Champ de données**, tapez le nom du champ contenant les données analytiques que vous souhaitez visualiser à l'aide de couleurs.  
  
5.  Dans **Couleurs personnalisées**, cliquez sur **Ajouter** pour spécifier chaque couleur personnalisée.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="DistributionOptions"></a> Pour définir des options de distribution pour une légende  
  
#### <a name="to-set-distribution-options-for-a-legend"></a>Pour définir des options de distribution pour une légende  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Sélectionnez l’option **Visualiser les données à l’aide de** \<type de règle>. Pour utiliser des options de distribution, vous devez créer des plages dans la page **Distribution** en fonction des données analytiques associées à la couche.  
  
4.  Cliquez sur **Distribution**.  
  
5.  Sélectionnez l'un des types de distribution suivants :  
  
    -   **EqualInterval**. Spécifie des plages qui divisent les données en intervalles de plages égaux.  
  
    -   **EqualDistribution**. Spécifie des plages qui divisent les données de sorte que toutes les plages ont le même nombre d'éléments.  
  
    -   **Optimal**. Spécifie des plages qui ajustent automatiquement la distribution pour créer des sous-plages équilibrées.  
  
    -   **Personnalisée**. Spécifiez votre propre nombre de plages pour contrôler la distribution de valeurs.  
  
     Pour plus d’informations sur les options de distribution, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
6.  Dans **Nombre de sous-plages**, tapez le nombre de sous-plages à utiliser. Lorsque le type de distribution est **Optimal**, le nombre de sous-plages est calculé automatiquement.  
  
7.  Dans **Début de plage**, tapez une valeur de plage minimale. Toutes les valeurs inférieures à ce nombre sont égales à la valeur de plage minimale.  
  
8.  Dans **Fin de plage**, tapez une valeur de plage maximale. Toutes les valeurs supérieures à ce nombre sont égales à la valeur de plage maximale.  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="RuleLegend"></a> Pour modifier le contenu d'une légende de règle  
  
#### <a name="to-change-the-contents-of-a-color-size-width-or-marker-type-legend"></a>Pour modifier le contenu d'une légende de couleur, de taille, de largeur ou de type de marqueur  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle **de type d’élément cartographique>***.  
  
3.  Vérifiez que l’option **Visualiser les données à l’aide de** \<*type de règle*> est sélectionnée.  
  
4.  Dans **Champ de données**, vérifiez que les données analytiques que vous visualisez sur la couche sont sélectionnées.  
  
    > [!NOTE]  
    >  Si aucun champ ne s’affiche dans la liste déroulante, cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de la couche** pour ouvrir la boîte de dialogue Propriétés des données de la couche de la carte à la page Données analytiques et vérifier que vous avez spécifié des données analytiques pour cette couche.  
  
5.  Cliquez sur **Légende**.  
  
6.  Dans **Afficher dans cette légende**, sélectionnez la légende de carte à utiliser pour afficher les résultats des règles.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="ColorScale"></a> Pour modifier le contenu de l'échelle de couleurs  
  
#### <a name="to-change-the-contents-of-the-color-scale-or-a-color-legend"></a>Pour modifier le contenu de l'échelle de couleurs ou d'une légende de couleur  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle de couleur **de type d’élément cartographique>***.  
  
3.  Sélectionnez l'option de règle de couleur à utiliser. Pour afficher des éléments dans une légende de carte ou une échelle de couleurs, vous devez sélectionner l’une des options **Visualiser les données à l’aide de** \<type de règle>.  
  
4.  Dans **Champ de données**, vérifiez que les données analytiques que vous visualisez sur la couche sont sélectionnées.  
  
    > [!NOTE]  
    >  Si aucun champ ne s’affiche dans la liste déroulante, cliquez avec le bouton droit sur la couche, puis cliquez sur **Données de la couche** pour ouvrir la boîte de dialogue Propriétés des données de la couche de la carte à la page Données analytiques et vérifier que vous avez spécifié des données analytiques pour cette couche.  
  
5.  Cliquez sur **Légende**.  
  
6.  Dans **Options d'échelle de couleurs**, sélectionnez **Afficher dans l'échelle de couleurs** pour afficher les résultats de la règle dans l'échelle de couleurs. Vous pouvez spécifier cette option pour plusieurs règles de couleur.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="HideItems"></a> Pour supprimer tous les éléments d'une légende  
  
#### <a name="to-hide-items-based-on-a-rule"></a>Pour masquer des éléments en fonction d'une règle  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Légende**.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="ChangeFormatItems"></a> Pour modifier le format de contenu d'une légende  
 Définissez les options de légende pour la règle associée à la légende de la carte.  
  
#### <a name="to-change-the-format-of-content-in-a-legend"></a>Pour modifier le format de contenu d'une légende  
  
1.  En mode Conception, cliquez sur la carte jusqu'à ce que le volet Carte s'affiche.  
  
2.  Cliquez avec le bouton droit sur la couche contenant les données souhaitées, puis cliquez sur *\<Règle **de type d’élément cartographique>***.  
  
3.  Cliquez sur **Légende**.  
  
4.  **Texte de légende** affiche des mots clés qui spécifient les données qui doivent apparaître dans la légende. Utilisez des mots clés de carte et des formats personnalisés pour contrôler le format du texte de légende. Par exemple, #FROMVALUE {C2} spécifie un format de devise à deux décimales. Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Ajouter, changer ou supprimer une carte ou une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md)   
 [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md)   
 [Résoudre les problèmes liés aux rapports : rapports cartographiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)   
 [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md)  
  
  
