---
title: Conseils de création de rapport (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: c1490ff0-5b8a-43c1-8d22-e459395db4f6
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 41cedb83e24fecb71089487df2a868b104a1783c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-design-tips-report-builder-and-ssrs"></a>Conseils de création de rapport (Générateur de rapports et SSRS)
  Utilisez les conseils suivants pour concevoir vos rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="DesigningReports"></a> Conception de rapports  
  
-   Un rapport bien conçu véhicule des informations qui aboutissent à une action. Identifiez les questions auxquelles le rapport permet de répondre. Gardez ces questions à l'esprit pendant que vous concevez le rapport.  
  
-   Pour concevoir des visualisations de données efficaces, pensez à la façon d'afficher des informations qui sont faciles à comprendre pour l'utilisateur du rapport. Choisissez une région de données qui correspond bien aux données que vous voulez visualiser. Par exemple, un graphique véhicule efficacement des informations de résumé et d'agrégation mieux qu'une table qui couvre de nombreuses pages d'informations détaillées. Vous pouvez visualiser les données d'un dataset dans n'importe quelle région de données, ce qui inclut des graphiques, cartes, indicateurs, graphiques sparkline, barres de données et données tabulaires dans différentes dispositions sous forme de grille selon un tableau matriciel.  
  
-   Si vous projetez de remettre le rapport sous un format d'exportation spécifique, testez le format d'exportation tôt dans votre conception. La prise en charge des fonctionnalités varie selon le convertisseur que vous choisissez.  
  
-   Si vous envisagez de remettre le rapport sous la forme d'un abonnement, testez l'abonnement tôt dans votre conception. La prise en charge des paramètres varie selon l'abonnement que vous créez.  
  
-   Lorsque vous générez des dispositions complexes, générez la disposition par étapes. Vous pouvez utiliser des rectangles comme conteneurs pour organiser des éléments de rapport. Vous pouvez générer des régions de données directement sur l'aire de conception pour optimiser la zone de travail, puis, dès que vous en avez terminé une, la faire glisser jusqu'au conteneur rectangle. En utilisant des rectangles en tant que conteneurs, vous pouvez positionner tout son contenu en une étape. Les rectangles permettent également de contrôler la façon dont les éléments de rapport sont rendus sur chaque page.  
  
-   Pour clarifier le rapport, envisagez d'utiliser la visibilité conditionnelle pour des éléments de rapport spécifiques et laissez l'utilisateur choisir d'afficher, ou non, les éléments. Vous pouvez définir la visibilité en fonction d'un paramètre ou d'une bascule de zone de texte. Vous pouvez ajouter de manière conditionnelle des zones de texte masquées pour afficher des résultats d'expression intermédiaires. Lorsqu'un rapport affiche des données inattendues, vous pouvez afficher ces résultats intermédiaires pour déboguer les expressions.  
  
-   Lorsque vous utilisez des éléments imbriqués dans des cellules ou des rectangles de tableau matriciel, vous pouvez définir des couleurs d'arrière-plan du conteneur et des éléments contenus. Par défaut, la couleur d'arrière-plan est **Aucune couleur**. Les éléments avec une couleur d'arrière-plan spécifique transparaissent à travers les éléments dont la couleur d'arrière-plan a pour valeur **Aucune couleur**. Cette technique peut vous permettre de sélectionner l'élément correct pour définir les propriétés d'affichage, telles que la visibilité de bordure sur les cellules de tableau matriciel.  
  
 Pour plus d’informations sur les éléments à prendre en compte au fil de la création de votre rapport, consultez [Planification d’un rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md).  
  
##  <a name="NamingConventions"></a> Conventions d'affectation des noms pour les rapports, les sources de données et les datasets  
  
-   Utilisez les conventions d'affectation des noms pour les sources de données et datasets qui documentent la source de données.  
  
    1.  **Sources de données.** Si vous ne souhaitez pas utiliser une base de données ou un serveur réel pour des raisons de sécurité, utilisez un alias qui indique à l'utilisateur la source de données.  
  
    2.  **DataSets.** Utilisez un nom qui indique la source de données sur lequel il est basé.  
  
    3.  **Régions de données.** Indique le type de région de données et les données qui s'affichent. Les noms de région de données sont utiles dans les scénarios suivants :  
  
        1.  **Région de données en tant que partie du rapport.** Lorsque les auteurs de rapport parcourent la Bibliothèque de parties de rapports, un nom descriptif les aide à rechercher les parties de rapport qu'ils recherchent.  
  
        2.  **Région de données en tant que flux de données.** Avec les autorisations appropriées, un lecteur du rapport peut créer un flux de données ATOM d'une région de données.  
  
-   Utilisez des traits de soulignement à la place des espaces dans les noms de rapports. Si vous téléchargez un rapport à partir d’un portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , les espaces sont remplacés par les traits de soulignement. Si vous utilisez la fonctionnalité de téléchargement pour enregistrer des rapports en local, puis les incluez dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], l'utilisation de traits de soulignement permet d'assurer l'exactitude des dépendances de rapports pour les sous-rapports et liens d'extraction.  
  
##  <a name="Data"></a> Utilisation des données  
  
-   La première étape consiste à faire en sorte que toutes les données que vous souhaitez utiliser s'affichent dans le volet des données de rapport. Lorsque vous affinez les questions auxquelles la conception du rapport permet d'apporter des réponses, pensez à la façon de limiter les données dans les datasets du rapport au strict nécessaire.  
  
-   En général, incluez uniquement les données que vous afficherez dans un rapport. Utilisez des variables de requête dans vos requêtes de dataset pour permettre à l'utilisateur de choisir les données qu'il veut afficher dans le rapport. Si vous créez des datasets partagés, spécifiez des filtres basés sur des paramètres de rapport afin de fournir les mêmes fonctionnalités.  
  
-   Si vous êtes un générateur de requêtes expérimenté, comprenez que, pour des quantités intermédiaires de données, vous pouvez regrouper des données dans le rapport, et non dans la requête. Si vous effectuez tous vos regroupements dans la requête, le rapport a tendance à être une présentation du jeu de résultats de la requête. En revanche, pour afficher des valeurs agrégées pour un grand nombre de données sur un graphique ou une matrice, il est inutile d'inclure des données de détail.  
  
-   Selon vos besoins, vous pouvez afficher des noms et des emplacements de sources de données de rapport, le texte de la commande de requête de dataset et des valeurs de paramètres dans le rapport. La première question que les nouveaux utilisateurs se posent généralement concerne l'origine des données. Pour clarifier le rapport, vous pouvez masquer de façon conditionnelle des zones de texte avec ce type d'informations et laisser les utilisateurs choisir ou non de les visualiser. Essayez d'ajouter ces informations dans la dernière page du rapport. Définissez la visibilité de la zone de texte selon un paramètre que l'utilisateur peut modifier.  
  
##  <a name="DesignSurface"></a> Interaction avec l'aire de conception du rapport  
 L'aire de conception du rapport n'offre pas le mode d'affichage WYSIWYG (« tel écrit, tel écran »). Lorsque vous placez des éléments de rapport sur l'aire de conception, leur emplacement relatif affecte la façon dont les éléments s'affichent dans la page de rapport rendue. Les espaces blancs sont conservés.  
  
-   Utilisez des lignes d'alignement et des boutons de disposition pour aligner et organiser des éléments sur l'aire de conception du rapport. Par exemple, vous pouvez aligner les parties supérieures ou les bords d'éléments sélectionnés, étendre un élément pour que sa taille corresponde à la taille d'un autre élément ou ajuster l'espacement entre des éléments.  
  
-   Utilisez les touches de direction pour ajuster la position et la taille des éléments sélectionnés sur l'aire de conception. Par exemple, les combinaisons de touches suivantes sont très utiles :  
  
    -   **Touches de direction** Déplacer l'élément de rapport sélectionné.  
  
    -   **Ctrl+touches de direction** Repositionner l’élément de rapport sélectionné.  
  
    -   **Ctrl+Maj+touches de direction** Augmenter ou diminuer la taille de l’élément de rapport sélectionné.  
  
-   Pour ajouter un élément à un rectangle, utilisez l'extrémité supérieure gauche du pointeur de la souris pour pointer sur l'emplacement initial de l'élément dans le conteneur rectangle. Utilisez les raccourcis clavier pour positionner les objets sélectionnés. Le rectangle s'étend automatiquement pour adapter sa taille à celle des éléments contenus.  
  
-   Pour ajouter plusieurs éléments à une cellule de tableau matriciel, commencez par ajouter un rectangle, puis ajoutez les éléments.  
  
     Par défaut, chaque cellule de tableau matriciel contient une zone de texte. Lorsque vous ajoutez un rectangle à une cellule, le rectangle remplace la zone de texte. Par exemple, placez des indicateurs imbriqués dans un rectangle dans une cellule de tableau matriciel pour mieux contrôler le développement de la taille d'un graphique ou d'un indicateur à mesure que vous modifiez la hauteur de la ligne dans laquelle se trouve la cellule.  
  
-   Utilisez le contrôle **Zoom** pour ajuster votre affichage de l'aire de conception. Vous pouvez travailler avec la page entière ou des sections plus petites de la page.  
  
-   Pour faire glisser des champs du volet des données de rapport vers le volet de regroupement, évitez de faire glisser le champ à travers d'autres éléments de rapport sur l'aire de conception, car cela sélectionne les autres éléments et désélectionne la région de données de tableau matriciel. Faites glisser le champ en bas du volet des données de rapport, puis horizontalement jusqu'au volet de regroupement.  
  
###  <a name="Selecting"></a> Sélection d'éléments  
 Pour sélectionner l'objet que vous souhaitez sur l'aire de conception du rapport, utilisez la touche Échap, le menu contextuel accessible par un clic droit, le volet Propriétés et le volet de regroupement.  
  
-   -   Appuyez sur Échap pour parcourir la pile d'éléments de rapport qui occupent le même espace sur l'aire de conception.  
  
    -   Sur certains éléments de rapport, essayez utiliser le menu contextuel accessible par un clic droit pour sélectionner l'élément de rapport ou la partie de l'élément de rapport que vous souhaitez.  
  
    -   Le volet Propriétés affiche les propriétés de la sélection en cours.  
  
    -   Pour utiliser des groupes de lignes et des groupes de colonnes dans une région de données de tableau matriciel, sélectionnez le groupe dans le volet de regroupement.  
  
 Dans le Concepteur de rapports dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], vous pouvez sélectionner dans la liste déroulante des objets de la barre d'outils du volet Propriétés ou de la vue hiérarchique des éléments de rapport dans la fenêtre Structure du document. Vous pouvez sélectionner des éléments dans ce volet et voir quel élément est sélectionné sur l'aire de conception. Pour ouvrir le volet Structure du document, dans le menu **Affichage** , pointez sur **Autres fenêtres**, puis cliquez sur **Structure du document**.  
  
##  <a name="ReportItems"></a> Utilisation des types spécifiques d'éléments de rapport  
  
###  <a name="Parameters"></a> Utilisation des paramètres  
  
-   L'objectif principal des paramètres de rapport est de filtrer les données au niveau de la source de données, et de récupérer uniquement les données nécessaires pour l'objectif du rapport.  
  
-   Pour les paramètres de rapport, recherchez un équilibre entre l'activation de l'interactivité et l'aide à un utilisateur pour qu'il obtienne les résultats souhaités. Par exemple, vous pouvez définir des valeurs par défaut pour un paramètre selon des valeurs que vous considérez comme étant populaires.  
  
###  <a name="Text"></a> Utilisation du texte  
  
-   Lorsque vous collez plusieurs lignes dans une zone de texte, le texte est ajouté sous la forme d'une séquence de texte unique. Chaque séquence de texte peut être mise en forme uniquement comme une unité. Pour mettre en forme chaque ligne indépendamment, insérez une nouvelle ligne en appuyant sur la touche Retour dans la séquence de texte, si nécessaire. Vous pouvez ensuite appliquer une mise en forme et des styles à chaque ligne de texte indépendante dans la zone de texte.  
  
-   Vous pouvez définir des propriétés de format et des actions sur une zone de texte ou sur le texte de l'espace réservé dans la zone de texte. S'il y a une seule ligne de texte, il est plus efficace de définir les propriétés sur la zone de texte plutôt que sur le texte.  
  
###  <a name="Expressions"></a> Utilisation des expressions  
  
-   Vous devez comprendre les formats des expressions simples et complexes. Vous pouvez taper un format d'expression simple directement dans des zones de texte, dans des propriétés dans le volet Propriétés ou dans des emplacements de boîtes de dialogue qui acceptent une expression. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
-   Lorsque vous créez une expression, il est recommandé de créer chaque partie indépendamment et de vérifier sa valeur. Vous pouvez ensuite combiner toutes les parties dans une expression finale. Une technique utile consiste à ajouter une zone de texte dans une cellule de matrice, afficher chaque partie de l'expression et définir une visibilité conditionnelle sur la zone de texte. Pour contrôler le style de bordure et la couleur lorsque la zone de texte est masquée, placez d'abord la zone de texte dans un rectangle, puis définissez le style de bordure et la couleur du rectangle pour qu'ils correspondent à la matrice.  
  
###  <a name="Indicators"></a> Utilisation des indicateurs  
  
-   Par défaut, un indicateur affiche au moins trois états. Après avoir ajouté un indicateur à un rapport, vous pouvez le configurer en ajoutant ou en supprimant des états. Pour un affichage plus aisé par vos utilisateurs, choisissez un indicateur qui varie en termes de couleur et de forme.  
  
##  <a name="Rendering"></a> Contrôle du rendu des éléments de rapport sur la page du rapport  
  
-   Sur l'aire de conception du rapport, les éléments de rapport s'agrandissent de façon à héberger le contenu à partir du dataset, de l'expression, du sous-rapport ou du texte associé.  
  
    -   Lorsque vous positionnez un élément dans la page de rapport, la distance entre l'élément et tous les éléments qui commencent à sa droite devient la distance minimale qui doit être maintenue lorsqu'un élément de rapport s'agrandit horizontalement. De la même façon, la distance entre un élément et l'élément au-dessus de lui devient une distance minimale qui doit être maintenue lorsque l'élément du haut s'agrandit verticalement.  
  
    -   Un élément dans un rapport s'agrandit pour contenir ses données et pousse les éléments homologues (éléments dans le même conteneur parent) en appliquant les règles suivantes :  
  
    -   Chaque élément est déplacé vers le bas pour conserver l'espace minimal entre lui-même et les éléments qui se terminent au-dessus de lui.  
  
    -   Chaque élément est déplacé vers la droite pour conserver l'espace minimal entre lui-même et les éléments qui se terminent à sa gauche. Pour les systèmes qui utilisent une disposition de droite à gauche, chaque élément est déplacé vers la gauche pour conserver l'espace minimal entre lui-même et les éléments qui se terminent à sa droite.  
  
    -   Les conteneurs s'étendent pour s'adapter au développement des éléments enfants. Pour un élément sélectionné, dans le volet Propriétés, la propriété Parent identifie le conteneur de l'élément. Vous pouvez également utiliser le volet Structure du document pour consulter la hiérarchie des relations contenant-contenu des éléments de rapport.  
  
    -   La barre d'outils de **Disposition** fournit plusieurs boutons qui permettent d'aligner les bords, les centres et l'espacement des éléments de rapport. Pour activer la barre d'outils de **disposition** , dans le menu **Affichage** , pointez sur **Barres d'outils**, puis cliquez sur **Disposition**.  
  
-   Si vous envisagez d'enregistrer le rapport en tant que fichier .pdf, la largeur du rapport doit être définie explicitement sur une valeur qui vous donne les résultats souhaités dans le format de fichier d'exportation. Par exemple, définissez la largeur de page du rapport sur exactement 7,9375 pouces et les marges droite et gauche sur 0,5 pouce.  
  
-   Utilisez **Page** et **Mise en page** dans la barre d’outils de la visionneuse de rapports pour effectuer le rendu d’un rapport dans un affichage compatible avec l’impression. Pour supprimer les pages horizontales inutiles, procédez comme suit :  
  
    1.  Supprimez tout l'espace blanc superflu entre les régions de données et sur les contours du rapport.  
  
    2.  Réduisez des marges de page dans la boîte de dialogue **Propriétés du rapport** .  
  
    3.  Utilisez des **Rectangles** comme conteneurs pour contrôler le rendu des éléments de rapport.  
  
    4.  Dans les en-têtes de colonnes, modifiez la propriété de zone de texte WritingMode pour utiliser le texte vertical.  
  
 La combinaison de ce comportement, des propriétés de hauteur et de largeur des éléments de rapport, de la taille du corps du rapport, de la définition de hauteur de page et de largeur de page, des paramètres de marge du rapport parent, ainsi que de la prise en charge de la pagination spécifique au convertisseur détermine quels éléments du rapport s'ajustent dans une page rendue. Pour plus d’informations, voir [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Générateur de rapports dans SQL Server 2016](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)   
 [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)   
 [Didacticiels du Générateur de rapports](../../reporting-services/report-builder-tutorials.md)  
  
  
