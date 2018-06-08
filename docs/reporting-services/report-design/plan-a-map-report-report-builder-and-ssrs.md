---
title: Planifier un rapport cartographique (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: dc0c27a4-7e31-4a15-a0bc-3a02479d5b02
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 0e70da5054e3a5211f98dffedcf9eafff7c4b3b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="plan-a-map-report-report-builder-and-ssrs"></a>Planifier un rapport cartographique (Générateur de rapports et SSRS)
Un rapport efficace présente des informations provoquant des actions ou stimulant des idées. Pour présenter des données analytiques, telles que les totaux des ventes ou des statistiques démographiques sur un arrière-plan géographique, vous pouvez ajouter une carte à votre rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . Une carte peut contenir plusieurs couches, où chaque couche affiche des éléments cartographiques définis par un type spécifique de données spatiales : points qui représentent des emplacements, lignes qui représentent des itinéraires ou polygones qui représentent des zones. Vous pouvez associer vos données analytiques aux éléments cartographiques sur chaque couche.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="MapPurpose"></a> Spécifier l'objectif de la carte  
 Une conception de rapport efficace fournit des informations permettant aux utilisateurs de prendre des mesures pour résoudre les éventuels problèmes. Pour créer un affichage de carte efficace et facilement compréhensible, déterminez en premier lieu les questions auxquelles la carte doit aider à répondre. Par exemple, vous pouvez visualiser les types suivants de données pour identifier des possibilités commerciales sur une carte :  
  
-   Chiffres des ventes relatifs pour chaque magasin.  
  
-   Chiffres des ventes catégorisés par caractéristique démographique de la clientèle, basée sur l'emplacement des clients par rapport aux emplacements des magasins.  
  
-   Chiffres des ventes comparatifs ou autres données financières par secteur de vente.  
  
-   Chiffres des ventes comparatifs pour différentes stratégies de rabais dans plusieurs magasins.  
  
 Après avoir identifié l'objectif de l'affichage de carte, vous devez analyser les données dont vous avez besoin. Les données analytiques proviennent de datasets du rapport. Les données d'emplacement proviennent de sources de données spatiales que vous devez spécifier.  
  
##  <a name="Data"></a> Spécifier les données spatiales et analytiques  
 Vous devez spécifier de quelles données spatiales et analytiques vous avez besoin.  
  
 Les données analytiques proviennent d'un dataset de rapport, des exemples de données incluses avec une carte de la bibliothèque de cartes ou de données analytiques incluses avec les données spatiales dans un fichier de forme ESRI.  
  
 Les données spatiales sont fournies sous trois formes : points, lignes et polygones.  
  
-   **Points.** Les points spécifient des emplacements, par exemple une ville ou l'adresse d'un magasin, d'un restaurant ou d'un palais des congrès. Pour chaque emplacement que vous souhaitez afficher sur une carte, vous devez fournir les données spatiales correspondantes. Après avoir ajouté des points à une carte, vous pouvez afficher un marqueur à l'emplacement du point et faire varier le type de marqueur, sa taille et sa couleur.  
  
-   **Lignes.** Les lignes spécifient des chemins ou des itinéraires, par exemple des itinéraires de livraison ou des trajectoires de vol. Après avoir ajouté une ligne à une carte, vous pouvez faire varier la couleur et la largeur de la ligne.  
  
-   **Polygones.** Les polygones spécifient des zones, par exemple des régions, secteurs, pays, états, provinces, comtés, villes ou zones urbaines, codes postaux, circonscriptions téléphoniques ou districts de recensement. Après avoir ajouté un polygone à une carte, en plus d'afficher le périmètre du polygone, vous pouvez afficher un marqueur au point central de la zone du polygone.  
  
 Après avoir identifié les données spatiales nécessaires, vous devez rechercher la source de données spatiales appropriée.  
  
### <a name="find-a-source-for-spatial-data"></a>Rechercher une source de données spatiales  
 Pour rechercher des données spatiales à utiliser dans votre carte, vous pouvez utiliser les sources suivantes :  
  
-   Fichiers de forme ESRI, notamment des fichiers de forme disponibles au public que vous pouvez rechercher sur Internet ;  
  
-   données spatiales provenant de sources de données spatiales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   cartes de rapports de la bibliothèque de cartes ;  
  
-   sites tiers offrant des données spatiales sous forme de fichiers de forme ESRI ou de données spatiales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   mosaïques Bing, qui fournissent un arrière-plan pour la vue cartographique. Pour afficher des mosaïques dans une carte, le serveur de rapports doit être configuré pour prendre en charge les Services Web Bing Maps.  
  
 Pour plus d'informations, consultez « Où puis-je obtenir des fichiers de forme ESRI ? » dans [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md).  
  
 Les données spatiales peuvent être politiquement sensibles et/ou protégées par copyright. Vérifiez les conditions d'utilisation et les déclarations de confidentialité concernant les sources de données spatiales pour savoir comment vous pouvez utiliser ces données spatiales dans votre rapport.  
  
 Après avoir localisé les données souhaitées, vous pouvez les incorporer dans la définition de rapport ou récupérer les données dynamiquement lors du traitement du rapport. Pour plus d'informations, consultez [Trouver un compromis entre la taille de la définition de rapport et le temps de traitement du rapport](#Embedding) plus loin dans cette rubrique.  
  
### <a name="determine-the-spatial-data-and-the-spatial-data-match-fields"></a>Déterminer les données spatiales et les champs de correspondance des données spatiales  
 Pour afficher des données analytiques sur une carte et pour faire varier la taille, la couleur ou le type de marqueur, vous devez spécifier des champs qui mettent en relation les données spatiales et les données analytiques.  
  
 Les données spatiales doivent contenir les champs suivants :  
  
-   **Spatial data.** Champ de données spatiales comprenant les jeux de coordonnées qui définissent chaque point, ligne ou polygone.  
  
-   **Champs de correspondance.** Un ou plusieurs champs qui identifient de manière unique chaque champ de données spatiales. Par exemple, pour un point d'emplacement de magasin, vous souhaiterez peut-être utiliser le nom du magasin. Si le nom de magasin n'est pas unique dans les données spatiales, vous pouvez inclure le nom de la ville avec celui du magasin.  
  
 Les champs de correspondance sont utilisés pour mettre en relation des données spatiales et des données analytiques.  
  
### <a name="determine-the-analytical-data-and-the-analytical-data-match-fields"></a>Déterminer les données analytiques et les champs de correspondance des données analytiques  
 Après avoir identifié les données spatiales, vous devez identifier les données analytiques. Les données analytiques peuvent provenir de l'une des sources suivantes :  
  
-   un dataset de rapport existant. Les champs sont spécifiés en tant qu'expressions de champ simples, par exemple, [Sales] ou =Fields!Sales.Value ;  
  
-   des champs de données fournis par la source de données spatiales. Les champs sont spécifiés en tant que mots clés commençant par #, suivis du nom de champ de la source de données spatiales.  
  
 Vous devez ensuite déterminer le nom des champs de correspondance :  
  
-   Champs de correspondance. Un ou plusieurs champs spécifiant les mêmes informations que les champs de correspondance des données spatiales pour générer une relation entre les données spatiales et les données analytiques. Les champs de correspondance doivent correspondre en termes de type de données aussi bien que de mise en forme.  
  
 Lorsque vous avez identifié la source de données spatiales, les données spatiales, la source de données analytiques, les données analytiques et les champs de correspondance, vous êtes prêt à décider quel type de carte vous souhaitez ajouter à votre rapport.  
  
##  <a name="MapType"></a> Choisir un type de carte  
 Lorsque vous exécutez l'Assistant Carte, vous ajoutez une carte et la première couche de carte à votre rapport. L'Assistant vous permet d'ajouter l'un des types de cartes suivants à votre rapport :  
  
-   une carte simple qui affiche des emplacements sans données analytiques associées ;  
  
-   une carte contenant une valeur analytique associée à chaque élément cartographique ; par exemple, le chiffre total des ventes pour chaque emplacement de magasin ;  
  
-   une carte contenant plusieurs valeurs analytiques associées à un élément cartographique ; par exemple, le nombre de clients et le chiffre total des ventes pour chaque emplacement de magasin.  
  
 Le tableau ci-dessous décrit chaque type de carte. Tous les types de cartes vous permettent de sélectionner un thème pour contrôler la palette, le style de bordure et la police, et afficher des étiquettes.  
  
|Icône d'Assistant|Style de couche|Type de couche|Description et options|  
|-----------------|-----------------|----------------|-----------------------------|  
|![rs_MapType_Polygon_Basic](../../reporting-services/report-design/media/rs-maptype-polygon-basic.gif "rs_MapType_Polygon_Basic")|Carte simple|Polygon|Carte qui affiche des zones uniquement, par exemple, des secteurs de vente.<br /><br /> Options : faites varier la couleur par palette ou utilisez une couleur unique. Une palette est un jeu prédéfini de couleurs. Lorsque toutes les couleurs d'une palette ont été attribuées, des nuances des couleurs sont attribuées.|  
|![rs_MapType_Polygon_ColorAnalytical](../../reporting-services/report-design/media/rs-maptype-polygon-coloranalytical.gif "rs_MapType_Polygon_ColorAnalytical")|Carte analytique en couleur|Polygon|Carte affichant des données analytiques en utilisant plusieurs couleurs, par exemple, pour afficher les chiffres des ventes par zone.|  
|![rs_MapType_Polygon_Bubble](../../reporting-services/report-design/media/rs-maptype-polygon-bubble.gif "rs_MapType_Polygon_Bubble")|Carte à bulles|Polygone|Carte affichant des données analytiques en faisant varier la taille des bulles centrées sur les zones, par exemple pour afficher les chiffres des ventes par zone.<br /><br /> Options : faites varier les couleurs de zone en fonction d'un deuxième champ analytique et spécifiez des règles de couleur.|  
|![rs_MapType_Line_Basic](../../reporting-services/report-design/media/rs-maptype-line-basic.gif "rs_MapType_Line_Basic")|Carte linéaire simple|Ligne|Carte qui affiche des lignes uniquement, par exemple des itinéraires de livraison.<br /><br /> Options : faites varier la couleur par palette ou utilisez une couleur unique.|  
|![rs_MapType_Line_Analytical](../../reporting-services/report-design/media/rs-maptype-line-analytical.gif "rs_MapType_Line_Analytical")|Carte linéaire analytique|Ligne|Carte faisant varier la couleur et la largeur des lignes, par exemple pour afficher le nombre de paquets livrés et les mesures de ponctualité par itinéraire.<br /><br /> Options : faites varier la largeur de ligne en fonction d'un champ analytique et la couleur de ligne en fonction d'un deuxième champ analytique, et spécifiez des règles de couleur.|  
|![rs_MapType_Marker_Basic](../../reporting-services/report-design/media/rs-maptype-marker-basic.gif "rs_MapType_Marker_Basic")|carte à marqueurs simple|Point|Carte affichant un marqueur à chaque emplacement, par exemple pour indiquer des villes.<br /><br /> Options : faites varier la couleur par palette ou utilisez une couleur unique, et modifiez le style de marqueur.|  
|![rs_MapType_Marker_Bubble](../../reporting-services/report-design/media/rs-maptype-marker-bubble.gif "rs_MapType_Marker_Bubble")|Carte à marqueurs à bulles|Point|Carte affichant une bulle pour chaque emplacement et faisant varier la taille des bulles en fonction d'un champ de données analytiques, par exemple pour indiquer les chiffres des ventes par ville.<br /><br /> Options : faites varier la couleur des bulles en fonction d'un deuxième champ analytique et spécifiez des règles de couleur.|  
|![rs_MapType_Marker_Analytical](../../reporting-services/report-design/media/rs-maptype-marker-analytical.gif "rs_MapType_Marker_Analytical")|Carte à marqueurs analytique|Point|Carte affichant un marqueur à chaque emplacement et faisant varier la couleur, la taille et le type de marqueur en fonction de données analytiques, par exemple pour indiquer les produits produisant le meilleur chiffre d'affaires, la plage de bénéfices et la stratégie de remise.<br /><br /> Options : faites varier le type de marqueur en fonction d'un champ analytique, la taille de marqueur en fonction d'un deuxième champ analytique, la couleur de marqueur en fonction d'un troisième et spécifiez des règles de couleur.|  
  
 Après avoir ajouté une carte à l'aide de l'Assistant Carte, vous pouvez créer des couches supplémentaires ou modifier des options pour une couche à l'aide de l'Assistant Couche. Pour plus d’informations sur les Assistants, consultez [Assistant Carte et Assistant Couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/map-wizard-and-map-layer-wizard-report-builder-and-ssrs.md).  
  
 Vous pouvez personnaliser les options d'affichage ou de données individuellement pour chaque couche. Pour plus d’informations sur la personnalisation d’une carte après avoir exécuté un Assistant, consultez [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
##  <a name="Legend"></a> Planifier les légendes  
 Pour aider vos utilisateurs à interpréter une carte, vous pouvez ajouter plusieurs légendes de carte, une échelle de couleurs et une échelle des distances. Lorsque vous concevez une carte, planifiez l'emplacement où vous souhaitez que les légendes s'affichent. Vous pouvez spécifier les informations suivantes à propos de chaque légende :  
  
-   **Emplacement de la légende.** Par exemple, les légendes peuvent être affichées à l'intérieur ou à l'extérieur de la fenêtre d'affichage, et dans 12 emplacements discrets par rapport à la fenêtre d'affichage.  
  
-   **Styles de légende**. Par exemple, vous pouvez spécifier le style de police, le style de bordure, la ligne de séparateur et les propriétés de remplissage.  
  
-   **Titre de la légende.** Par exemple, vous pouvez spécifier le texte du titre, et contrôler indépendamment s'il faut afficher un titre pour une légende de la carte ou pour l'échelle de couleurs.  
  
-   **Mise en page de la légende de carte.** Par exemple, les légendes de carte peuvent être affichées comme tables verticales ou horizontales.  
  
 Le contenu de la légende est créé automatiquement pendant le traitement des rapports en fonction des options de règle définies pour chaque couche.  
  
 Par défaut,toutes les couches affichent les résultats des règles dans la première légende de carte. Vous pouvez créer plusieurs légendes, puis, pour chaque règle, déterminer quelle légende utiliser pour afficher les résultats.  
  
 Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md) et [Modifier les légendes de carte, l’échelle de couleurs et les règles associées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="Embedding"></a> Trouver un compromis entre la taille de la définition de rapport et le temps de traitement du rapport  
 Une conception de rapport efficace en matière de cartes nécessite de trouver un compromis entre les options qui contrôlent les performances du rapport et la taille de la définition de rapport. Les éléments cartographiques basés sur des données spatiales, ou les mosaïques Bing, peuvent être statiques et incorporés dans la définition de rapport, ou dynamiques et créés chaque fois que le rapport est traité. Vous devez évaluer les avantages et inconvénients présentés par des données cartographiques statiques et dynamiques et rechercher un compromis approprié pour votre scénario. Considérez les points suivants lorsque vous prenez cette décision :  
  
-   Les éléments cartographiques incorporés peuvent augmenter considérablement la taille de la définition de rapport, mais ils réduisent le temps nécessaire pour afficher la carte dans le rapport. Votre serveur de rapports peut être soumis à des limites de taille que vous devez prendre en considération.  
  
-   La définition de rapport spécifie des limites en termes de nombre de points de données spatiales pouvant être traités ainsi qu'une valeur séparée spécifiant le nombre d'éléments cartographiques pouvant être inclus dans la définition de rapport.  
  
-   Les éléments cartographiques dynamiques réduisent la taille de la définition de rapport, mais augmentent le temps requis pour traiter et restituer la carte.  
  
-   Lorsque la source de données spatiales est située sur votre ordinateur, les éléments cartographiques sont toujours incorporés dans la définition de rapport. Cela inclut des données spatiales de la bibliothèque de cartes ou de fichiers de forme ESRI installés localement.  
  
 Pour utiliser des données spatiales dynamiques, la source de données spatiales doit être sur le serveur de rapports. Lorsque les rapports sont conçus dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], les sources de données spatiales peuvent être ajoutées à un projet et publiées sur le serveur de rapports avec la définition de rapport. Si vous utilisez le Générateur de rapports pour concevoir des rapports, vous devez télécharger en premier lieu les données spatiales sur le serveur de rapports, puis, dans l'Assistant ou dans les propriétés de couche, spécifier cette source de données spatiales pour la couche.  
  
## <a name="see-also"></a> Voir aussi  
 [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md)   
 [Didacticiel : rapport cartographique &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-map-report-report-builder.md)   
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Résoudre les problèmes liés aux rapports : rapports cartographiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)  
  
  
