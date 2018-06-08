---
title: Créer une carte de documents (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c200a97b-67f2-499f-8374-3ed1ebe3f33c
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 237c1aedf1400dfa7aaa5a380030d81ed93b2f85
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-document-map-report-builder-and-ssrs"></a>Créer un explorateur de documents (Générateur de rapports et SSRS)

Un explorateur de documents fournit un ensemble de liens de navigation permettant d'accéder aux éléments de rapport dans un rapport rendu. Lorsque vous visualisez un rapport qui comprend un explorateur de document, un volet latéral distinct apparaît en regard du rapport. L'utilisateur peut cliquer sur les liens de l'explorateur de documents pour accéder à la page du rapport qui affiche l'élément en question. Les sections et les groupes du rapport sont organisés en une hiérarchie de liens. Lorsque vous cliquez sur un de ses éléments, le rapport est actualisé de façon à afficher la zone du rapport correspondant à l'élément sélectionné.  
  
 Pour ajouter des liens à l'explorateur de documents, vous affectez à la propriété **DocumentMapLabel** de l'élément de rapport du texte que vous créez ou une expression dont le résultat est le texte que vous souhaitez afficher dans l'explorateur de documents. Vous pouvez également ajouter à l'explorateur de documents des valeurs uniques pour un groupe de tables ou de matrices. Par exemple, pour un groupe se basant sur la couleur, chaque couleur unique est un lien vers la page du rapport qui affiche l'instance de groupe correspondant à cette couleur.  
  
 Vous pouvez également créer une URL d’accès à un rapport qui remplace l’affichage de l’explorateur de documents, ce qui permet d’exécuter le rapport sans afficher l’explorateur de documents, puis cliquer sur le bouton **Afficher / masquer l’explorateur de documents** dans la barre d’outils de la visionneuse de rapports pour activer/désactiver l’affichage.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="DocMapRenderExtensions"></a> Explorateurs de document et extensions de rendu  
 L'explorateur de document est destiné à être utilisé dans l'extension de rendu HTML, par exemple dans l'Aperçu ou la Visionneuse de rapports. Les autres extensions de rendu articulent un explorateur de document différemment :  
  
-   L'extension PDF restitue un plan de document sous la forme du volet Signets.  
  
-   L'extension Excel restitue un explorateur de document sous la forme d'une feuille de calcul nommée qui comprend une hiérarchie de liens. Les sections du rapport sont restituées dans des feuilles de calcul distinctes incluses avec l'explorateur de document dans le même classeur.  
  
-   Dans Word, la table des matières est un explorateur de documents.  
  
-   Les extensions Atom, TIFF, XML et CSV ignorent les explorateurs de documents.  
  
 Pour plus d’informations, consultez [Fonctionnalité interactive des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md).  
  
##  <a name="AddRptItemToMap"></a>   
#### <a name="to-add-a-report-item-to-a-document-map"></a>Pour ajouter un élément de rapport à un explorateur de documents  
  
1.  En mode Conception, sélectionnez l'élément de rapport (par exemple une table, une matrice ou une jauge) que vous souhaitez ajouter à l'explorateur de documents. Les propriétés de l'élément de rapport s'affichent dans le volet Propriétés.  
  
    > [!NOTE]  
    >  Pour sélectionner une région de données de tableau matriciel, cliquez dans n'importe quelle cellule pour afficher les handles de ligne et de colonne, puis cliquez sur le handle d'angle.  
  
2.  Dans le volet Propriétés, tapez le texte devant s'afficher dans l'explorateur de documents dans la propriété **DocumentMapLabel** ou entrez une expression dont le résultat est une étiquette. Par exemple, tapez **Graphique sur les ventes**.  
  
    > [!NOTE]  
    >  Si le volet Propriétés n’est pas visible, sous l’onglet **Affichage** , dans le groupe **Afficher/Masquer** , sélectionnez **Propriétés**.  
  
3.  Répétez les étapes 1 et 2 pour chaque élément de rapport devant apparaître dans l'explorateur de documents.  
  
4.  Cliquez sur **Exécuter**. Le rapport est exécuté et l'explorateur de documents affiche les étiquettes que vous avez créées. Cliquez sur un lien quelconque pour accéder à la page du rapport contenant l'élément en question.  

  
##  <a name="AddUniqueValuesToMap"></a>   
#### <a name="to-add-unique-group-values-to-a-document-map"></a>Pour ajouter des valeurs de groupe uniques à un explorateur de documents  
  
1.  En mode Conception, sélectionnez la table, la matrice ou la liste qui contient le groupe que vous souhaitez afficher dans l'explorateur de documents. Le volet Regroupement affiche les groupes de lignes et de colonnes.  
  
2.  Dans le volet Groupes de lignes, cliquez avec le bouton droit sur le groupe, puis cliquez sur **Modifier le groupe**. La page **Général** de la boîte de dialogue **Propriétés du groupe de tableaux matriciels** s'ouvre.  
  
3.  Cliquez sur **Avancé**.  
  
4.  Dans la zone de liste **Explorateur de documents** , tapez ou sélectionnez une expression qui correspond à l'expression de groupe.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Répétez les étapes 1 à 4 pour chaque groupe devant apparaître dans l'explorateur de documents.  
  
7.  Cliquez sur **Exécuter**. Le rapport est exécuté et l'explorateur de documents affiche les valeurs de groupe. Cliquez sur un lien quelconque pour accéder à la page du rapport contenant l'élément en question.  
  
##  <a name="HideMapWhenViewRpt"></a>   
#### <a name="to-hide-the-document-map-when-you-view-a-report"></a>Pour masquer l'explorateur de documents lors de l'affichage d'un rapport  
  
1.  Dans le Gestionnaire de rapports, accédez au rapport contenant l'explorateur de documents.  
  
     Par exemple, pour les exemples de rapport [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] , l'URL suivante spécifie le rapport nommé Product Catalog.  
  
    ```  
    http://localhost/Reports/Pages/Report.aspx?ItemPath=%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog  
    ```  
  
2.  Copiez le chemin d'accès du rapport sur le serveur. Dans l'exemple, le chemin d'accès du rapport est `%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog`.  
  
3.  Créez une nouvelle URL composée des trois éléments suivants :  
  
    -   Visionneuse de rapports sur le serveur de rapports : `http://localhost/ReportServer/Pages/ReportViewer.aspx?`  
  
    -   Nom du rapport copié à l'étape 1, par exemple : `%2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog`  
  
    -   Paramètres d'informations du périphérique qui spécifient le masquage de l'explorateur de documents : `&rs%3aCommand=Render&rc%3aFormat=HTML4.0&rc%3aDocMap=False`  
  
     L'URL ci-dessous est composée de ces trois éléments, ajoutés dans l'ordre dans lequel ils sont mentionnés.  
  
    ```  
    http://localhost/ReportServer/Pages/ReportViewer.aspx?  
    %2fAdventureWorks2012+Sample+Reports%2fProduct+Catalog  
    &rs%3aCommand=Render&rc%3aFormat=HTML4.0&rc%3aDocMap=False  
    ```  
  
     Pour utiliser cette URL, copiez-la et supprimez tous les sauts de ligne.  
  
4.  Collez l'URL dans le Gestionnaire de rapports et appuyez sur ENTRÉE. Le rapport est exécuté et l'explorateur de documents est masqué.  
  
> [!NOTE]  
>  Pour plus d'informations sur le téléchargement de ces exemples de rapports, consultez [Exemples de rapports du Générateur de rapports et du Concepteur de rapports](http://go.microsoft.com/fwlink/?LinkId=198283).  
>   
>  Pour plus d'informations, consultez « Accès URL » dans la [documentation de Reporting Services](http://go.microsoft.com/fwlink/?linkid=121312) dans la documentation en ligne de SQL Server.  


D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
